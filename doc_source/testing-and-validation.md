# Testing and validation<a name="testing-and-validation"></a>

 

1. Subscribe to `Q1` on **AMQ\_APPLE** and `Q2` on **AMQ\_ORANGE**\. Using a Network Bridge, create a queue replica on both sides\.
**Note**  
 The process for external subscribers is the same as subscribing to local queues\. 

    The following example shows the **AMQ\_ORANGE** broker with consumers in *us\-east\-1* and **AMQ\_APPLE** with consumers in *us\-east\-2* : ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/ibm-testing-and-validation-fig-1.PNG) ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/ibm-testing-and-validation-fig-2.PNG) 

1. Both queues are now available to both brokers, producers can send messages to any broker, and subscribers can receive messages from any broker\. For *JMS 1\.1* compliant applications, change the endpoint URL to an ActiveMQ failover URL\.

**Note**  
To learn more about a phased migration approach from IBM MQ to Amazon MQ, refer to this [post](http://aws.amazon.com/blogs/compute/migrating-from-ibm-mq-to-amazon-mq-using-a-phased-approach/)\.