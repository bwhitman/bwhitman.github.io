---
title: "Broadcasting your logs with RabbitMQ and Python"
date: "2009-07-17"
---

**Update** – thanks to Matthias at LShift I’ve updated this code so it is much better and runs without crashing :)

You’ve got sixteen computers running all spitting out various messages. Some of them are useful, some you should ignore, and then there’s errors– you need to know when they happen. But what do you do? Spawn sixteen ssh machine tail -f /var/logs? Crazy time. No, what you want to do is forward your logging system to a message queue where anyone can subscribe to it and filter out what they want. Let me show you how.

We’re using RabbitMQ here for lots of other things and we spent some time figuring out how to make a “pubsub” or “forward and delete” style queue. In this model, messages sent to an exchange get forwarded to any queue that is listening and is promptly deleted. If you’re not listening, you don’t get to see it.

You can do this via a “fanout” exchange type in AMQP. Fanout is a special exchange type that forwards all messages sent to the exchange to all connected queues. So, all we have to do is setup the fanout, and then each consumer/listener that wants to listen in should register a unique queue name (we use UUIDs) and they immediately start getting all the messages (code initially based on the [queueing-playground samples](http://github.com/myelin/queueing-playground/tree/master)):

import amqplib.client\_0\_8 as amqp
import pickle, os, uuid, logging
from time import time, strftime, localtime

my\_exchange = "broadcast.fanout"
my\_uuid = str(uuid.uuid1())

def setup\_amqp(mode, queue\_name):
    conn = amqp.Connection(hostname,userid="guest",password="guest",ssl=False)
    ch = conn.channel()
    ch.access\_request('/data', active=True, read=(mode=='r'), write=(mode=='w'))
    ch.exchange\_declare(my\_exchange, 'fanout', durable=False, auto\_delete=False)
    if(mode=='r'):
        qname, n\_msgs, n\_consumers = ch.queue\_declare(queue\_name, durable=False, exclusive=True, auto\_delete=True)
        ch.queue\_bind(queue\_name, my\_exchange, queue\_name)
    return ch

class Producer():
    def \_\_init\_\_(self):
        self.ch = setup\_amqp('w', queue\_name="logger")

    def message(self, message):
        self.ch.basic\_publish(amqp.Message(pickle.dumps(message)), my\_exchange, "")

class Consumer():
    def \_\_init\_\_(self, callback\_function=None):
        self.ch = setup\_amqp('r',queue\_name="logger\_"+my\_uuid)
        self.callback\_function = callback\_function
    
    def callback(self, msg):
        message = pickle.loads(msg.body)
        if(self.callback\_function):
            self.callback\_function(message)

    def consume\_forever(self):
        self.ch.basic\_consume("logger\_"+my\_uuid, callback=self.callback, no\_ack=True)
        while self.ch.callbacks:
            self.ch.wait()

    def close(self):
        self.ch.close()

This “just works” – any message the Producer sends goes to all connected Consumers(). Each consumer registers a new unique queue name to the existing “broadcast.fanout” exchange. You set your callback function in Consumer’s init, and in there you do your magic – filtering for messages you want, adding up stuff, timing, printing, saving to disk.

def print\_message(message):
    print str(message)

def main():
    c = Consumer(callback\_function=print\_message)
    c.consume\_forever()

But what about standard logging? Well, it’s easy to hook this up to Python’s logging module by making a new logging.Handler:

class BroadcastLogHandler(logging.Handler):
    def \_\_init\_\_(self):
        self.broadcaster = Producer()
        self.level = 0
        self.filters = \[\]
        self.lock = 0
        self.machine = os.uname()\[1\]

    def emit(self, record):
        # Send the log message to the broadcast queue
        message = {"source":"logger","machine":self.machine, "message":record.msg % record.args, "level":record.levelname, "pathname":record.pathname, "lineno":record.lineno, "exception":record.exc\_info}
        self.broadcaster.message(message)

def StreamAndBroadcastHandler(level=logging.DEBUG):
    "Factory function for a logging.StreamHandler instance connected to your namespace."
    logger = logging.getLogger('your-namespace')
    logger.setLevel(level)
    handler\_messages = BroadcastLogHandler()
    logger.addHandler(handler\_messages)

In this setup you call the factory function StreamAndBroadcastHandler() in the code you want to log to both screen and message queue.

import logging
StreamAndBroadcastHandler(logging.INFO)
logger = logging.getLogger("your-namespace")
logger.info("yo baby")
logger.error("o noes")

Logs in this setup will go to both screen and the messager; anyone listening will see it immediately. Our emit function packages up all the log metadata along with the machine name so you can filter on which machine sent out the message.
