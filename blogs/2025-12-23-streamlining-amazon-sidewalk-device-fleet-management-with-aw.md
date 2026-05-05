---
title: "Streamlining Amazon Sidewalk Device Fleet Management with AWS IoT Core’s New Bulk Operations"
url: "https://aws.amazon.com/blogs/iot/streamlining-amazon-sidewalk-device-fleet-management-with-aws-iot-cores-new-bulk-operations/"
date: "Tue, 23 Dec 2025 19:28:51 +0000"
author: "Ben Cooke"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<p><a href="https://docs.sidewalk.amazon/" rel="noopener noreferrer" target="_blank">Amazon Sidewalk</a> is a shared, community-sourced network that leverages existing Amazon Echo and Ring devices as gateways to provide secure, low-power connectivity for IoT devices—enabling applications ranging from asset tracking and smart home security to remote diagnostics for appliances and tools.</p> 
<p><a href="https://docs.aws.amazon.com/iot-wireless/latest/developerguide/iot-sidewalk.html" rel="noopener noreferrer" target="_blank">AWS IoT Core for Amazon Sidewalk</a> device management is evolving to meet the needs of growing deployments that leverage this community-sourced network. To manage a Sidewalk device fleet, operators need to configure device settings and manage device identities through AWS IoT Core APIs with scale in mind. This has required implementing retry logic, tracking operation outcomes, and understanding API rate limits. As customer deployments scale beyond thousands of devices, there is an opportunity to streamline configuration management across entire fleets and empower teams to manage large-scale deployments with greater ease and confidence.</p> 
<p>Today, we’re excited to announce new bulk management capabilities for AWS IoT Core for Amazon Sidewalk that helps transform how you provision, configure, and manage thousands of devices. With the <a href="https://s12d.com/aws-iot-wireless-device-bulk-management-cdk-package" rel="noopener noreferrer" target="_blank">new AWS Cloud Development Kit (CDK) stack</a> from the AWS IoT Core team, you can now onboard entire manufacturing batches through simple JSON files, update device configurations across your fleet in minutes, and receive detailed operational reports—all while respecting API rate limits and maintaining full visibility through Amazon CloudWatch dashboards. Whether you’re provisioning your first batch of Sidewalk devices or managing updates across an existing fleet, these new capabilities reduce operational overhead from hours to minutes while providing enterprise-grade error handling and reporting.</p> 
<p>The new <strong>‘bulk management solution for Sidewalk device fleets’ </strong>is a CDK app that eliminates the manual overhead of device management operations through AWS IoT Core.</p> 
<p><img alt="AWS IoT architecture diagram showing data flow from S3 input through Step Functions orchestration, parallel Lambda processing, to Aurora database storage with SNS notifications" class="aligncenter wp-image-17354 size-full" height="723" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/12/22/IOTB-911-1.png" width="821" /></p> 
<p style="text-align: center;"><em>Bulk Provisioning AWS CloudFormation Stack for AWS IoT Core for Amazon Sidewalk</em></p> 
<p><strong>Key capabilities:</strong></p> 
<p>The stack delivers five essential capabilities that address the core challenges of fleet management:</p> 
<p><strong>CDK-based deployment for easy setup</strong> – Deploy the entire solution to your account with a single CDK command, customizing behavior through a simple configuration file. No complex infrastructure setup or manual resource provisioning required.</p> 
<p><strong>JSON-based bulk operations</strong> – Define device operations using straightforward JSON files that support both create and update operations. Reference devices by <a href="https://docs.sidewalk.amazon/provisioning/" rel="noopener noreferrer" target="_blank">Sidewalk Manufacturing Serial Number</a> (SMSN) or <a href="https://docs.aws.amazon.com/iot-wireless/latest/developerguide/what-is-iot-wireless.html" rel="noopener noreferrer" target="_blank">AWS IoT Wireless</a> Device ID.</p> 
<p><strong>Real-time monitoring through Amazon CloudWatch</strong> – Track operation progress through purpose-built CloudWatch dashboards that display processing rates, success metrics, and error counts as they happen.</p> 
<p><strong>Automated error handling and reporting</strong> – Receive comprehensive reports distinguishing between retriable and permanent failures, with clear error messages for rapid remediation. The stack automatically retries any failures with exponential backoff.</p> 
<p><strong>Flexible notification options</strong> – Choose your preferred notification channel—Amazon Simple Queue Service (SQS) for queue-based processing, Amazon SNS for event-driven workflows, or Amazon S3-only for simple file-based reporting.</p> 
<p><strong>Three core operations:</strong></p> 
<p>The stack supports three fundamental operations that cover the entire device lifecycle:</p> 
<p><strong>1. Bulk create: </strong>Upload a JSON file containing device configurations including <a href="https://docs.aws.amazon.com/iot-wireless/latest/developerguide/iot-sidewalk-add-device.html" rel="noopener noreferrer" target="_blank">SMSN, device profiles, destinations, and positioning settings</a>. The stack validates inputs, processes devices in parallel while respecting API limits, and generates detailed reports of successful and failed provisioning attempts.</p> 
<p><strong>2. Bulk update: </strong>Update device settings such as positioning status, destination names, or tags across hundreds or thousands of devices simultaneously. The stack automatically looks up devices by SMSN or AWS IoT Wireless Device ID, applies only the specified changes, and maintains a complete audit trail of modifications.</p> 
<p><strong>3. Bulk validation: </strong>Validate JSON structure and field requirements before making any AWS API calls, catching configuration errors early. This prevents partial batch failures and wastes API calls, providing immediate feedback on issues like missing required fields, invalid field formats, or malformed JSON structure.</p> 
<p>Each operation respects your configured API rate limits, provides detailed success/failure reporting, and integrates seamlessly with your existing AWS infrastructure through standard services like Amazon S3, AWS Lambda, and Amazon Aurora.</p> 
<p><strong>How it works:</strong></p> 
<p><strong>Step 1: Sidewalk bulk management stack deployment</strong></p> 
<p>Download the <a href="https://s12d.com/aws-iot-wireless-device-bulk-management-cdk-package" rel="noopener noreferrer" target="_blank"><strong>Sidewalk device bulk management package</strong></a> and extract it on a machine that has AWS credentials for your account<strong>. </strong>You can learn more about configuring security credentials for the AWS CDK CLI <a href="https://docs.aws.amazon.com/cdk/v2/guide/configure-access.html" rel="noopener noreferrer" target="_blank">here</a>.</p> 
<p>Deployment requires just a configuration file and two CDK commands. The CDK app automatically provisions all necessary AWS resources in your account.</p> 
<p>First, install and bootstrap the AWS CDK in your account:</p> 
<pre><code class="lang-bash"># Install CDK globally
npm install -g aws-cdk
# Bootstrap CDK in your AWS account
cdk bootstrap</code></pre> 
<p>Create a <em>config.json</em> file in the directory where you extracted the package to customize the stack for your specific requirements:</p> 
<pre><code class="lang-json">{  
  // Notification channel: "SQS", "SNS", or "NONE" (S3 reports only) 
  "notificationType": "SQS", // SQS configuration (if using SQS) 
  "sqsProperties": { 
    "queueName": "sidewalk-bulk-notifications", 
    "visibilityTimeout": 300 }, 
  // Default API rate limits - adjust based on your AWS IoT Core quotas 
  "createWirelessDeviceApiTps": 10, 
  "getWirelessDeviceApiTps": 10, 
  "updateWirelessDeviceApiTps": 10
}</code></pre> 
<p>Deploy the solution with your configuration:</p> 
<pre><code class="lang-bash">cd aws-iot-wireless-device-bulk-management-cdk-v1.0.0
cdk deploy --parameters-file config.json</code></pre> 
<p>This CDK deployment command creates:</p> 
<ul> 
 <li><strong>Amazon S3 bucket</strong> for uploading device JSON files and storing operation reports</li> 
 <li><strong>AWS Lambda functions</strong> for processing bulk operations with automatic retry logic</li> 
 <li><strong>Amazon Aurora table</strong> integrated with your database cluster for device state management</li> 
 <li><strong>Amazon CloudWatch dashboards</strong> for real-time operation monitoring</li> 
 <li><strong>Notification infrastructure</strong> (Amazon SQS queue or Amazon SNS topic based on your configuration)</li> 
