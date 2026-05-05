---
title: "Deploying Small Language Models at Scale with AWS IoT Greengrass and Strands Agents"
url: "https://aws.amazon.com/blogs/iot/deploying-small-language-models-at-scale-with-aws-iot-greengrass-and-strands-agents/"
date: "Fri, 19 Dec 2025 17:09:34 +0000"
author: "Ozan Cihangir"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<h1>Introduction</h1> 
<p>Modern manufacturers face an increasingly complex challenge: implementing intelligent decision-making systems that respond to real-time operational data while maintaining security and performance standards. The volume of sensor data and operational complexity demands AI-powered solutions that process information locally for immediate responses while leveraging cloud resources for complex tasks.The industry is at a critical juncture where edge computing and AI converge. Small Language Models (SLMs) are lightweight enough to run on constrained GPU hardware yet powerful enough to deliver context-aware insights. Unlike Large Language Models (LLMs), SLMs fit within the power and thermal limits of industrial PCs or gateways, making them ideal for factory environments where resources are limited and reliability is paramount. For the purpose of this blog post, assume a SLM has approximately 3 to 15 billion parameters.</p> 
<p>This blog focuses on <a href="https://opcfoundation.org/about/opc-technologies/opc-ua/" rel="noopener noreferrer" target="_blank">Open Platform Communications Unified Architecture (OPC-UA)</a> as a representative manufacturing protocol. OPC-UA servers provide standardized, real-time machine data that SLMs running at the edge can consume, enabling operators to query equipment status, interpret telemetry, or access documentation instantly—even without cloud connectivity.</p> 
<p>AWS IoT Greengrass enables this hybrid pattern by deploying SLMs together with <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/run-lambda-functions.html" rel="noopener noreferrer" target="_blank">AWS Lambda functions</a> directly to OPC-UA gateways. Local inference ensures responsiveness for safety-critical tasks, while the cloud handles fleet-wide analytics, multi-site optimization, or model retraining under stronger security controls.</p> 
<p>This hybrid approach opens possibilities across industries. <a href="https://arxiv.org/abs/2501.02342" rel="noopener noreferrer" target="_blank">Automakers could run SLMs in vehicle compute units for natural voice commands and enhanced driving experience</a>. Energy providers could process SCADA sensor data locally in substations. In gaming, <a href="https://aws.amazon.com/blogs/gametech/revolutionizing-games-with-small-language-model-ai-companions/" rel="noopener noreferrer" target="_blank">SLMs could run on players’ devices to power companion AI in games</a>. Beyond manufacturing, <a href="https://www.unesco.org/en/articles/small-language-models-slms-cheaper-greener-route-ai" rel="noopener noreferrer" target="_blank">higher education institutions could use SLMs to provide personalized learning, proofreading, research assistance and content generation</a>.</p> 
<p>In this blog, we will look at how to deploy SLMs to the edge seamlessly and at scale using AWS IoT Greengrass.</p> 
<h1>Solution overview</h1> 
<p>The solution uses AWS IoT Greengrass to deploy and manage SLMs on edge devices, with Strands Agents providing local agent capabilities. The services used include:</p> 
<ul> 
 <li><a href="https://aws.amazon.com/greengrass/" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass</a>: An open-source edge software and cloud service that lets you deploy, manage and monitor device software.</li> 
 <li><a href="https://aws.amazon.com/iot-core/" rel="noopener noreferrer" target="_blank">AWS IoT Core</a>: Service enabling you to connect IoT devices to AWS cloud.</li> 
 <li><a href="https://aws.amazon.com/s3/" rel="noopener noreferrer" target="_blank">Amazon Simple Storage Service (S3</a>): A highly scalable object storage which lets you to store and retrieve any amount of data.</li> 
 <li><a href="https://strandsagents.com/" rel="noopener noreferrer" target="_blank">Strands Agents</a>: A lightweight Python framework for running multi-agent systems using cloud and local inference.</li> 
