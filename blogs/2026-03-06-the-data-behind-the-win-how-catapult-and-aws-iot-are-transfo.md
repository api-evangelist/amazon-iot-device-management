---
title: "The data behind the win: How Catapult and AWS IoT are transforming pro sports"
url: "https://aws.amazon.com/blogs/iot/the-data-behind-the-win-how-catapult-and-aws-iot-are-transforming-pro-sports/"
date: "Fri, 06 Mar 2026 01:58:59 +0000"
author: "Greg Breen"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<p>In professional sports, the difference between winning and losing often comes down to fine margins. Teams across the globe are turning to data-driven insights to optimize athlete performance, mitigate injuries, and gain competitive advantages. <a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-1.png"><img alt="Catapult logo" class="size-full wp-image-17469 alignright" height="39" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-1.png" width="150" /></a><a href="https://www.catapult.com/" rel="noopener noreferrer" target="_blank">Catapult Sports</a>, is a sports technology company that empowers professional teams to make data-driven decisions. By using <a href="https://aws.amazon.com/iot/" rel="noopener noreferrer" target="_blank">AWS IoT</a> services, Catapult is transforming how teams collect, analyze, and act on their data.</p> 
<p>Catapult powers the data-driven insights that professional teams need to optimize athlete health, reduce injuries, and excel in the science of human movement. With over 500 staff across 24 locations worldwide, Catapult serves more than 5,000 professional teams in 40+ sports across 128 countries. This includes top franchises in the NFL, NHL, and English Premier League.</p> 
<h2>The challenge: Meeting the demands of elite sports</h2> 
<p>Professional sports teams operate in high-pressure environments. They rely on athlete-monitoring technology that generates real-time insights during games and practices, providing athletes and coaches with valuable information to improve performance. To successfully adopt such technology, teams have fundamental expectations: instant deployment, seamless updates, remote management across global operations, and zero tolerance for device failures or configuration errors during critical moments.</p> 
<p>As sports analytics becomes increasingly sophisticated, teams also demand faster access to new features, enhanced algorithms, and improved capabilities that can deliver competitive advantages. The challenge for sports technology providers is delivering enterprise-grade reliability while maintaining the agility to innovate rapidly.</p> 
<h2>The solution: Vector 8 powered by AWS IoT</h2> 
<p>To meet these demanding requirements, Catapult developed <a href="https://www.catapult.com/blog/vector-8-introduction" rel="noopener noreferrer" target="_blank">Vector 8</a>, its next-generation athlete monitoring solution built on AWS IoT services. The Vector 8 suite consists of four main components:</p> 
<table cellpadding="10px" class="styled-table"> 
 <tbody> 
  <tr> 
   <td style="padding: 10px;"><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-2.png"><img alt="Vector 8 tag" class="aligncenter size-full wp-image-17454" height="150" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-2.png" width="80" /></a></td> 
   <td style="padding: 10px;"><strong>Vector 8 Tag</strong>: A compact, rugged wearable device that athletes wear during training and competition. It offers precise location tracking indoors and outdoors, captures player movements and sports-specific events, supports integration with third-party Bluetooth sensors, and uses ultra-wideband (UWB) communication. Each tag offers up to six hours of battery life.</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;"><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-3.png"><img alt="Vector 8 dock" class="aligncenter size-full wp-image-17455" height="163" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-3.png" width="289" /></a></td> 
   <td style="padding: 10px;"><strong>Vector 8 Dock</strong>: Used post-session to recharge tags and synchronize data. The dock enables fast Wi-Fi downloads and data syncing directly to the cloud, with a 30-tag capacity and parallel uploading capabilities that dramatically speed up time-to-data.</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;"><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-4.png"><img alt="Vector 8 receiver" class="aligncenter size-full wp-image-17456" height="151" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-4.png" width="129" /></a></td> 
   <td style="padding: 10px;"><strong>Vector 8 Receiver</strong>: Connects through Wi-Fi and Ethernet to stream live data to the cloud for real-time analysis during games and practice sessions. It supports both online and offline workflows and provides real-time diagnostics for performance monitoring and troubleshooting.</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px;"><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-5.png"><img alt="Vector 8 relay" class="aligncenter size-full wp-image-17457" height="57" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-5.png" width="57" /></a></td> 
   <td style="padding: 10px;"><strong>Vector 8 Relay</strong>: Extends coverage across larger venues with a 400-meter range, removing the need for multiple receivers and making scalable deployments efficient and cost-effective.</td> 
  </tr> 
 </tbody> 
