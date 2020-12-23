# Steps to re\-platform<a name="tibco-re-platform"></a>

 You can use the following procedure to migrate the TIBCO EMS architecture shown [here](tibco-ems-typical-architecture.md) to an equivalent Amazon MQ architecture without impacting *App 1* or *App 2*: 

1. Create an [active/standby broker](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/active-standby-broker-deployment) in *us\-east\-1* and another in *us\-east\-2* named as **AMQ\_ORANGE** and **AMQ\_APPLE**\.

1. Create a *Network Bridge* between 2 brokers by adding a duplex network connector definition to one of the queues:

   ```
   <networkConnectors> 
       <networkConnector duplex="true" name="connector_AMQ_ORANGE_to_AMQ_APPLE" uri="masterslave:(ssl://b-d63bcc4d-682b-40a2-8227-31386bcf1e3d-1.mq.us-east-2.amazonaws.com:61617,ssl://b-d63bcc4d-682b-40a2-8227-31386bcf1e3d-2.mq.us-east-2.amazonaws.com:61617)" userName="amqadmin"/> 
   </networkConnectors>
   ```

    After the reboot of **AMQ\_ORANGE**, there should be a Network Bridge created between both brokers as illustrated below: ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/migration-guide/images/tibco-replatform-fig-1.PNG) 
**Note**  
Steps 1 and 2 can be replicated using a AWS CloudFormation template\. For more information about using AWS CloudFormation to set up Amazon MQ brokers, see the Amazon MQ [ AWS CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_AmazonMQ)\.

1.  Retrieve the list of static TIBCO EMS server destinations from the config files, `queues.conf` and `topics.conf` or by using the following `tibemsadmin` commands: 

   ```
   show queues * static
   show topics * static
   ```

    When finished, update the Amazon MQ broker **AMQ\_ORANGE** configuration file to add startup destinations as shown here: 

   ```
   <destinations>
       <queue physicalName="FOO.BAR"/>
       <topic physicalName="SOME.TOPIC"/>
   </destinations>
   ```

1.  Destination properties for TIBCO EMS can be found in queues\.conf and topics\.conf files\. Per Destination level Policy can be set in Amazon MQ using the `destinationPolicy` section in the configuration file\. 

1.  Retrieve the list of TIBCO EMS Bridges from `bridges.conf`\. For example, the Bridge from source topic `NOTIFY.FOOBAR` to target queues `FOO ` and `BAR` is shown as: 

   ```
   [topic:NOTIFY.FOOBAR]
   queue=FOO
   queue=BAR
   ```

    When finished, up the Amazon MQ broker **AMQ\_ORANGE ** configuration file to add Composite Destinations that match TIBCO EMS bridges\. 
**Note**  
 *Simple Topic to Queue* bridges are needed in TIBCO EMS to support *m\-hop* routing\. In Amazon MQ this is not needed and queues can be used directly with a [Network of Brokers](https://docs.aws.amazon.com/amazon-mq/latest/developer-guide/network-of-brokers)\. 