</ul> 
<p>We demonstrate the agent capabilities in the code sample using an industrial automation scenario. We provide an OPC-UA simulator which defines a factory consisting of an oven and a conveyor belt as well as maintenance runbooks as the source of the industrial data. This solution can be extended to other use cases by using other agentic tools.The following diagram shows the high-level architecture:</p> 
<p><img alt="AWS IoT Greengrass workflow for edge-based language model deployment using Strands Agents and Ollama" class="alignnone size-full wp-image-17288" height="866" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/11/28/IOTB-901-slm-at-edge.png" width="1451" /></p> 
<ol> 
 <li>User uploads a model file in GPT-Generated Unified Format (GGUF) format to an <a href="https://aws.amazon.com/s3/" rel="noopener noreferrer" target="_blank">Amazon S3</a> bucket which <a href="https://aws.amazon.com/greengrass/" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass</a> devices have access to.</li> 
 <li>The devices in the fleet receive a file download job. <a href="https://github.com/awslabs/aws-greengrass-labs-s3-file-downloader" rel="noopener noreferrer" target="_blank"><em>S3FileDownloader</em></a> component processes this job and downloads the model file to the device from the S3 bucket. The S3FileDownloader component can handle large file sizes, typically needed for SLM model files that exceed the native Greengrass component artifact size limits.</li> 
 <li>The model file in GGUF format is loaded into Ollama when Strands Agents component makes the first call to Ollama. GGUF is a binary file format used for storing LLMs. Ollama is a software which loads the GGUF model file and runs inference. The model name is specified in the recipe.yaml file of the component.</li> 
 <li>The user sends a query to the local agent by publishing a payload to a device specific agent topic in <a href="https://docs.aws.amazon.com/iot/latest/developerguide/mqtt.html" rel="noopener noreferrer" target="_blank">AWS IoT MQTT broker</a>.</li> 
 <li>After receiving the query, the component leverages the <a href="https://github.com/strands-agents/sdk-python" rel="noopener noreferrer" target="_blank">Strands Agents SDK</a>‘s model-agnostic orchestration capabilities. The Orchestrator Agent perceives the query, reasons about the required information sources, and acts by calling the appropriate specialized agents (Documentation Agent, OPC-UA Agent, or both) to gather comprehensive data before formulating a response.</li> 
 <li>If the query is related to an information that can be found in the documentation, Orchestrator Agent calls Documentation Agent.</li> 
 <li>Documentation Agent finds the information from the provided documents and returns it to Orchestrator Agent.</li> 
 <li>If the query is related to current or historic machine data, Orchestrator Agent will call OPC-UA Agent.</li> 
 <li>OPC-UA Agent makes a query to the OPC-UA server depending on the user query and returns the data from server to Orchestrator Agent.</li> 
 <li>Orchestrator Agent forms a response based on the collected information. Strands Agents component publishes the response to a device specific agent response topic in <a href="https://docs.aws.amazon.com/iot/latest/developerguide/mqtt.html" rel="noopener noreferrer" target="_blank">AWS IoT MQTT broker</a>.</li> 
 <li>The <a href="https://github.com/strands-agents/sdk-python" rel="noopener noreferrer" target="_blank">Strands Agents SDK</a> enables the system to work with locally deployed foundation models through Ollama at the edge, while maintaining the option to switch to cloud-based models like those in <a href="https://aws.amazon.com/bedrock/" rel="noopener noreferrer" target="_blank">Amazon Bedrock</a> when connectivity is available.</li> 
 <li>AWS IAM Greengrass service role provides access to the S3 resource bucket to download models to the device.</li> 
 <li>AWS IoT certificate attached to the IoT thing allows Strands Agents component to receive and publish MQTT payloads to <a href="https://aws.amazon.com/iot-core/" rel="noopener noreferrer" target="_blank">AWS IoT Core</a>.</li> 
 <li>Greengrass component logs the component operation to the local file system. Optionally, <a href="https://aws.amazon.com/cloudwatch/" rel="noopener noreferrer" target="_blank">AWS CloudWatch</a> logs can be enabled to monitor the component operation in the CloudWatch console.</li> 
