# TIBCO EMS typical architecture<a name="tibco-ems-typical-architecture"></a>

**Topics**
+ [TIBCO EMS cross\-regional architecture in AWS](#tibco-ems-typical-cross-region-architecture)
+ [TIBCO EMS high availability architecture](#tibco-ems-typical-high-availability-architecture)

## TIBCO EMS cross\-regional architecture in AWS<a name="tibco-ems-typical-cross-region-architecture"></a>

 The below diagram shows the typical architecture of TIBCO EMS routing between two TIBCO EMS Servers in different regions, common in many enterprise systems\. TIBCO EMS Server **EMS\_ORANGE** is deployed in the *us\-east\-1* region and **EMS\_APPLE** is deployed in the *us\-east\-2* region: ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/tibco-cross-regional-fig-1.PNG) For application *App 1* to communicate with *App 2*: 

1. *App 1* uses a topic destination, *Topic1* on server **EMS\_ORANGE** to publish messages\.

1. Published messages are transmitted to topic *Topic1* on server **EMS\_APPLE** using the configured route\.

1. Published messages are transmitted to *Topic1* on server **EMS\_APPLE** using the configured route\.

1. On **EMS\_APPLE**, a bridge is configured to move messages from topic, *Topic1* to queue, *Queue1*\. Messages are then consumed by *App 2*\.

## TIBCO EMS high availability architecture<a name="tibco-ems-typical-high-availability-architecture"></a>

 In this configuration, High availability is provided by configuring a pair of servers, *Primary* and *Secondary*\. In a typical enterprise architecture, two high availability configurations, *shared* and *unshared*\. The shared state setup is the most widely used setup in enterprise settings\. The following diagram demonstrates the Shared State configuration for a pair of messaging servers: ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/tibco-high-availability-fig-1.PNG) 

 In the above diagram, a pair of messaging servers share a state by sharing file\-based storage\. The primary server attains the lock on the shared storage capacity, becomes active, and accepts client connections, while the secondary server remains in passive mode\. Meanwhile, the primary and secondary servers will be made aware of one another's status via periodic, heartbeat pings\. 

 In te case of a failover, the secondary server will assume the state of the primary server, and acquire the lock on the shared state\. 

**Note**  
 The above configuration is unable to support more than two servers, and data replication across the servers for durability\. 