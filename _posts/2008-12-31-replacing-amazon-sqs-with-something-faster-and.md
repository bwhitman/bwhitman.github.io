---
title: "Replacing Amazon SQS with something faster and cheaper"
date: "2008-12-31"
---

![](images/RabbitMQLogo.png)

Did you know that [SQS](http://aws.amazon.com/sqs/) costs a dollar per million messages? That’s sent or received, so really it’s 2 bucks a million. Another thing with SQS: it’s slow. Even with 200 connections adding 10K messages takes _minutes_. \[Actual #: 10000 100 byte strings from an EC2 instance takes 200 seconds.\]

So I played with [RabbitMQ](http://www.rabbitmq.com/). [Phil J](http://whirlycott.com) told me about it. Normally when he says something I just do it, but this time I wanted at least to prove to myself that he was right. 

If you want to replace SQS with an AMQP like RabbitMQ, here’s what you do. You make a 'direct exchange’ with the routing key being the queue name. You bind the queue to the exchange with the routing key and messages sent to the exchange are forwarded to the relevant queues. Consumers then get directly from the queue and send an “ack” when they are done, which pops it off the queue. There’s two consuming methods, one is called basic\_consume which appears to block all other consumers from reading the queue (don’t know why) and one is basic\_get which acts like SQS in that multiple consumers can check the queue and send acks independently of the get. Other exchange types are “fanout” which sends a message to everybody, and “topic” which lets you set a hierarchy of queue names to group clusters of workers. I used the [py-amqp](http://barryp.org/software/py-amqplib/) library in python to get this done, it just worked. Other good links: [performance test code](http://github.com/myelin/queueing-playground/tree/master), [queue w/ thrift](http://github.com/myelin/simple-thrift-queue/tree/master), [amqp description](http://rajith.2rlabs.com/2007/10/13/amqp-in-10-mins-part4-standard-exchange-types-and-supporting-common-messaging-use-cases/).

Adding 10000 ~100byte json strings to Rabbit in durable mode from a low-end amzn instance (not the rabbit instance) takes ~5 seconds. That’s 40x faster for sending than SQS. Messages can be retrieved serially from a single thread at about 25 msgs/second (again, more threads == same retrieval rate) Adding 100K messages to Rabbit took 55 seconds.   
  
During message production @ 10K msgs, the beam process has about 15% CPU and 5% RAM on the amzn instance it’s hosted on. During retrieval, the CPU drops to 0.5%. At idle RAM stays at 5 with no CPU. I added 2M messages to test RAM usage and RAM went up to 50% (1.7GB total on these boxes.) [Lots of talk about how to make this easier on yourself here](http://www.nabble.com/RabbitMQ-memory-management-td19428592i20.html) but the overall word is to not have that many unread messages sitting around. 

Costs for rabbit: $72/mo for the rabbit instance if I want to keep it segregated. Requests are free if i stay in ec2.   
  
Upsides: much faster adding, more flexible, slightly cheaper.

Downsides: something I have to think about, backup, image, etc, although so far it seems pretty hands-off and I can put the persistent data on an EBS w/ an hourly snapshot just in case. 

Edit: Some code in Python using py-amqp follows. All I had to do on the server was apt-get install erlang and then install the RabbitMQ .deb file and poke a hoke in the amzn security group for the AMQP port 5672. I didn’t do any other config on the rabbitMQ server to be able to run this code. (Obviously for real world use you’ll want auth & stuff)

 
import simplejson
import amqplib.client\_0\_8 as amqp
        
class Queue():
    """Base class for AMQP queue service."""
    def \_\_init\_\_(self, queue\_name):
        self.conn = amqp.Connection('HOST\_HERE', userid='guest', password='guest', ssl=False) 
        self.queue\_name = queue\_name
        self.ch = self.conn.channel()

    def declare(self):
        return self.ch.queue\_declare(self.queue\_name, passive=False, durable=True, exclusive=False, auto\_delete=False)
        
    def \_\_len\_\_(self):
        """Return number of messages waiting in this queue"""
        \_,n\_msgs,\_ = self.declare()
        return n\_msgs
    
    def consumers(self):
        """Return how many clients are currently listening to this queue."""
        \_,\_,n\_consumers = self.declare()
        return n\_consumers
    

class QueueProducer(Queue):
    def \_\_init\_\_(self, queue\_name):
        """Create new queue producer (guy that creates messages.) 
        Will create a queue if the given name does not already exist."""
        Queue.\_\_init\_\_(self, queue\_name)
        self.ch.access\_request('/data',active=True,read=False,write=True)
        self.ch.exchange\_declare('sqs\_exchange', 'direct', durable=True, auto\_delete=False)
        qname, n\_msgs, n\_consumers  = self.declare()
        print "Connected to %s (%d msgs, %d consumers)" % (qname, n\_msgs, n\_consumers)
        self.ch.queue\_bind(self.queue\_name, 'sqs\_exchange', self.queue\_name)
    
    def delete(self):
        """Delete a queue and closes the queue connection."""
        self.ch.queue\_delete(self.queue\_name)
        self.ch.close()
        
    def write(self, message):
        """Write a single message to the queue. Message can be a dict or a list or whatever."""
        m = amqp.Message(simplejson.dumps(message), content\_type='text/x-json')
        self.ch.basic\_publish(m, 'sqs\_exchange', self.queue\_name)

class QueueConsumer(Queue):
    def \_\_init\_\_(self, queue\_name):
        """Create new queue consumer (guy that listens to messages.)"""
        Queue.\_\_init\_\_(self, queue\_name)
        self.ch.access\_request('/data',active=True, read=True, write=False)
        self.ch.queue\_bind(self.queue\_name, 'sqs\_exchange', self.queue\_name)
    
    def ack(self, delivery\_tag):
        """Acknowledge receipt of the message (which will remove it off the queue.)
         Do this after you've completed your processing of the message. 
         Otherwise after some amount of time (about a minute) it will go back on the queue.
         e.g. 
        
         (object, tag) = consumer.get()
         if(object is not None):
             error = doSomethingWithMessage(object)
             if(error is None):
                 consumer.ack(tag)
        
        """
        self.ch.basic\_ack(delivery\_tag)
        
    def get(self):
        """Get a message. Returns the object and a delivery tag.""" 
        m = self.ch.basic\_get(self.queue\_name)
        if(m is not None):
            try:
                ret = simplejson.loads(m.body)
            except ValueError:
                print "Problem decoding json for body " + str(m.body) + ". deleting."
                self.ack(m.delivery\_tag)
                return (None, None)
            return (ret, m.delivery\_tag)
        else:
            return (None,None)