</ol> 
<h1>Prerequisites</h1> 
<p>Before starting this walkthrough, ensure you have:</p> 
<ul> 
 <li>An <a href="https://portal.aws.amazon.com/billing/signup" rel="noopener noreferrer" target="_blank">AWS account</a>.</li> 
 <li>A device running AWS IoT Greengrass and SLMs (e.g., NVIDIA Jetson Orin Nano or <a href="https://docs.aws.amazon.com/dlami/latest/devguide/gpu.html" rel="noopener noreferrer" target="_blank">Amazon Elastic Compute Cloud (EC2) GPU instance</a>). You can learn more about deploying Greengrass in <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/getting-started.html" rel="noopener noreferrer" target="_blank">Tutorial: Getting started with AWS IoT Greengrass</a> documentation. You can use the provided template to deploy a working environment to EC2.</li> 
 <li><a href="https://ollama.com/download/linux" rel="noopener noreferrer" target="_blank">Ollama</a> installed and running on the device.</li> 
 <li>Python 3.10+ installed on the device.</li> 
 <li><a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-development-kit-cli.html" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass Development Kit (GDK) CLI</a> installed.</li> 
 <li>An SLM which supports agents and tool calling (e.g. Qwen 3).</li> 
</ul> 
<h1>Walkthrough</h1> 
<p>In this post, you will:</p> 
<ul> 
 <li>Deploy Strands Agents as an AWS IoT Greengrass component.</li> 
 <li>Download SLMs to edge devices.</li> 
 <li>Test the deployed agent.</li> 
</ul> 
<h2>Component deployment</h2> 
<p>First, let’s deploy the StrandsAgentGreengrass component to your edge device.Clone the Strands Agents repository:</p> 
<div class="hide-language"> 
 <div class="hide-language"> 
  <pre><code class="lang-shell">git clone https://github.com/aws-solutions-library-samples/guidance-for-deploying-ai-agents-to-device-fleets-using-aws-iot-greengrass.git
cd guidance-for-deploying-ai-agents-to-device-fleets-using-aws-iot-greengrass</code></pre> 
 </div> 
</div> 
<p>Use Greengrass Development Kit (GDK) to build and publish the component:</p> 
<p>To publish the component, you need to modify the region and bucket values in <em>gdk-config.json</em> file. The recommended artifact bucket value is <em>greengrass-artifacts</em>. GDK will generate a bucket in <em>greengrass-artifacts-&lt;region&gt;-&lt;account-id&gt; </em>format, if it does not exist already. You can refer to<a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/gdk-cli-configuration-file.html#gdk-config-examples" rel="noopener noreferrer" target="_blank"> Greengrass Development Kit CLI configuration file</a> documentation for more information. After modifying the bucket and region values, run the following commands to build and publish the component.</p> 
<div class="hide-language"> 
 <div class="hide-language"> 
  <pre><code class="lang-shell">gdk component build
gdk component publish</code></pre> 
 </div> 
</div> 
<p>The component will appear in the AWS IoT Greengrass Components Console. You can refer to <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/deploy-first-component.html" rel="noopener noreferrer" target="_blank">Deploy your component</a> documentation to deploy the component to your devices.</p> 
<p>After the deployment, the component will run on the device. It consists of Strands Agents, an OPC-UA simulation server and sample documentation. Strands Agents uses Ollama server as the SLM inference engine. The component has OPC-UA and documentation tools to retrieve the simulated real-time data and sample equipment manuals to be used by the agent.</p> 
<p>If you want to test the component in an Amazon EC2 instance, you can use <em>IoTResources.yaml</em> Amazon CloudFormation template to deploy a GPU instance with necessary software installed. This template also creates resources for running Greengrass. After the deployment of the stack, a Greengrass Core device will appear in the <a href="https://console.aws.amazon.com/iot/home?#/greengrass/v2/cores" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass console</a>. The CloudFormation stack can be found under <em>source/cfn</em> folder in the repository. You can read how to deploy a CloudFormation stack in <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html" rel="noopener noreferrer" target="_blank">Create a stack from the CloudFormation console</a> documentation.</p> 
<h2>Downloading the model file</h2> 
<p>The component needs a model file in GGUF format to be used by Ollama as the SLM. You need to copy the model file under <code>/tmp/destination/</code> folder in the edge device. The model file name must be model.gguf, if you use the default ModelGGUFName parameter in the recipe.yaml file of the component.</p> 
<p>If you don’t have a model file in GGUF format, you can download one from Hugging Face, for example <a href="https://huggingface.co/Qwen/Qwen3-1.7B-GGUF/blob/main/Qwen3-1.7B-Q8_0.gguf" rel="noopener noreferrer" target="_blank">Qwen3-1.7B-GGUF</a>. In a real-world application, this can be a fine-tuned model which solves specific business problems for your use case.</p> 
<h2>(Optional) Use S3FileDownloader to download model files</h2> 
<p>To manage model distribution to edge devices at scale, you can use the <a href="https://github.com/awslabs/aws-greengrass-labs-s3-file-downloader" rel="noopener noreferrer" target="_blank">S3FileDownloader</a> AWS IoT Greengrass component. This component is particularly valuable for deploying large files in environments with unreliable connectivity, as it supports automatic retry and resume capabilities. Since the model files can be large, and device connectivity is not reliable in many IoT use cases, this component can help you to deploy models to your device fleets reliably.</p> 
<p>After deploying S3FileDownloader component to your device, you can publish the following payload to <code>things/&lt;MyThingName&gt;/download</code> topic by using <a href="https://docs.aws.amazon.com/iot/latest/developerguide/view-mqtt-messages.html" rel="noopener noreferrer" target="_blank">AWS IoT MQTT Test Client</a>. The file will be downloaded from the Amazon S3 bucket and put into <code>/tmp/destination/</code> folder in the edge device:</p> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
&nbsp; &nbsp;&nbsp;"jobId": "filedownload",
&nbsp; &nbsp;&nbsp;"s3Bucket": "&lt;ModelFileBucket&gt;",
&nbsp; &nbsp;&nbsp;"key":"model.gguf"
}</code></pre> 
</div> 
<p>If you used the CloudFormation template provided in the repository, you can use the S3 bucket created by this template. Refer to the output of the CloudFormation stack deployment to view the name of the bucket.</p> 
<h2>Testing the local agent</h2> 
<p>Once the deployment is complete and the model is downloaded, we can test the agent through the <a href="https://console.aws.amazon.com/iot/home#/test" rel="noopener noreferrer" target="_blank">AWS IoT Core MQTT Test Client</a>. Steps:</p> 
<ol> 
 <li>Subscribe to <code>things/&lt;MyThingName&gt;/#</code> topic to view the response of the agent.</li> 
 <li>Publish a test query to the input topic <code>things/&lt;MyThingName&gt;/agent/query</code>:</li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
