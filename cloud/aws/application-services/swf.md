---
description: Simple Workflow Service
---

# SWF

Amazon SFW is a web service that makes it easy to coordinate work across distributed application components. SWF enables application components. SWF enables applications for a range of use cases, including media processing, web application backends, business process workflows, and analytics pipelines, to be designed as a coordination of tasks.

Tasks represent invocations of various processing steps in an application which can be performed by executable code, web service calls, human actions and scripts.

#### SWF Actors

* **Workflow starters** - an application that can initiate \(start\) a workflow.
* **Deciders** - control the flow of activity tasks in a workflow execution. If something has finished \(or failed\) in a workflow, a decider decides what to do next.
* **Activity workers** - carry out the activity tasks.

### SQS vs SWF

* SQS has a retention period of up to 14 days, with SWF workflow executions can last up to 1 year.
* SWF represents a task-oriented API, whereas SQS offers a message-oriented API.
* SWF ensures that a task is assigned only once and is never duplicated. With SQS, you need to handle duplicate messages.
* SWF keeps track of all the tasks and events in an application. With SQS, you need to implement your own application-level tracking, especially if your application uses multiple queues.