</ul> 
<p>Please note that you will incur AWS charges for using the above-mentioned services. For more information, refer pricing pages of each AWS service listed above. As provided, the stack costs ~$50/mo for quiescent hosting costs primarily driven by the Aurora cluster (0.5 ACU min). The operation of provisioning or updating config on 1M devices would add &lt;$15 in incremental cost.</p> 
<p><strong>Step 2: Device provisioning</strong></p> 
<p>With the stack deployed, you can immediately begin provisioning devices in bulk.Create a JSON file defining your device batch with all necessary configuration:</p> 
<pre><code class="lang-json">{ 
  "operation": "create",
  "batchName": "manufacturing-batch-20250917",
  "devices": [
    {
      "smsn": "SIDEWALK-DEVICE-001",
      "deviceName": "warehouse-sensor-001",
      "deviceProfileId": "prof-a1b2c3d4e5f6",
      "uplinkDestinationName": "warehouse-data-destination",
      "positioning": {
        "enabled": true,
        "positioningDestinationName": "asset-tracking-destination" },
      //optional tags
      "tags": [
       {"key": "location", "value": "warehouse-1"},
       {"key": "type", "value": "temperature-sensor"} ]
    },
    { 
      "smsn": "SIDEWALK-DEVICE-002",
      "deviceName": "warehouse-sensor-002",
      "deviceProfileId": "prof-a1b2c3d4e5f6",
      "uplinkDestinationName": "warehouse-data-destination",
      "positioning": { "enabled": false } }
    // ... additional devices ]
}</code></pre> 
<p>Upload the file to the Amazon S3 bucket, triggering automatic processing:</p> 
<ol> 
 <li><strong>Immediate validation</strong> of JSON structure and required fields.</li> 
 <li><strong>Parallel processing</strong> of devices while respecting API rate limits.</li> 
 <li><strong>Automatic retries</strong> for transient failures with exponential backoff. See retry logic below.</li> 
 <li><strong>Comprehensive reporting</strong> delivered to S3 and your notification channel.</li> 
</ol> 
<p>As processing begins, your CloudWatch dashboard displays:</p> 
<ul> 
 <li>Devices processed per minute</li> 
 <li>Running success/failure counts</li> 
 <li>Current retry queue depth</li> 
 <li>Estimated time to completion</li> 
</ul> 
<p><strong>Step 3: Configuration updates</strong></p> 
<p>To modify device configurations across your fleet without re-provisioning, follow the steps below.</p> 
<p>Reference devices using either their original SMSN or the AWS-assigned Wireless Device ID:</p> 
<pre><code class="lang-json">{ 
  "operation": "update",
  "batchName": "enable-positioning-batch-20250918",
  "devices": [ 
    { // Reference by SMSN
      "smsn": "SIDEWALK-DEVICE-001",
      "positioning": { "enabled": false } },
    { // Reference by AWS Wireless Device ID
      "awsWirelessDeviceId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
      "positioning": { "enabled": true, "positioningDestinationName": "new-tracking-destination" } },
    { // Update multiple properties
      "smsn": "SIDEWALK-DEVICE-003",
      "deviceName": "warehouse-sensor-003-renamed",
      "uplinkDestinationName": "warehouse-data-v2",
      "tags": [ 
        {"key": "firmware", "value": "v2.1.0"},
        {"key": "lastUpdated", "value": "2025-09-18"} ] 
    } 
  ]
}</code></pre> 
<p>The stack supports updating any modifiable device property:</p> 
<ul> 
 <li>Enable/disable positioning capabilities</li> 
 <li>Change uplink or positioning destinations</li> 
 <li>Update device names and tags</li> 
 <li>Modify any other AWS IoT Core supported attributes</li> 
</ul> 
<p>The update process follows the same pattern as creation—upload the JSON file to S3, monitor progress via CloudWatch, and receive detailed reports upon completion. The stack automatically handles device lookups, validates that devices exist before attempting updates, and provides clear error messages for any devices that cannot be modified.</p> 
<p><strong>Best practices:</strong></p> 
<p><strong>Recommended batch sizes</strong> based on configuration maturity –</p> 
<ul> 
 <li><strong>Small batches (100-500 devices)</strong>: Ideal for testing and validation</li> 
 <li><strong>Medium batches (500-2,000 devices)</strong>: Optimal balance of processing time and error isolation</li> 
 <li><strong>Large batches (2,000-10,000 devices)</strong>: Production deployments with well-tested configurations</li> 
</ul> 
<p>Configure TPS limits based on your <a href="https://docs.aws.amazon.com/general/latest/gr/iot-core.html" rel="noopener noreferrer" target="_blank">AWS IoT Core quotas</a> and operational requirements:</p> 
<table border="1px" cellpadding="10px" class="styled-table"> 
 <thead> 
  <tr> 
   <th style="padding: 10px;"><strong>Operation</strong></th> 
   <th style="padding: 10px;"><strong>Default TPS</strong></th> 
   <th style="padding: 10px;"><strong>Recommended Setting</strong></th> 
   <th style="padding: 10px;"><strong>Processing Rate</strong></th> 
  </tr> 
 </thead> 
 <tbody> 
  <tr> 
   <td style="padding: 10px;">Create</td> 
   <td style="padding: 10px;">10</td> 
   <td style="padding: 10px;">8 (80% of limit)</td> 
   <td style="padding: 10px;">~480 devices/min</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">Update</td> 
   <td style="padding: 10px;">10</td> 
   <td style="padding: 10px;">8 (80% of limit)</td> 
   <td style="padding: 10px;">~480 devices/min</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">Get</td> 
   <td style="padding: 10px;">10</td> 
   <td style="padding: 10px;">10 (100% of limit)</td> 
   <td style="padding: 10px;">~600 devices/min</td> 
  </tr> 
 </tbody> 
</table> 
<p>Calculate expected processing time using this formula:</p> 
<p><strong>Time (minutes) = Number of Devices / (TPS * 60) * 1.2</strong></p> 
<p>The 1.2 factor accounts for retries and processing overhead. Example estimates:</p> 
<ul> 
 <li>1,000 devices at 8 TPS: ~2.5 minutes</li> 
 <li>5,000 devices at 8 TPS: ~12.5 minutes</li> 
 <li>10,000 devices at 8 TPS: ~25 minutes</li> 
</ul> 
<p><strong>Error handling –</strong></p> 
<p>Common error codes and their meanings:</p> 
<table border="1px" cellpadding="10px" class="styled-table"> 
 <thead> 
  <tr> 
   <th style="padding: 10px;"><strong>Error code</strong></th> 
   <th style="padding: 10px;"><strong>Meaning</strong></th> 
   <th style="padding: 10px;"><strong>Action required</strong></th> 
  </tr> 
 </thead> 
 <tbody> 
  <tr> 
   <td style="padding: 10px;">ResourceNotFoundException</td> 
   <td style="padding: 10px;">Device profile or destination not found</td> 
   <td style="padding: 10px;">Verify resource exists before retry</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">ThrottlingException</td> 
   <td style="padding: 10px;">API rate limit exceeded</td> 
   <td style="padding: 10px;">Automatic retry with backoff</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">ValidationException</td> 
   <td style="padding: 10px;">Invalid parameter value</td> 
   <td style="padding: 10px;">Fix configuration and retry</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">ConflictException</td> 
   <td style="padding: 10px;">Device already exists</td> 
   <td style="padding: 10px;">Skip or use update operation</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;">InternalServerException</td> 
   <td style="padding: 10px;">Temporary AWS service issue</td> 
   <td style="padding: 10px;">Automatic retry</td> 
  </tr> 
 </tbody> 
</table> 
<p>The stack implements intelligent retry logic:</p> 
<ul> 
 <li><strong>Automatic retries</strong>: Transient errors (throttling, internal errors) retry up to 3 times</li> 
 <li><strong>Exponential backoff</strong>: Wait times of 1s, 2s, 4s between retries</li> 
 <li><strong>Dead letter queue</strong>: Permanent failures logged for manual review</li> 
 <li><strong>Batch isolation</strong>: Failed devices don’t block successful ones</li> 
</ul> 
<p><strong>Validation best practices</strong></p> 
<ul> 
 <li><strong>Test with small batches</strong> before processing thousands of devices</li> 
 <li><strong>Validate device profiles exist</strong> using AWS CLI or Console before bulk operations</li> 
 <li><strong>Use consistent naming conventions</strong> for easier troubleshooting</li> 
 <li><strong>Include meaningful batch names</strong> for operation tracking</li> 
 <li><strong>Verify JSON syntax</strong> using a JSON validator before upload</li> 
 <li><strong>Check required fields</strong> match your device profile requirements</li> 
</ul> 
<p><strong>Conclusion</strong></p> 
<p>AWS IoT Core’s new bulk management stack for Amazon Sidewalk fundamentally helps transform how organizations deploy and manage IoT devices at scale. By replacing manual API calls and custom scripts with a robust, CDK-deployable solution, teams can now provision thousands of devices in minutes rather than hours or days. This represents a significant step forward for IoT teams looking to scale their device deployments efficiently. By leveraging AWS IoT Core for Amazon Sidewalk’s bulk provisioning features, you can onboard devices using the AWS IoT console, API operations, or AWS CLI commands—with the flexibility to add devices individually or via CSV files stored in Amazon S3For IoT operations teams, these capabilities translate directly into reduced operational overhead by making it easier to securely onboard, organize, monitor, and remotely manage Sidewalk devices at scale throughout their lifecycle. Combined with built-in monitoring, teams gain the operational visibility needed to maintain reliable Sidewalk device fleets. With these new capabilities now available, your team can shift focus from managing provisioning infrastructure to building the innovative IoT solutions that drive your business forward—letting AWS handle the complexity of scaling your Sidewalk device fleet from hundreds to millions.</p> 
<p><strong>Additional Resources</strong></p> 
<ul> 
 <li><a href="https://s12d.com/aws-iot-wireless-device-bulk-management-cdk-package" rel="noopener noreferrer" target="_blank">Sidewalk device bulk management CDK package</a></li> 
 <li><a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-sidewalk.html" rel="noopener noreferrer" target="_blank">AWS IoT Core for Amazon Sidewalk Documentation</a></li> 
 <li><a href="https://developer.amazon.com/sidewalk" rel="noopener noreferrer" target="_blank">Amazon Sidewalk Developer Portal</a></li> 
 <li><a href="https://docs.aws.amazon.com/cdk/latest/guide/" rel="noopener noreferrer" target="_blank">AWS Cloud Development Kit (CDK) Documentation</a></li> 
 <li><a href="https://docs.aws.amazon.com/iot-wireless/2020-11-22/apireference/" rel="noopener noreferrer" target="_blank">AWS IoT Core API Reference</a></li> 
 <li><a href="https://repost.aws/topics/TAgOdRefu7TDi_D2aHBGFnhA/aws-iot-core" rel="noopener noreferrer" target="_blank">Support: AWS re:Post IoT Community</a></li> 
</ul> 
<h3>About the authors</h3> 
<p>
 <!-- First Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="IOTB-911-2.jpg"><img alt="" class="wp-image-17378 size-full alignleft" height="125" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/12/23/IOTB-911-2.jpeg" width="125" /></p> 
 <h3 class="lb-h4">Ben Cooke</h3> 
 <p style="color: #879196; font-size: 1rem;">Ben is a Senior Partner Solutions Architect at Amazon Web Services, helping partners and customers create innovative solutions for the IoT, Games and Media &amp; Entertainment industries. With over 20 years of technology experience spanning embedded systems, cloud architecture, and technical sales&nbsp;roles, Ben brings deep technical expertise to solving complex industry challenges. Outside of work, he enjoys adventures with his family and all things automotive.</p> 
</div> 
<p>
 <!-- Second Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="IOTB-911-3.jpg"><img alt="" class="wp-image-17379 size-full alignleft" height="125" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/12/23/IOTB-911-3.jpeg" width="125" /></p> 
 <h3 class="lb-h4">Calvin Li (李一晗)</h3> 
 <p style="color: #879196; font-size: 1rem;">Calvin is a Senior Software Development Engineer in the AWS IoT team based in Seattle, WA, specializing in IoT device connectivity, location services, and scalable architectures that support millions of connected devices. When not working, he enjoys exploring new technology and traveling with his family.</p> 
</div> 
<p>
 <!-- Third Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="IOTB-911-4.jpg"><img alt="" class="wp-image-17380 size-full alignleft" height="125" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/12/23/IOTB-911-4.jpeg" width="125" /></p> 
 <h3 class="lb-h4">Kexin Zhang (张珂昕)</h3> 
 <p style="color: #879196; font-size: 1rem;">Kexin is a Software Engineer in AWS IoT team based in Seattle, where she helps build scalable IoT applications from prototype to production. When not connecting devices to the cloud, she disconnects by hiking, swimming, and solving puzzles that don’t require debugging.</p> 
</div>
