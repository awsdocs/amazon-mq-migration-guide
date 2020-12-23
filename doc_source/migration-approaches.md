# Migration approaches<a name="migration-approaches"></a>

The first step in migrating your commercial message broker to Amazon MQ is determining the right migration approach for your existing application architecture\. The following are the most common approaches to migration:

**Topics**
+ [Rehost](#rehost)
+ [Replatform](#replatform)
+ [Refactor \(re\-architect\)](#re-architect-refactor)
+ [Phased migration](#phased)

## Rehost<a name="rehost"></a>

The rehost \("lift and shift"\) approach is ideal for moving on\-premises workloads to the cloud when time is critical and ambiguity is high\. Rehosting is the process of moving your existing applications from one infrastructure to another\. The most significant benefit of this approach is that it enables you to migrate without changing large portions of application code\. Fewer changes can also reduce the amount of re\-training needed for engineers\.

## Replatform<a name="replatform"></a>

The replatform strategy involves moving applications almost as\-is, but replacing some components to take advantage of a cloud architecture\. Here you might make a number of cloud\-based optimizations to achieve some tangible benefit, but you aren't changing the core architecture of your application\. You may be looking only to reduce the amount of time that you spend managing your broker\.

## Refactor \(re\-architect\)<a name="re-architect-refactor"></a>

Refactoring \(also known as "re\-architecting"\) maximizes the benefits of moving to the cloud\. Refactoring requires research, planning, experimentation, implementation, and deployment\. These efforts generally provide the greatest rate of return in the form of reduced hardware and storage costs, less operational maintenance, and the most flexibility to meet future business needs\. In many cases, it involves breaking up the application into independent services and transitioning to a microservices architecture\.

## Phased migration<a name="phased"></a>

If you are interested in a phased, incremental migration approach, we recommend using a JMS proxy implementation such as [Camel](https://camel.apache.org/)\. For example, you can use the [JMS Bridge Sample](https://github.com/muellerc/amazon-mq-to-websphere-mq-bridge) project, which allows you to bridge from your existing on\-premises messaging broker to [Amazon MQ](https://aws.amazon.com/amazon-mq/)\.

You can also use [enterprise integration patterns](https://github.com/aws-samples/amazon-mq-enterprise-integration-patterns/)sample to learn how to use [Apache Camel](https://camel.apache.org/manual/latest/getting-started.html) and Amazon MQ to implement common patterns for routing, message transformation, and integration with other AWS services\. In the sample, you will use Amazon EKS to scale Apache Camel\. You can apply the same approach to migrate from IBM MQ or TIBCO EMS to Amazon MQ\.