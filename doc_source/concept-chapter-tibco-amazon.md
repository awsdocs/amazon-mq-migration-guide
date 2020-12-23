# TIBCO EMS server<a name="concept-chapter-tibco-amazon"></a>

**Topics**
+ [Terminologies](#tibco-ems-and-amazon-mq-terminologies)
+ [TIBCO EMS typical architecture](tibco-ems-typical-architecture.md)
+ [Replicating TIBCO EMS with Amazon MQ](tibco-amazon-mq-architecture.md)
+ [Steps to re\-platform](tibco-re-platform.md)
+ [Testing and validation](tibco-testing-and-validation.md)

## Terminologies<a name="tibco-ems-and-amazon-mq-terminologies"></a>

 The following is a list of common TIBCO EMS concepts and how they relate to Amazon MQ\. 


|  **TIBCO EMS**  |  **Amazon MQ**  |   |   | 
| --- | --- | --- | --- | 
|  Component  |  Description  |  Component  |  Description  | 
|  EMS Server  |  TIBCO EMS Server is a message broker that supports standards\-based Java Message Service \(JMS\) 1\.1 and 2\.0\. It also supports TIBCO proprietary messaging formats, FTL, Rendezvous, and SmartSockets\.  |  Broker  |  Broker in Amazon MQ is equivalent to TIBCO EMS Server\. It provides support for industry\-standard APIs such as JMS and NMS, and protocols such as AMQP, STOMP, MQTT, and WebSocket\.  | 
|  Static Destination  |  Configuration information for a static destination is stored in configuration files for the EMS server\.  |  Startup Destination  |  Amazon MQ allows you to create destinations when the broker is started by configuring Startup Destinations\.  | 
|  Dynamic Destination  |  Dynamic Destination is created as required by the client application and exists as long as there are messages or consumers associated with a destination\.  |  Destination  |  In Amazon MQ, a destination is, by default, created automatically when it is used\. You can use the Delete Inactive Destinations feature in order to replicate the behavior of Dynamic Destinations in TIBCO EMS\.  | 
|  Temporary Destination  |  A Temporary Destination is used for reply messages in request/reply interactions\.  |  Temporary Destination  |  A Temporary Destination is used for reply messages in request/reply interactions\.  | 
|  Queue  |  A queue is a mode to provide point to point messaging channel from producers to consumers  |  Queue  |  Similar to that of TIBCO EMS, Amazon MQ’s Queue is a mode to provide point\-to\-point messaging channels from producers to consumers  | 
|  Route  |  TIBCO EMS servers have to enable and configure routing to route messages to one or more servers\. A route forwards messages between corresponding global destinations\.  |  Network Connector  |  [Networks of Brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers) can connectto each other on a Network Connector and allows connections across Availability Zones and region\.  | 
|  Routed Queue  |  A Routed Queue has to be configured on another EMS server for messages to be forwarded to or from a Queue\. Queue messages can travel only one hop to the home queue, and one hop from the home queue\.  |  Network of Brokers  |  This is achieved using a network of brokers\. Amazon MQ provides a richer feature\-set to work with destination behavior in [Networks of Brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers)\.  | 
|  Topic  |  Topics implement publish and subscribe messaging, and are equivalent to topics in Amazon MQ\.  |  Topics  |  Topics in Amazon MQ are equivalent to topics in TIBCO EMS\.  | 
|  Global Topic  |  A Global Topic has to be configured on another EMS server for messages to be forwarded to or from a Topic\. In a multi\-hop zone, Topic messages are forwarded to all servers connected by routers within the zone\. In a one\-hop zone, topic messages travel only one hop\.  |  Network of Brokers  |  This is achieved using a network of brokers\. Amazon MQ provides a richer feature\-set to work with destination behavior in [Networks of Brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers)\.  | 
|  Destination Bridges  |  This allows all messages delivered to one destination to also be delivered to the bridged destination\. It is most commonly used to create durability of messages published on a topic using the topic to queue bridge\.  |  Virtual Topics and Composite Destinations  |  Topic to queue bridge\-like functionality can be achieved using Virtual Topics in Amazon MQ\. Other bridges can be migrated using Composite Destinations in Amazon MQ\.  | 
|  Message Store  |  EMS server writes persistent messages to disk and provides file\-based and database stores\. For file\-based stores, you have to truncate the files periodically to relinquish disk space\.  |  N/A  |  Amazon MQ also supports message persistence\. Amazon MQ is a managed service and the overall storage is fully managed by AWS\.  | 