</table> 
<h3>Real-time performance insights</h3> 
<p>Vector 8 delivers three types of data to coaches and sports scientists:</p> 
<ol> 
 <li><strong>Device health data</strong> – Live telemetry, including battery status and received signal strength indication (RSSI), plus live device configuration, such as firmware versions. Transmitted in real-time, enabling proactive intervention before devices fail during critical moments.</li> 
 <li><strong>Hot data</strong> – Live in-game or in-practice data sampled at 10 Hz, including acceleration, velocity, player positioning, and heart rate monitoring. Coaches see instant visualizations in iOS apps, enabling in-the-moment interventions, such as substituting fatigued players or adjusting tactics based on movement patterns.</li> 
 <li><strong>Cold data</strong> – Post-game raw inertial data sampled at 100 Hz, used for machine learning (ML) inference to automatically detect sports-specific movements like jumps, tackles, and throws.</li> 
</ol> 
<p>Catapult’s AI-driven analytics can generate 600 unique metrics per player per game, providing deep insights into athlete performance, workload management, and injury prevention. With the integration with <a href="https://www.catapult.com/solutions/video-analysis" rel="noopener noreferrer" target="_blank">Catapult’s video analysis solutions</a>, coaches can align physical output data with actual game footage, transforming how teams analyze and improve performance.</p> 
<h2>AWS IoT architecture</h2> 
<p>At the heart of Vector 8’s connectivity is <a href="https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html" rel="noopener noreferrer" target="_blank">AWS IoT Core</a>. AWS IoT Core provides a scalable and highly available cloud endpoint for authentication, authorization, encryption in transit, and device management at scale. The AWS IoT Core <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html" rel="noopener noreferrer" target="_blank">rules engine</a> serves as the integration point that routes data from thousands of devices to the appropriate AWS services over our high-bandwidth, low-latency, robust network infrastructure.</p> 
<p><a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/what-is-iot-greengrass.html" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass</a> is the foundation of the Vector 8 dock and receiver software architecture. AWS IoT Greengrass provides both an edge runtime and a cloud service that helps Catapult build, deploy, and manage multi-process IoT applications at scale. The <a href="https://github.com/aws-greengrass/aws-greengrass-nucleus" rel="noopener noreferrer" target="_blank">open-source edge runtime</a> manages Catapult’s <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/greengrass-components.html" rel="noopener noreferrer" target="_blank">custom components</a> alongside <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/public-components.html" rel="noopener noreferrer" target="_blank">AWS-provided components</a>, handling component versioning, <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-deployments.html#component-dependency-resolution" rel="noopener noreferrer" target="_blank">dependency resolution</a>, <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/logging-and-monitoring.html" rel="noopener noreferrer" target="_blank">logging</a>, and <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/interprocess-communication.html" rel="noopener noreferrer" target="_blank">interprocess communication</a>.</p> 
<div class="wp-caption aligncenter" id="attachment_17458" style="width: 1441px;">
 <a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-6.png"><img alt="Figure 1: Device health and hot data ingestion with the Catapult Receiver" class="size-full wp-image-17458" height="624" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-6.png" width="1431" /></a>
 <p class="wp-caption-text" id="caption-attachment-17458">Figure 1: Device health and hot data ingestion with the Catapult Receiver</p>
</div> 
<p>For real-time data streaming, Catapult uses <a href="https://docs.aws.amazon.com/msk/latest/developerguide/what-is-msk.html" rel="noopener noreferrer" target="_blank">Amazon Managed Streaming for Apache Kafka (Amazon MSK)</a> to buffer and process device health telemetry and live athlete performance data. <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html" rel="noopener noreferrer" target="_blank">Amazon Elastic Container Service (Amazon ECS)</a> runs containerized analytics services that process battery levels, firmware versions, and other diagnostics in real time. Results are exposed through <a href="https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html" rel="noopener noreferrer" target="_blank">Amazon API Gateway</a> and delivered to Catapult’s web interface and mobile apps, where coaches and equipment managers monitor device status.</p> 
<div class="wp-caption aligncenter" id="attachment_17459" style="width: 1440px;">
 <a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-7.png"><img alt="Figure 2: Cold data ingestion with the Catapult Dock" class="size-full wp-image-17459" height="492" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-7.png" width="1430" /></a>
 <p class="wp-caption-text" id="caption-attachment-17459">Figure 2: Cold data ingestion with the Catapult Dock</p>
</div> 
<p>Post-game data flows to <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html" rel="noopener noreferrer" target="_blank">Amazon Simple Storage Service (Amazon S3</a>), where raw 100 Hz inertial data is stored for machine learning inference. A two-hour session with 30 athletes can be uploaded to Amazon S3 in less than 30 seconds, representing 36 million data points from a typical NHL game.</p> 
<h2>Key benefits and outcomes with AWS IoT</h2> 
<h3>Seamless onboarding experience</h3> 
<p>Vector 8 delivers an out-of-the-box experience that gets teams up and running in minutes. As Mike Lee, Principal Product Manager at Catapult, explains:</p> 
<blockquote>
 <p><em>“With Vector 8, customers simply download the Catapult Vector iOS app, unbox the hardware, and follow a guided in-app registration flow, and that’s it. Their full system connected, configured, and updated in less than 10 minutes.”</em></p>
