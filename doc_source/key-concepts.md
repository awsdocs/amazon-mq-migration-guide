# Key concepts<a name="key-concepts"></a>

**Topics**
+ [Messaging protocols](#messaging-protocols)
+ [Message persistence](#message-persistence)
+ [Network options](#network-options)
+ [Availability options](#high-availability)
+ [Messaging patterns](#messaging-patterns)
+ [Performance and scalability](#scalability-options)
+ [Latency](#latency)
+ [Destination options](#destination-options)
+ [Security and authentication](#authentication-and-permissions)
+ [Broker quotas](#broker-limits)
+ [Configuration options](#configurations)
+ [Cost estimation](#cost-estimation-and-tco)

## Messaging protocols<a name="messaging-protocols"></a>

You can connect your broker to Amazon MQ without any code changes if you currently use one of the following industry\-standard protocols:
+ [AMQP](http://activemq.apache.org/amqp.html)
+ [MQTT](http://activemq.apache.org/mqtt.html)
+ MQTT over [WebSocket](http://activemq.apache.org/websockets.html)
+ [OpenWire](http://activemq.apache.org/openwire.html)
+ [STOMP](http://activemq.apache.org/stomp.html)
+ STOMP over WebSocket

For more information about connecting to an Amazon MQ managed broker, see [Working Examples of Using Java Message Service \(JMS\) with ActiveMQ](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-working-java-example) in the *Amazon MQ Developer Guide*\.

## Message persistence<a name="message-persistence"></a>

To replicate *persistence mode* or *sync point control* options with Amazon MQ, you can deploy your brokers as [active/standby brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/active-standby-broker-deployment)\. In the active/standby deployment, brokers use shared storage across multiple Availability Zones, with an optional time to live \(TTL\)\.

For more information about how Amazon MQ ensures message durability, see [Availability options](#high-availability)\.

## Network options<a name="network-options"></a>

Depending on the interlopability of your applications and the type of access that they need, you can permit public access, [VPN access](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN), or [VPC access](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc) using Amazon Virtual Private Cloud \(Amazon VPC\)\. 

In a *hybrid architecture* where on\-premises systems need access to resources in the cloud, we recommend setting up your Amazon MQ managed brokers with *public network access*\. You can also achieve a hybrid solution by using [AWS VPN](http://aws.amazon.com/vpn/) or [AWS Direct Connect](http://aws.amazon.com/directconnect/)\.

**Tip**  
If your resources are primarily deployed within the AWS Cloud, we recommend configuring your Amazon MQ brokers with Amazon VPC\. For network access across multiple VPCs, you can use [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)\.

## Availability options<a name="high-availability"></a>

Amazon MQ supports durability\-optimized brokers backed by [Amazon Elastic File System \(Amazon EFS\)](http://aws.amazon.com/efs/)\. You can configure [single\-instance brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/single-broker-deployment) \(one broker in one Availability Zone\) or [active/standby brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/active-standby-broker-deployment) \(two brokers in two different Availability Zones\)\. In either configuration, Amazon MQ can automatically provision infrastructure for high message durability by storing messages redundantly across multiple Availability Zones\.

**Note**  
 In the event of a broker or Availability Zone failure, active/standby brokers automatically fail over to a standby instance in another Availability Zone\.

To achieve high availability and message durability, you can use a [network of brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers.html)\. A network of brokers is a series of simultaneously active single\-instance or active/standby brokers that allows you to rapidly scale your throughput and connection count\. You can configure a network of brokers in a variety of topologies depending on your application's needs\.

## Messaging patterns<a name="messaging-patterns"></a>

Amazon MQ offers the following topology options to support a variety of messaging patterns:
+ Point\-to\-point
+ Request\-response
+ Hub and spoke
+ Mesh
+ Enterprise service bus

For more information about using Amazon MQ to set up the right broker topology for your cloud architecture, see [ Amazon MQ Broker Architecture](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-broker-architecture) in the *Amazon MQ Developer Guide*\.

**Important**  
Revising a broker configuration or an ActiveMQ user does not immediately apply those changes\. For your changes to take effect, you must wait for the next maintenance window or reboot the broker\. For more information, see [Amazon MQ Broker Configuration Lifecycle](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-broker-configuration-lifecycle) in the *Amazon MQ Developer Guide*\.

## Performance and scalability<a name="scalability-options"></a>

With Amazon MQ you can scale your messaging middleware horizontally, vertically, or in a hybrid model\.

*Horizontal scaling* enables you to increase your throughput and connection count without interruptions, because your resources remain active and online\. To scale horizontally, you can deploy a [network of brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers.html) in an active/standby configuration across multiple Availability Zones\.

To scale your resources *vertically*, you can increase the compute capacity of your broker instances from `mq.t2.micro` \(1 vCPU and 1 GiB\) up to `mq.m5.4xlarge` \(16 vCPU and 64 GiB\)\. For more information about Amazon MQ instance types, see [Instance Types](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/broker#broker-instance-types) in the *Amazon MQ Developer Guide*\.

**Note**  
Choosing larger broker instance types might not improve overall system throughput\. Overall latency is due to many factors, such as message size, the type of protocol, the number of active producers and consumers, consumption speed, and message persistence\.

Amazon MQ also supports creating throughput\-optimized message brokers backed by [Amazon Elastic Block Store \(Amazon EBS\)](http://aws.amazon.com/ebs/)\. These brokers are ideal for applications such as high\-volume order processing, stock trading, and text processing\.

To instruct Amazon MQ to optimize for queues with slow consumers, set the `concurrentStorageAndDispatchQueues` attribute to `false`\.

The following table shows the throughput of an `mq.m5.2xlarge` broker configured with these options:
+ **Broker instance** – `mq.m5.2xlarge`
+ **Persistent** – `TRUE`
+ **Client** – `m5.xlarge`
+ **CSAD** – `TRUE`
+ **Protocol** – OpenWire


|  | Producers/Consumers | 
| --- |--- |
| Message size | Metrics | 25 | 50 | 100 | 200 | 
| --- |--- |--- |--- |--- |--- |
| 1 KB | TPS | 2,250 | 4,300 | 8,467 | 16,334 | 
| --- |--- |--- |--- |--- |--- |
| CPU% | 8% | 15% | 27% | 58% | 
| --- |--- |--- |--- |--- |
| 5 KB | TPS | 2,067 | 3,834 | 7,150 | 14,516 | 
| --- |--- |--- |--- |--- |--- |
| CPU% | 10% | 17% | 30% | 63% | 
| --- |--- |--- |--- |--- |
| 10 KB | TPS | 1,900 | 3,467 | 7,083 | 11,334 | 
| --- |--- |--- |--- |--- |--- |
| CPU% | 15% | 24% | 48% | 80% | 
| --- |--- |--- |--- |--- |
| 50 KB | TPS | 1,592 | 2,917 | 4,500 | 4,917 | 
| --- |--- |--- |--- |--- |--- |
| CPU% | 30% | 52% | 83% | 92% | 
| --- |--- |--- |--- |--- |
| 100 KB | TPS | 1,250 | 2,184 | 2,513 | 2,770 | 
| --- |--- |--- |--- |--- |--- |
| CPU% | 42% | 72% | 85% | 92% | 
| --- |--- |--- |--- |--- |

**Note**  
Performance numbers can vary depending on multiple configuration parameters\. For more information on Amazon MQ throughput measurements, see [Throughput Benchmarks](chapter-benchmarks.md)\.

You can measure the throughput of your Amazon MQ brokers using [JMS Benchmark](http://aws.amazon.com/blogs/compute/measuring-the-throughput-for-amazon-mq-using-the-jms-benchmark/)\.

## Latency<a name="latency"></a>

You can set up your Amazon MQ brokers for low\-latency messaging, with latency often as low as single\-digit milliseconds\. Use an *always\-on* connection to help reduce the amount of time that it takes to deliver messages to a consumer\.

Using *in\-memory* storage can further reduce overall latency across your messaging architecture\. For more information on how different storage types can affect latency, see [Differences Between Storage Types](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/broker-storage#differences-between-storage-types) in the *Amazon MQ Developer Guide*\.

## Destination options<a name="destination-options"></a>

You can set up Amazon MQ managed brokers as *queues* or *topics*\. Amazon MQ queues are, by default, *first in, first out \(FIFO\)* queues, also known as ordered queues\. You can scale FIFO queues using [Message Groups](https://activemq.apache.org/message-groups)\. You can configure your broker destinations with *at\-least\-once delivery*, *at\-most\-once delivery*, or *exactly\-once delivery* options\.

Topics in Amazon MQ use the *publisher/subscriber* pattern and can be durable or non\-durable\. Amazon MQ topics also support [Virtual Destinations](https://activemq.apache.org/virtual-destinations.html), where publishers broadcast messages to a pool of subscribers through queues\. We recommend using this method instead of durable topics\.

**Tip**  
You can optimize and fine\-tune the performance of your topics\. For more information, see [Performance Tuning](https://activemq.apache.org/performance-tuning.html) on the Apache ActiveMQ website\.

## Security and authentication<a name="authentication-and-permissions"></a>

With Amazon MQ you control who is allowed to create or modify brokers, and which applications are allowed to send and receive messages\. For more information about authentication options, and how to integrate the [Lightweight Directory Access Protocol \(LDAP\)](https://ldap.com/) with your Amazon MQ brokers, see [Messaging Authentication and Authorization for ActiveMQ](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/security-authentication-authorization) in the *Amazon MQ Developer Guide*\.

Connections to Amazon MQ brokers use Transport Layer Security \(TLS\)\. To isolate your brokers in a private virtual network, you can restrict access to a private endpoint within a VPC\. To control network access to your brokers, you can configure security groups in the VPC\. For more information, see [Security Best Practices for Amazon MQ](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/using-amazon-mq-securely) in the *Amazon MQ Developer Guide*\.

Amazon MQ encrypts messages at rest and in transit using encryption keys that it manages and stores securely in [AWS Key Management Service \(AWS KMS\)](http://aws.amazon.com/kms/)\. AWS KMS helps reduce the operational burden and complexity involved in protecting sensitive data\. With encryption at rest, you can build security\-sensitive applications that meet encryption compliance and regulatory requirements\.

**Tip**  
For additional security, we highly recommend designing your application to use [client\-side encryption](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction)\.

For more information about Amazon MQ security and how messaging data is encrypted, see [Data Protection in Amazon MQ](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/data-protection) in the *Amazon MQ Developer Guide*\.

## Broker quotas<a name="broker-limits"></a>

By default, each Amazon MQ broker can support 1,000 connections \(or 100 connections for `mq.t2.micro` brokers\)\. To allow multiple consumers to share connections to your Amazon MQ brokers and to improve overall performance, we recommend using *pooled connections*\. For more information, see [Always Use Connection Pooling](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/connecting-to-amazon-mq.html#always-use-connection-pooling) in the *Amazon MQ Developer Guide*\.

You can request an increase for many [broker usage quotas](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-limits.html#broker-limits) for your AWS account\. For more information, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

## Configuration options<a name="configurations"></a>

Amazon MQ supports standard JMS features including point\-to\-point \(message queues\), publish\-subscribe \(topics\), request/reply, persistent and non\-persistent modes, JMS transactions, and distributed \(XA\) transactions\.

Amazon MQ brokers can also support more complex messaging patterns such as composite destinations, which enable producers to send the same message to multiple destinations, and virtual destinations, which enable publishers to broadcast messages via a topic to a pool of receivers subscribing through queues\.

For more information, see [Amazon MQ Broker Configuration Parameters](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/amazon-mq-broker-configuration-parameters) in the *Amazon MQ Developer Guide*\.

## Cost estimation<a name="cost-estimation-and-tco"></a>

With Amazon MQ, you pay only for the provisioned capacity that you use\. Factors such as broker instance type, the amount of data stored on each broker instance, and the AWS Region in which you deploy your brokers can affect your total cost of ownership\. To estimate your broker costs for Amazon MQ, you can us the [AWS Pricing Calculator](https://calculator.aws/#/)\.

**Note**  
For data transferred in and out of Amazon MQ, you pay standard AWS data transfer charges\.

To get started, Amazon MQ offers a Free Tier, which includes up to 750 hours of a single\-instance `mq.t2.micro` or `mq.t3.micro` broker per month, and up to 5 GB of durability\-optimized storage per month for one year\. For more information on the Free Tier, pricing, and associated costs, see [Amazon MQ Pricing](http://aws.amazon.com/amazon-mq/pricing/)\.