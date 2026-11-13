Lets say there is a Simple system : User places an order on amazon
What all needs to happen?
1. Save order in DB
2. Reduce inventory
3. Send Confirmation mail
4. Send SMS
5. Notify Warehouse
6. Generate Invoice
7. Update Analytics


Naive Approach (Synchronous Processing)
1. Order Serivce
2. Inventory Service
3. Email Service
4. SMS Service
5. Analytics Service

Problem
1. User waits untill all these finish
2. If Email service is slow then order API is slow
3. If SMS service id down then order fails
4. Tight coupling = fragile system

The solution is Asynchronous Processing.
1. Background jobs using application threads in the service
   executor.submit( () -> sendEmail(orderId));
   Pros: simple, no infra required, low latency, good for tiny apps
   Cons: services crashes then job lost, no retreis, no durability, not scalable across machine

2. Databased-based Async (polling)
   Order service -> DB (task table)
   Worker -> polls DB -> processes task
   pros: Durable, Easy to implement, simple logic
   cons: Polling is inefficient, DB load increase, Scaling worker is painful

3. Cron Jobs/Shudeled Works
   Its a time driven architecture. Good for Time-based async work, Batch processing, Maintenance tasks
   Pros: Extremely Simple, predictable execution, no event dependency
   cons: not real time, delay is inherent, poor for user-triggered actions

4. Webhooks
   Your system calls someone else's endpoint asynchronously. Good for third party integrations but not reliable without retries
   Pros: Simple, No queue infra, Good for integrations
   Cons: Network failures, Retry complexity, Tight coupling, No buffering

5. Serverless/ Task Services
   Managed async execution. i.e. AWS lamda, cloud tasks, azure functions. Good for cloud-native systems and spiky workloads
   Pros: Auto-scaling, no server management, pay-per-use
   Cons: Cold starts, Vendor lock-in, Execution limits

6. Message Queue
   Producer -> Message Queue -> Consumer
   As tasks are getting processed by consumer it will get removed from the queues

8. Event Streaming (Kafka/ Pub-Sub)
   Events are published and many consumers react
   
   
   