</blockquote> 
<p>Teams can start tracking athletes within minutes of receiving their equipment through this streamlined self-service onboarding experience. No technical support or complex setup is required. The automated process configures devices correctly and runs the latest firmware immediately.</p> 
<h3>Fast device updates</h3> 
<p>Over-the-air (OTA) updates through <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-deployments.html" rel="noopener noreferrer" target="_blank">AWS IoT Greengrass deployments</a> have transformed how Catapult delivers new capabilities to customers. Mike Lee highlights the improvement:</p> 
<blockquote>
 <p><em>“One of the biggest game changers with Vector 8 is how we handle updates. By moving to automated OTA updates with AWS IoT Greengrass, we’re able to update devices up to 32 times faster than previous generations, saving customers a huge amount of time and operational hassle.”</em></p>
</blockquote> 
<p>By moving to automated updates delivered through AWS IoT Greengrass deployments, Catapult has achieved a level of fleet-wide consistency that wasn’t possible with previous generations. Mike Lee emphasizes the significance of this shift:</p> 
<blockquote>
 <p><em>“We have never been in the position where we’ve had almost the entire wearable device fleet on the latest firmware. That’s going to help us avoid hundreds of support tickets.”</em></p>
</blockquote> 
<p>This near-universal fleet currency means that all customers benefit from the latest features, performance improvements, and bug fixes automatically, with no manual intervention required.</p> 
<h3>Enabling rapid innovation</h3> 
<p>The reliable, high-performance over-the-air update capability has fundamentally changed how Catapult’s firmware team operates. Mike Lee describes the transformation:</p> 
<blockquote>
 <p><em>“For the first time, we can safely push changes to devices as soon as they’re ready. What used to take months to reach customers can now be delivered in weeks, or even days, especially when we’re iterating on new features or running beta programs.”</em></p>
</blockquote> 
<p>This acceleration enables Catapult to iterate quickly on new features based on customer feedback, delivering value faster and staying ahead of the competition. The shift from months-long release cycles to weekly or even daily releases represents a fundamental change in how the company innovates.</p> 
<h3>Proactive device management and diagnostics</h3> 
<p>AWS IoT <a href="https://docs.aws.amazon.com/iot/latest/developerguide/secure-tunneling.html" rel="noopener noreferrer" target="_blank">secure tunneling</a>, coupled with the AWS IoT Greengrass <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/secure-tunneling-component.html" rel="noopener noreferrer" target="_blank">secure tunneling component</a>, enables Catapult’s support team to provide world-class service. Support engineers can establish SSH sessions to docks and receivers on demand, with no customer involvement beyond consent. With the AWS IoT Greengrass <a href="https://docs.aws.amazon.com/greengrass/v2/developerguide/log-manager-component.html" rel="noopener noreferrer" target="_blank">log manager component</a>, device logs automatically flow to <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html" rel="noopener noreferrer" target="_blank">Amazon CloudWatch</a> in near real-time, enabling proactive issue identification and resolution before customers notice problems.</p> 
<p>This remote access capability transforms support operations from reactive troubleshooting to proactive monitoring. Issues that previously required scheduling customer appointments and complex file transfers can now be diagnosed and resolved in minutes.</p> 
<h3>Intelligent configuration management</h3> 
<p>AWS IoT <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html" rel="noopener noreferrer" target="_blank">Device Shadows</a> enable Catapult to manage device configurations from the cloud without requiring physical connections. When coaches assign athletes to devices or update performance thresholds through Catapult’s mobile app, those configuration changes automatically sync to the cloud and propagate to all registered docks. The next time a tag is placed in any dock, it receives the latest configuration, ensuring consistency across the entire device fleet.</p> 
<p>This capability has removed thousands of manual device staging operations. Replacement devices automatically receive the correct customer configuration upon the first connection, enabling true hot-swap functionality that keeps teams operational even when hardware is replaced.</p> 
<h2>Looking ahead</h2> 
<p>With the foundation of cloud connectivity and automated device management in place, Catapult is exploring several future innovations:</p> 
<ul> 
 <li><strong>Live data streaming to the cloud</strong> – Currently, 10 Hz live data streams only to sideline iOS apps. Streaming this data to the cloud would enable more in-game analytics and enable remote access for coaches and sports scientists.</li> 
 <li><strong>Edge machine learning inference</strong> – Most ML inference currently happens in the cloud post-game. Catapult is investigating shifting more inference to the edge, on receivers or even tags, to provide more real-time insights during games and practice sessions. The robust OTA update mechanism enables nearly continuous iteration and enhancement of deployed models.</li> 
 <li><strong>AI-powered team analytics</strong> – Analyzing team-wide tag data to understand team movements, group dynamics, and tactical patterns using advanced AI models.</li> 
 <li><strong>Natural language video search</strong> – Using AI, like multimodal embedding models, to enable <a href="https://aws.amazon.com/blogs/machine-learning/unlocking-video-understanding-with-twelvelabs-marengo-on-amazon-bedrock/" rel="noopener noreferrer" target="_blank">natural language search and understanding of video content</a>. This helps coaches to find specific plays or situations.</li> 
