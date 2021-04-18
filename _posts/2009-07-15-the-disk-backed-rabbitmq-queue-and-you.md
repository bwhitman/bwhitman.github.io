---
title: "The disk backed rabbitmq queue and you"
date: "2009-07-15"
---

![](images/RabbitMQLogo.png)

RabbitMQ is [still doing great](http://notes.variogr.am/post/67710296/replacing-amazon-sqs-with-something-faster-and-cheaper) but had one very bad flaw: more than 1.5m un-eaten messages on a single “small” type server (1-2GB RAM) would just take the whole thing down. And the [current solution involves giving your machine more swap](http://www.lshift.net/blog/2009/04/02/cranial-surgery-giving-rabbit-more-memory). When you’re not in a real time scenario (worker messaging on “fake M/R tasks” is ours), you want your messages to stick around and not break all the servers. A broken queue really bites. Fortunately, the LShift guys are on the case and today I got to try out the new disk-backed queue setup. [It’s in as the bug20980 branch.](http://hg.rabbitmq.com/rabbitmq-server/)

You’ll want to build rabbitmq yourself, not so bad….

\# Get dependencies and checkout rabbitmq
apt-get install mercurial libncurses-dev
hg clone [http://hg.rabbitmq.com/rabbitmq-server](http://hg.rabbitmq.com/rabbitmq-server)
hg clone [http://hg.rabbitmq.com/rabbitmq-codegen](http://hg.rabbitmq.com/rabbitmq-codegen)

# Download erlang compiler & etc
curl -O "http://www.erlang.org/download/otp\_src\_R13B01.tar.gz"
tar xzvf otp\_src\_R13B01.tar.gz
cd otp\_src\_R13B01
./configure; make; make install

# Build rabbit from the "bug20980" branch which allows the disk-pin
cd ~/rabbitmq-server
hg update -C bug20980
make

# If you want your disk-backed queues to not go into /var/lib/rabbitmq  
\# (e.g. amazon machine!!) you'll want to do this (/vol is our EBS location)

export RABBITMQ\_LOG\_BASE="/vol/rabbitlog"
export RABBITMQ\_MNESIA\_BASE="/vol/mnesia"

# Start up rabbitmq
cd scripts
./rabbitmq-server

Now you’ve got two choices. You can have rabbit make the decision when to send your queues off to “disk mode,” or you can do it yourself. The yourself version is:

./rabbitmqctl pin\_queue\_to\_disk queue\_name\_you\_want\_to\_pin

You can check up on the status of your queues, including their mode, like so:

./rabbitmqctl list\_queues name mode messages messages\_ready messages\_unacknowledged durable
Listing queues ...
memqueue	mixed	100	100	0	true
diskqueue	disk	2509876	2509876	0	true

If you let Rabbit make the choice, it seems to wait for your system RAM to start running out. Queues will stay in “mixed” mode until that happens (on my EC2 m1.large 8GB RAM machine w/ 4GB swap, this happened when we got to 75% via top – 5 million 1KB or so messages.) The disk mode kicks in automatically (and seems to stay there) – your RAM usage will just drop almost immediately. It won’t crash! 

Why not stay with disk mode full time? Performance, although on EC2 machines it’s kind of a wash anyway. We all know that IO and network throughput on these guys is … suboptimal … to put it lightly. With mixed mode on I am still getting roughly 2000 messages/second added and 25 messages/second to ack&read (both of these #s are from another EC2 instance on the same zone talking to the broker serially via py-amqplib.) Disk mode is slightly worse write speed (1900 m/s) and no noticeable difference is read speed (there is obviously something else blocking us there.)

Going to give this some real world stress tests over the next week, but so far no real problems.

Many thanks to [LShift](http://www.lshift.net/) for the help.
