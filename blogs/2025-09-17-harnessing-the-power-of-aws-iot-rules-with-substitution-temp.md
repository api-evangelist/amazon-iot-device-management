---
title: "Harnessing the power of AWS IoT rules with substitution templates"
url: "https://aws.amazon.com/blogs/iot/unleashing-the-power-of-aws-iot-rules-substitution-templates-in-actions/"
date: "Wed, 17 Sep 2025 16:37:18 +0000"
author: "Andrea Sichel"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<p><a href="https://aws.amazon.com/iot-core/">AWS IoT Core</a> is a managed service that helps you to securely connect billions of Internet of Things (IoT) devices to the AWS cloud. The <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html">AWS IoT rules engine</a> is a component of AWS IoT Core and provides SQL-like capabilities to filter, transform, and decode your IoT device data. You can use AWS IoT rules to route data to more than 20 AWS services and HTTP endpoints using <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html">AWS IoT rule actions</a>. <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-substitution-templates.html">Substitution templates</a> are a capability in IoT rules that augments the JSON data returned when a rule is triggered and AWS IoT performs an action. This blog post explores how AWS IoT rule actions with substitution templates unlock simpler, more powerful IoT architectures. You’ll learn proven ways to cut costs and enhance scalability. Through practical examples of message routing and load balancing, smarter, more efficient IoT solutions.</p> 
<h2>Understanding the fundamental components</h2> 
<p>Each AWS IoT rule is built upon three fundamental components: a SQL-like statement that handles message filtering and transformation, one or more IoT rule actions that run and route data to different AWS and third party services, and optional functions that can be utilized in both the SQL statement and rule actions.</p> 
<p>The following is an example of an AWS IoT rule and its components.</p> 
<pre><code class="lang-json">{
   "sql": "SELECT *, get_mqtt_property(name) FROM 'devices/+/telemetry'", 
   "actions":[
    {
      "s3":{  
        "roleArn": "arn:aws:iam::123456789012:role/aws_iot_s3",
        "bucketname": "MyBucket",
        "key" : "MyS3Key"
      }
    }
   ]
}
</code></pre> 
<p>The SQL statement serves as the gateway for rule processing and determines which MQTT messages should be handled based on specific topic patterns and conditions. The rule employs a SQL-like and supports SELECT, FROM, and WHERE clauses (for more information, see <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-sql-reference.html">AWS IoT SQL reference</a>). Within this structure, the FROM clause defines the MQTT topic filter, and the SELECT and WHERE clauses specify which data elements should be extracted or transformed from the incoming message.</p> 
<p>Functions are essential to the SQL statement and IoT rule actions. AWS IoT rules provide an extensive collection of internal <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-sql-functions.html">functions</a> designed to convert data types, manipulate strings, perform mathematical calculations, handle timestamps, and much more. Additionally, AWS IoT rules provide a set of external functions that help you to retrieve data from AWS services (such as, Amazon DynamoDB, AWS Lambda, Amazon Secrets Manager, and AWS IoT Device Shadow) and embed that data in your message payload. These functions support sophisticated data transformations directly within the rule processing pipeline and eliminates the need for external processing.</p> 
<p>Rule actions determine the destination and handling of processed data. AWS IoT rules support a library of built-in <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html">rule actions</a> that can transmit data to AWS services, like AWS Lambda, Amazon Simple Storage Service (Amazon S3), Amazon DynamoDB, and Amazon Simple Queue Service (Amazon SQS). These rule actions can also transmit data to third-party services like Apache Kafka. Each rule action can be configured with specific parameters that govern how the data should be delivered or processed by the target service.</p> 
<h2>Substitution templates: The hidden gem</h2> 
<p>You can implement functions within the AWS IoT rule SELECT and WHERE statements to transform and prepare message payloads. If you apply this approach too frequently, however, you might overlook the powerful option to use substitution templates and perform transformations directly within the IoT rule action.</p> 
<p>Substitution templates support dynamically inserted values and rule functions into the rule action’s JSON using the <code>${expression}</code> syntax. These templates support many SQL statement functions, such as timestamp manipulation, encoding/decoding operations, string processing, and topic extraction. When you utilize substitution templates within AWS IoT rule actions, you can implement sophisticated routing that significantly reduces the complexity in other architectural layers, resulting in more efficient and maintainable AWS IoT solutions.</p> 
<h3>Real-world implementation patterns</h3> 
<p>Let’s dive into some practical examples that show the versatility and power of using substitution templates in AWS IoT rules actions. These examples will demonstrate how this feature can simplify your IoT data processing pipelines and unlock new capabilities in your IoT applications.</p> 
<h4>Example 1: Conditional message distribution using AWS IoT registry attributes</h4> 
<p>Consider a common IoT scenario where a platform distributes device messages to different business partners, and each partner has their own message processing SQS queue. Different partners own each device in the fleet and their relationship is maintained in the registry as a thing attribute called partnerId.</p> 
<p>The traditional approach includes the following:</p> 
<ul> 
 <li>Option 1 – Maintain partner routing logic on the device. Multiple AWS IoT rules rely on WHERE conditions to input payload: 
  <ul> 
   <li>Requires devices to know their partner’s ID.</li> 
   <li>Increases device complexity and maintenance.</li> 
   <li>Creates security concerns with exposing partner identifiers.</li> 
   <li>Makes partner changes difficult to manage.</li> 
  </ul> </li> 
 <li>Option 2 – Employ an intermediary Lambda function to retrieve the partner ID values associated with devices from the AWS IoT registry and subsequently propagate the message to the partner specific SQS queue: 
  <ul> 
   <li>Adds unnecessary compute and registry query costs.</li> 
   <li>Potentially increases message latency.</li> 
   <li>Creates additional points of failure.</li> 
   <li>Requires maintenance of routing logic.</li> 
   <li>May face Lambda concurrency limits.</li> 
  </ul> </li> 
</ul> 
<p>Here’s a more elegant solution and process that uses substitution templates and the new AWS IoT <a href="https://docs.aws.amazon.com/iot/latest/developerguide/thing-types-propagating-attributes.html">propagating attributes feature</a>:</p> 
<ul> 
 <li>Insert the Partner IDs as attributes in the AWS IoT registry</li> 
 <li>Use the propagating attributes feature to enrich your MQTTv5 user property and dynamically construct the Amazon SQS queue URL using the device’s <code>partnerId</code>. See the following example:</li> 
</ul> 
<pre style="padding-left: 40px;"><code class="lang-json">{
    "ruleArn": "arn:aws:iot:us-east-1:123456789012:rule/partnerMessageRouting",
    "rule": {
        "ruleName": "partnerMessageRouting",
        "sql": "SELECT * FROM 'devices/+/telemetry'",
        "actions": [{
            "sqs": {
                "queueUrl": "https://sqs.us-east-1.amazonaws.com/123456789012/partner-queue-${get(get_user_properties('partnerId'),0}}",
                "roleArn": "arn:aws:iam::123456789012:role/service-role/iotRuleSQSRole",
                "useBase64": false
            }
        }],
        "ruleDisabled": false,
        "awsIotSqlVersion": "2016-03-23"
    }
}</code></pre> 
<p>Using this solution, a device with <code>partnerId</code>=”partner123″ publishes a message. The message is automatically routed to the “partner-queue-partner123” SQS queue.</p> 
<p><span style="text-decoration: underline;">Benefits of this solution:</span></p> 
<p>Using the substitution template significantly simplifies the architecture and provides a scalable and maintainable solution for partner-specific message distribution. The solution,</p> 
<ul> 
 <li>Eliminates the need for additional compute resources.</li> 
 <li>Provides immediate routing without added latency.</li> 
 <li>Simplifies partner relationship management through updates in the AWS IoT thing registry. For example, introducing new partners, can be updated by modifying the registry attributes. This update wouldn’t require any updates or changes to the devices or the routing logic.</li> 
 <li>Maintains security by not exposing queue information to devices.</li> 
</ul> 
<h4>Example 2: Intelligent load balancing with Amazon Kinesis Data Firehose</h4> 
<p>Consider a scenario where millions of devices publish telemetry data to the same topic. There is also a need to distribute this high-volume data across multiple Amazon Data Firehose streams to avoid throttling issues when buffering the data to Amazon S3.</p> 
<p>The traditional approach includes the following:</p> 
<ul> 
 <li>Device-side load balancing: 
  <ul> 
   <li>Implement configuration management to provide different stream IDs across the devices.</li> 
   <li>Require the devices to include stream targeting in their messages.</li> 
   <li>Create multiple AWS IoT rules to match the specific stream IDs.</li> 
  </ul> </li> 
 <li>AWS Lambda-based routing: 
  <ul> 
   <li>Deploy a Lambda function to distribute messages across streams.</li> 
   <li>Implement custom load balancing logic.</li> 
  </ul> </li> 
</ul> 
<p>Traditional approaches exhibit similar negative impacts as outlined in the preceding example (maintenance overhead, security vulnerabilities, device complexity, additional costs, increased latency, and failure points). Furthermore, they present specific challenges in high-volume scenarios, such as heightened risk of throttling and complex streams management.</p> 
<p>By leveraging AWS IoT rule substitution templates, you can implement a streamlined, serverless load balancing solution that dynamically assigns messages to different Firehose delivery streams by:</p> 
<ol> 
 <li>Generate a random number between 0-100000 using rand()*100000.</li> 
 <li>Convert (casting) this random number to an integer.</li> 
 <li>Use modulo operation (mod) to get the remainder when divided by 8.</li> 
 <li>Append this remainder (0-7) to the base name “firehose_stream_”.</li> 
</ol> 
<p>The result is that messages are randomly distributed across eight different Amazon Data Firehose streams (firehose_stream_0 through firehose_stream_7). See the following example:</p> 
<pre><code class="lang-json">{ 
  "ruleArn": 
    "arn:aws:iot:us-east-1:123456789012:rule/testFirehoseBalancing", 
  "rule": { 
    "ruleName": "testFirehoseBalancing", 
    "sql": "SELECT * FROM 'devices/+/telemetry'", 
    "description": "", 
    "createdAt": "2025-04-11T11:09:02+00:00", 
    "actions": [ 
        { "firehose": { 
            "roleArn": "arn:aws:iam::123456789012:role/service-role/firebaseDistributionRoleDemo", 
            "deliveryStreamName": "firehose_stream_${mod(cast((rand()*100000) as Int),8)}", 
            "separator": ",",
            "batchMode": false 
        } 
     } 
    ], 
  "ruleDisabled": false, 
  "awsIotSqlVersion": "2016-03-23" 
  }
}
</code></pre> 
<p><span style="text-decoration: underline;">Benefits of this solution:</span></p> 
<p>This flexible load balancing pattern helps to handle high message volumes by spreading the load across multiple streams. The primary advantage of this approach lies in its scalability. By modifying the modulo function (which determines the remainder of a division, for instance, 5 mod 3 = 2), the dividend (currently set to 8) can be adjusted to correspond with the desired number of streams. For example:</p> 
<ul> 
 <li>Change to mod(…, 4) for distribution across 4 streams.</li> 
 <li>Change to mod(…, 16) for distribution across 16 streams.</li> 
</ul> 
<p>Using this template makes it easy to scale your architecture up or down without changing the core logic of the rule.</p> 
<h4>Example 3: Use CASE statements in substitution templates to build a conditional routing logic</h4> 
<p>Consider a scenario where you need to route your IoT device data, depending on the specific device, either to a production-based or to a Development/Testing (Dev/Test) Lambda function.</p> 
<p>The traditional approach includes the following:</p> 
<ul> 
 <li>Device-side load balancing:</li> 
 <li> 
  <ul> 
   <li>Implement configuration management to provide different environment IDs across the devices.</li> 
   <li>Require the devices to include an environment IDs in their messages.</li> 
   <li>Create multiple AWS IoT rules to match the specific environment IDs.</li> 
  </ul> </li> 
 <li>AWS Lambda-based routing: 
  <ul> 
   <li>Deploy a Lambda function to distribute messages across the different environment AWS Lambda functions after a check against the AWS IoT registry (or an alternative database).</li> 
  </ul> </li> 
</ul> 
<p>Traditional approaches exhibit the same negative impacts as outlined in the preceding examples.</p> 
<p>Here’s a more elegant solution and process that uses substitution templates and the new AWS IoT <a href="https://docs.aws.amazon.com/iot/latest/developerguide/thing-types-propagating-attributes.html">propagating attributes feature</a>:</p> 
<ul> 
 <li>Associate the environment IDs as attributes for all devices in the AWS IoT Registry</li> 
 <li>Use the propagating attributes feature to enrich your MQTTv5 user property</li> 
 <li>Utilize the propagated property to dynamically construct the AWS Lambda function ARN within a CASE statement embedded within the AWS IoT Rule action definition.</li> 
</ul> 
<p>See the following example:</p> 
<pre><code class="lang-json">{ 
  "ruleArn": 
    "arn:aws:iot:us-east-1:123456789012:rule/ConditionalActions", 
  "rule": { 
    "ruleName": "testLambdaConditions", 
    "sql": "SELECT * FROM 'devices/+/telemetry'", 
    "description": "", 
    "createdAt": "2025-04-11T11:09:02+00:00", 
    "actions": [ 
        { "lambda": { 
            "functionArn": 
                "arn:aws:lambda:us-east-1:123456789012:function:${CASE get(get_user_properties('environment'),0) 
                    WHEN \"PROD\" THEN \"message_handler_PROD\" 
                    WHEN \"DEV\" THEN \"message_handler_DEV\" 
                    WHEN NULL THEN \"message_handler_PROD\" 
                    ELSE \"message_handler_PROD\" END }",  
        } 
     } 
  ], 
  "ruleDisabled": false, 
  "awsIotSqlVersion": "2016-03-23" 
 }
}
</code></pre> 
<p><span style="text-decoration: underline;">Benefits of this solution:</span></p> 
<p>Using the substitution template significantly simplifies the architecture and provides a scalable and maintainable solution for partner-specific message distribution. The solution,</p> 
<ul> 
 <li>Removes the requirement to define separate IoT rule and IoT rule actions for each condition.</li> 
 <li>Helps you reduce the cost of using IoT rules and IoT rule actions.</li> 
</ul> 
<h2>Conclusion</h2> 
<p>This blog post explored how substitution templates for AWS IoT rules can transform complex IoT architectures into elegant and efficient solutions. The examples demonstrated that substitution templates are more than just a feature – they’re a powerful architectural tool that leverages AWS IoT capabilities to efficiently solve complex challenges without introducing additional complexity or cost. Substitution templates provide a serverless, scalable approach that eliminates the need for additional compute resources or complex client-side logic. This approach not only reduces operational overhead but also provides immediate cost benefits by removing unnecessary compute resources and simplifying the overall architecture.</p> 
<p>The next time you find yourself designing AWS IoT message routing patterns or facing scaling challenges, consider how a substitution template might offer a simpler and more efficient solution. By leveraging these powerful AWS IoT features, you can create more maintainable, cost-effective, and scalable IoT solutions that truly serve your business needs.</p> 
<p>Remember: The simplest solution is often the most elegant one. With AWS IoT rule substitution templates, that simplicity comes built in.</p> 
<hr /> 
<h3>About the Authors</h3> 
<p><strong><img alt="" class="alignleft size-full wp-image-17221" height="184" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/09/11/ansichel.png" width="150" /><a href="https://www.linkedin.com/in/andreasichel/">Andrea Sichel</a> </strong>is a Principal Specialist IoT Solutions Architect at Amazon Web Services, where he helps customers navigate their cloud adoption journey in the IoT space. Driven by curiosity and a customer-first mindset, he works on developing innovative solutions while staying at the forefront of cloud technology. Andrea enjoys tackling complex challenges and helping organizations think big about their IoT transformations. Outside of work, Andrea coaches his son’s soccer team and pursues his passion for photography. When not behind the camera or on the soccer field, you can find him swimming laps to stay active and maintain a healthy work-life balance.</p> 
<p><img alt="" class="alignleft size-full wp-image-17222" height="184" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/09/11/avinash.png" width="150" /><a href="https://www.linkedin.com/in/avinashupadhyaya/"><strong>Avinash Upadhyaya</strong></a> is Senior Product Manager for AWS IoT Core where he is responsible to define product strategy, roadmap prioritization, pricing, and a go-to-market strategy for features within the AWS IoT service.</p>