&nbsp; &nbsp;&nbsp;"query": "What is the status of the conveyor belt?"
}</code></pre> 
</div> 
<ol start="3"> 
 <li>You should receive responses on multiple topics: 
  <ol type="a"> 
   <li>Final response topic (<code>things/&lt;MyThingName&gt;/agent/response</code>) which contains the final response of the Orchestrator Agent:</li> 
  </ol> </li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
&nbsp; &nbsp;&nbsp;"query": "What is the status of the oven?",
&nbsp; &nbsp;&nbsp;"response": "The oven is currently operating at 802.2°F (slightly above the setpoint of 800.0°F), with heating active...",
&nbsp; &nbsp;&nbsp;"timestamp": 1757677413.6358254,
&nbsp; &nbsp;&nbsp;"status": "success"
}</code></pre> 
</div> 
<ol> 
 <li> 
  <ol start="2" type="a"> 
   <li>Sub-agent responses (<code>things/&lt;MyThingName&gt;/agent/subagent</code>) which contains the response from intermediary agents such as OPC-UA Agent and Documentation Agent:</li> 
  </ol> </li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-json">{
&nbsp; &nbsp;&nbsp;"agent": "opc factory",
&nbsp; &nbsp;&nbsp;"query": "Get current oven status",
&nbsp; &nbsp;&nbsp;"response": "**Oven Status Report:**\n- **Current Temperature:** 802.2°F...",
&nbsp; &nbsp;&nbsp;"timestamp": 1757677323.443954
}</code></pre> 
</div> 
<p>The agent will process your query using the local SLM and provide responses based on both the OPC-UA simulated data and the equipment documentation stored locally.For demonstration purposes, we use the AWS IoT Core MQTT test client as a straightforward interface to communicate with the local device. In production, Strands Agents can run fully on the device itself, eliminating the need for any cloud interaction.</p> 
<h2>Monitoring the component</h2> 
<p>To monitor the component’s operation, you can connect remotely to your AWS IoT Greengrass device and check the component logs:</p> 
<div class="hide-language"> 
 <pre><code class="lang-shell">sudo tail -f /greengrass/v2/logs/com.strands.agent.greengrass.log</code></pre> 
