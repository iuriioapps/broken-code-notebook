---
description: Simple Queue Service
---

# SQS

Amazon SQS is a web service that gives you access to a message queue that can be used to store messages while waiting for a computer to process them.

SQS is a distributed queue system that enables web service applications to quickly and reliably queue messages that one component in the application generates to be consumed by another component. A queue is a temporary repository for messages that are awaiting processing.

Using SQS you can decouple the components of an application so they run independently, easing message management between components. Any component of a distributed application can store messages in a fail-safe queue. 

Messages can contain up to 256Kb of text in any format. Any component can later retrieve the messages programmatically using the Amazon SQS API.

There are 2 types of queue:

* standard
* FIFO - first in, first out

SQS offers standard as a default queue type. A standard queue lets you have a nearly-unlimited number of transactions per second. Standard queues guarantee that the message is delivered a least once. However, occasionally, more than one copy of a message might be delivered out of order. Standard queues provide best-effort ordering which ensures that messages are generally delivered in the same order as they are sent.

FIFO queues complements the standard queue. The most important feature of this queue type are FIFO delivery and exactly once processing: the order in which the messages are sent and received is strictly preserved and a message is delivered once and remains available until a consumer processes and deletes it; duplicates are not introduced into the queue.

FIFO queues also support message groups that allow multiple ordered message groups within a single queue. FIFO queues are limited to 300 transactions per seconds, but have all the capabilities of the standard queues.

