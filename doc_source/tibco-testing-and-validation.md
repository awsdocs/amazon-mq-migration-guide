# Testing and validation<a name="tibco-testing-and-validation"></a>

 In the [TIBCO EMS typical architecture](tibco-ems-typical-architecture.md) section, a *Topic to Queue* bridge was used to forward messages to other EMS servers\. In Amazon MQ, *App 1* would send messages directly to `Q1` because messages on a queue are forwarded in a [Network of Brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers)\. 

 In the TIBCO EMS example, messages from *App 2* are sent to `Q2` and then forwarded to `Q2@EMS_APPLE`\. In Amazon MQ, the queue name, `Q2`, would be the same on both message brokers, simplifying the configuration of *App 1*\. 

 The following example shows the **AMQ\_ORANGE** broker with consumers in *us\-east\-1* and **AMQ\_APPLE** with consumers in *us\-east\-2* ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/tibco-testing-and-validation-fig-1.PNG) ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/tibco-testing-and-validation-fig-2.PNG) 