</div> 
<p>This will show you the real-time operation of the agent, including model loading, query processing, and response generation. You can learn more about Greengrass logging system in <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/monitor-logs.html" rel="noopener noreferrer" target="_blank">Monitor AWS IoT Greengrass logs</a> documentation.</p> 
<h1>Cleaning up</h1> 
<p>Go to <a href="http://console.aws.amazon.com/iot/home?#/greengrassIntro" rel="noopener noreferrer" target="_blank">AWS IoT Core Greengrass console</a> to delete the resources created in this post:</p> 
<ol> 
 <li>Go to Deployments, choose the deployment that you used for deploying the component, then revise the deployment by removing the Strands Agents component.</li> 
 <li>If you have deployed <em>S3FileDownloader</em> component, you can remove it from the deployment as explained in the previous step.</li> 
 <li>Go to Components, choose the Strands Agents component and choose ‘Delete version’ to delete the component.</li> 
 <li>If you have created <em>S3FileDownloader</em> component, you can delete it as explained in the previous step.</li> 
 <li>If you deployed the CloudFormation stack to run the demo in an EC2 instance, delete the stack from <a href="https://console.aws.amazon.com/cloudformation/" rel="noopener noreferrer" target="_blank">AWS CloudFormation console</a>. Note that the EC2 instance will incur hourly charges until it is stopped or terminated.</li> 
 <li>If you don’t need the Greengrass core device, you can delete it from Core devices section of Greengrass console.</li> 
 <li>After deleting Greengrass Core device, delete the IoT certificate attached to the core thing. To find the thing certificate, go to <a href="https://console.aws.amazon.com/iot/home?#/thinghub" rel="noopener noreferrer" target="_blank">AWS IoT Things console</a>, choose the IoT thing created in this guide, view the Certificates tab, choose the attached certificate, choose Actions, then choose Deactivate and Delete.</li> 
</ol> 
<h1>Conclusion</h1> 
<p>In this post, we showed how to run a SLM locally using Ollama integrated through Strands Agents on AWS IoT Greengrass. This workflow demonstrated how lightweight AI models can be deployed and managed on constrained hardware while benefiting from cloud integration for scale and monitoring. Using OPC-UA as our manufacturing example, we highlighted how SLMs at the edge enable operators to query equipment status, interpret telemetry, and access documentation in real time—even with limited connectivity. The hybrid model ensures critical decisions happen locally, while complex analytics and retraining are handled securely in the cloud.This architecture can be extended to create a hybrid cloud-edge AI agent system, where edge AI agents (using AWS IoT Greengrass) seamlessly integrate with cloud-based agents (using Amazon Bedrock). This enables distributed collaboration: edge agents manage real-time, low-latency processing and immediate actions, while cloud agents handle complex reasoning, data analytics, model refinement, and orchestration.</p> 
<hr /> 
<h3>About the authors</h3> 
<p style="clear: both;"><img alt="" class="size-full wp-image-4649 alignleft" height="133" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/11/28/ozanci-1.jpg" width="100" /><strong>Ozan Cihangir</strong> is a Senior Prototyping Engineer at AWS Specialists &amp; Partners Organization. He helps customers to build innovative solutions for their emerging technology projects in the cloud.</p> 
<p style="clear: both;"><img alt="" class="size-full wp-image-4648 alignleft" height="133" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/11/28/luisgrac-1.jpg" width="100" /><strong> Luis Orus</strong> is a senior member of the AWS Specialists &amp; Partners Organization, where he has held multiple roles – from building high-performing teams at global scale to helping customers innovate and experiment quickly through prototyping.</p> 
<p style="clear: both;"><img alt="" class="size-full wp-image-4649 alignleft" height="133" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/11/28/majlesia-1.jpg" width="100" /><strong>Amir Majlesi</strong> leads the EMEA prototyping team within AWS Specialists &amp; Partners Organization. He has extensive experience in helping customers accelerate cloud adoption, expedite their path to production and foster a culture of innovation. Through rapid prototyping methodologies, Amir enables customer teams to build cloud native applications, with a focus on emerging technologies such as Generative &amp; Agentic AI, Advanced Analytics, Serverless and IoT.</p> 
<p style="clear: both;"><img alt="" class="size-full wp-image-4648 alignleft" height="133" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/11/28/jaimestb.png" width="100" /><strong>Jaime Stewart</strong> focused his Solutions Architect Internship within AWS Specialists &amp; Partners Organization around Edge Inference with SLMs. Jaime currently pursues a MSc in Artificial Intelligence.</p>