</ul> 
<h2>Conclusion</h2> 
<p>Catapult’s Vector 8 platform demonstrates how AWS IoT services enable sports technology companies to deliver enterprise-grade solutions with consumer-grade simplicity. By achieving 10-minute onboarding, 32x faster updates, and 97% of the fleet running the latest firmware, Catapult has set a new standard for athlete monitoring technology.</p> 
<p>These enhancements allow Catapult to move beyond troubleshooting and toward strategic advancement. Engineering teams can iterate rapidly; their support teams can resolve issues more quickly; and customers can focus on what matters most – optimizing athlete performance and winning games.</p> 
<p>As professional sports become increasingly data-driven, Catapult’s AWS IoT-powered platform positions them to continue leading the transformation of how teams train, compete, and succeed.</p> 
<h2>Learn more</h2> 
<p>To learn more about AWS IoT services and how they can transform your connected device business, visit <a href="https://aws.amazon.com/iot/" rel="noopener noreferrer" target="_blank">aws.amazon.com/iot</a>. To learn more about Catapult, visit <a href="http://catapult.com" rel="noopener noreferrer" target="_blank">catapult.com</a>.</p> 
<p>To dive deeper into this particular use case, watch the recording “<a href="https://youtu.be/Plw5F5LKNnc" rel="noopener" target="_blank">AWS re:Invent 2025 – Peak Performance: IoT Innovation in Professional Sports (SPF301)</a>”.</p> 
<p></p> 
<hr style="width: 80%;" /> 
<h2>About the authors</h2> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Greg Breen" class="size-thumbnail wp-image-10053 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-8.png" width="120" />
  </div> 
  <h3 class="lb-h4">Greg Breen</h3> 
  <p><a href="https://www.linkedin.com/in/gregory-breen/" rel="noopener" target="_blank">Greg Breen</a> is a Senior IoT Specialist Solutions Architect at Amazon Web Services. Based in Australia, he helps customers throughout Asia Pacific to build their IoT solutions. With deep experience in embedded systems, he has a particular interest in assisting product development teams to bring their devices to market.</p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Mike Garbuz" class="size-thumbnail wp-image-10053 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-9.jpeg" width="120" />
  </div> 
  <h3 class="lb-h4">Mike Garbuz</h3> 
  <p><a href="https://www.linkedin.com/in/mgarbuz/" rel="noopener" target="_blank">Mike Garbuz</a> is a Sports Solutions Architect at Amazon Web Services. Based in Australia, he helps sports customers across Australia get the most out of AWS services. With experience in Machine Learning and Data and Analytics, he guides these customers to get the most out of their data through the use of AWS services.</p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Farzad Khodadadi" class="size-thumbnail wp-image-10053 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-10.png" width="120" />
  </div> 
  <h3 class="lb-h4">Farzad Khodadadi</h3> 
  <p><a href="https://www.linkedin.com/in/farzadkhodadadi/" rel="noopener" target="_blank">Farzad Khodadadi</a> is a Lead Principal Software Engineer at Catapult with over 15 years of experience delivering enterprise-scale cloud-native technology solutions and shaping long-term engineering strategy. Since joining the company, he has provided technical leadership for the design and delivery of Catapult’s IoT platform, transitioning vision into scalable, business-ready capabilities. His leadership and passion for IoT has accelerated the adoption of IoT across multiple products and teams, enabling new value streams while supporting Catapult’s broader digital and product strategy.</p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Mike Lee" class="size-thumbnail wp-image-10053 alignleft" height="150" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/03/04/iotblog-7-image-11.png" width="120" />
  </div> 
  <h3 class="lb-h4">Mike Lee</h3> 
  <p><a href="https://www.linkedin.com/in/mike-lee-7312082b/" rel="noopener" target="_blank">Mike Lee</a> is a Principal Product Manager at Catapult, based in Melbourne, Australia. With over 14 years in sports technology and five years working directly in professional sport as a Sports Scientist, Mike combines deep domain knowledge with product leadership to build technology that delivers real-world value for teams, athletes, and performance staff.</p> 
 </div> 
</footer>
