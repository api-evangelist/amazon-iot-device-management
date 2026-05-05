---
title: "Optimize long-term video storage costs with Amazon Kinesis Video Streams warm storage tier"
url: "https://aws.amazon.com/blogs/iot/optimize-long-term-video-storage-costs-with-amazon-kinesis-video-streams-warm-storage-tier/"
date: "Sun, 04 Jan 2026 15:26:42 +0000"
author: "Jinseon Lee"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<h2>Introduction</h2> 
<p><a href="https://aws.amazon.com/kinesis/video-streams/" rel="noopener noreferrer" target="_blank">Amazon Kinesis Video Streams</a> provides powerful solutions for cloud-based video management in physical security and surveillance organizations. While organizations need continuous recording capabilities, traditional storage approaches often lead to increased costs. Kinesis Video Streams excels at real-time video management and short-term storage, serving enterprise deployments with extensive camera networks. Now, the innovative Kinesis Video Streams <a href="https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/tiered-storage.html" rel="noopener noreferrer" target="_blank">warm storage feature</a> enhances these capabilities, delivering cost-efficient solutions for long-term video retention, and enabling organizations to optimize their storage strategies.</p> 
<p>In this post, we show how by effectively using the newly launched Kinesis Video Streams warm storage feature, you can build a much more cost-efficient solution for long-term video retention while maintaining the performance and accessibility your operations require.</p> 
<h2>What did we launch?</h2> 
<p>On November 26th, 2025, Amazon Kinesis Video Streams added a new warm storage tier, delivering cost-effective storage for extended media retention. The standard Kinesis Video Streams storage tier (now designated as the hot tier) remains optimized for real-time data access and short-term storage. The new warm tier enables long-term media retention with sub-second access latency at reduced storage costs.</p> 
<h3>Customer benefits</h3> 
<p>While Amazon Kinesis Video Streams hot storage excels at real-time streaming, regulatory compliance often requires extended retention periods, making it costly for physical security and enterprise video surveillance customers. Customers that typically used Kinesis Video Streams hot tier for streaming but then moved data to other cost-efficient storage can now keep media in Kinesis Video Streams’ warm tier without incurring a cost penalty. This brings several advantages: (a) Customers can maintain simpler architectures without managing data migration, reduce their operational overhead, and keep their data readily accessible within the Kinesis Video Streams ecosystem. (b) Larger datasets remain in Kinesis Video Streams, customers benefit from more complete historical data and less fragmentation across storage systems. This enables enhanced analytics capabilities through easier processing of continuous data streams, better temporal analysis potential, and streamlined ML/AI model training. In addition, ingestion into Kinesis Video Streams warm storage tier is priced per persisted fragment count. That means developers control costs by adjusting fragment size – larger fragments reduce ingestion costs, while smaller fragments deliver lower latency. This flexibility lets developers optimize their specific use cases.</p> 
<h3>Architecture</h3> 
<p>Amazon Kinesis Video Streams tiered storage architecture provides a seamless integration between real-time streaming capabilities and cost-optimized long-term storage. The architecture consists of two components: Video ingestion, that includes IoT devices and cameras stream video data to KVS endpoints, and the Storage tier. The storage tier selection is based on the stream’s configuration, and fragments are stored in either hot or warm tier.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-1.png"><img alt="Image 1: Architecture Diagram - Data Center to Cloud Integration" class="alignnone wp-image-17415 size-full" height="656" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-1.png" width="1648" /></a></p> 
<p>Key benefits of this architecture are:</p> 
<ul> 
 <li><span style="text-decoration: underline;">Unified API</span>: Same Amazon Kinesis Video Streams APIs work across both storage tiers</li> 
 <li><span style="text-decoration: underline;">Seamless playback</span>: Applications can retrieve video regardless of storage tier</li> 
 <li><span style="text-decoration: underline;">Dynamic configuration</span>: Storage tiers can be changed without service interruption</li> 
 <li><span style="text-decoration: underline;">Automatic lifecycle:</span> Built-in policies manage data transitions and retention</li> 
</ul> 
<h3>Cost considerations</h3> 
<p>The table below shows Amazon Kinesis Video Streams’ hot and warm tier pricing structure for US East (N. Virginia) region. Note that warm tier storage prices have a significant reduction while consumption costs remain consistent across both tiers. Warm ingestion is priced&nbsp;based on fragment count rather than per GB, which provides predictable cost management for long-term retention scenarios. Warm tier storage comes with a 30-day minimum retention period, meaning AWS charges customers for at least 30 days even&nbsp;if they delete the data earlier.</p> 
<table border="1px" cellpadding="10px" class="styled-table"> 
 <caption>
  Table-1
 </caption> 
 <tbody> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Pricing dimension</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Kinesis Video Streams hot tier</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Kinesis Video Streams warm tier</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Data Ingested – per GB</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0085</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">n/a</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Data Ingested – per 1,000 persisted fragment count</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">n/a</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0100</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Data stored – per GB month</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0230</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0125</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Data Consumed – per GB</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0085</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0085</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Data Consumed HLS – per GB</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0119</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">0.0119</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Image extraction less 1080 – per million</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$10</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$10</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">Image extraction more 1080 – per million</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$18</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$18</td> 
  </tr> 
 </tbody> 
</table> 
<p>Kinesis Video Streams is built on <a href="https://aws.amazon.com/s3/" rel="noopener noreferrer" target="_blank">Amazon Simple Storage Service (Amazon S3)’s</a> data store, ensuring your video data is stored with high durability and availability. However, the frequency of accessing stored video data can vary significantly across different use cases. For example, consider a scenario where CCTV raw data must be retained for 30 days for security compliance purposes. If no special events occur, the frequency and segments of data that need to be reviewed will decrease over time. In such situations, warm tier functionality allows you to reduce storage costs while maintaining the same durability and availability of storage.</p> 
<p>Amazon Kinesis Video Streams’ warm tier is built on <a href="https://aws.amazon.com/s3/storage-classes/" rel="noopener noreferrer" target="_blank">Amazon S3 Standard-Infrequent Access (S3 Standard-IA)</a> to provide the following benefits:</p> 
<ul> 
 <li><span style="text-decoration: underline;">Up to 67% cost reduction:</span> Significant cost savings compared to standard Amazon Kinesis Video Streams storage</li> 
 <li><span style="text-decoration: underline;">Same durability and availability:</span> 99.999999999% durability and 99.9% availability guarantee</li> 
 <li><span style="text-decoration: underline;">Seamless integration:</span>&nbsp;Supports compatibility with existing Amazon Kinesis Video Streams workflows and APIs</li> 
 <li><span style="text-decoration: underline;">Automated lifecycle management: </span>Automated data management through configurable policies</li> 
</ul> 
<p>Now let’s look at three ways to optimize costs using the warm tier.</p> 
<h2><strong>1. Storage duration-based cost optimization</strong></h2> 
<p>Warm tier is designed for cost optimization in use cases requiring long-term storage. While the warm tier offers relatively lower storage costs, it requires a minimum retention period of 30 days.<br /> Therefore, for use cases requiring only short-term storage, hot tier may be more cost-effective.</p> 
<p>Real-world cost analysis example: Let’s examine 1,000 CCTV cameras operating 8 hours daily:</p> 
<ul> 
 <li><span style="text-decoration: underline;">Ingestion period:</span> 30 days</li> 
 <li><span style="text-decoration: underline;">Bitrate</span>: 4Mbps</li> 
 <li><span style="text-decoration: underline;">Fragment duration:</span> 2 seconds</li> 
 <li><span style="text-decoration: underline;">AWS Region</span>: us-east-1 region</li> 
</ul> 
<h3>Cost comparison based on retention period</h3> 
<table border="1px" cellpadding="10px" class="styled-table"> 
 <caption>
  Table-2
  <sup><a href="#footnote-1" id="footnote-ref-1" rel="noopener noreferrer" target="_blank">[1]</a></sup>
 </caption> 
 <tbody> 
  <tr> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Retention period (days)</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Storage cost ($) for period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Ingest cost ($) per period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Total Kinesis Video Streams cost ($) per period</strong></td> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Benefit Warm/Hot</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">7</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$2,233</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,818.74</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,419</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">-62%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">15</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,785</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$8,370.52</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,419</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">-13%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">30</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,419</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">28%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">60</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$19,138</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$10,401</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$22,724.25</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$14,620</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">36%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">90</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$28,707</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$15,602</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$32,293.41</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$19,821</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">39%</td> 
  </tr> 
 </tbody> 
</table> 
<p><strong>Key findings:</strong> As seen above, under the same fragment length, warm tier cost benefits are higher for longer retention periods, typically starting from 30 days.</p> 
<h2>2. Fragment length-based cost optimization</h2> 
<p>Another innovative feature of warm tier is fragment-based billing&nbsp;instead of the traditional GB-based billing of hot tier. This billing approach allows for significant reduction in ingestion costs by adjusting fragment length.</p> 
<p>The table below shows how benefits of warm tier increase compared to hot tier when the fragment length increases. This is for 30-day retention.</p> 
<table border="1px" cellpadding="10px" class="styled-table"> 
 <caption>
  Table-3
  <sup><a href="#footnote-2" id="footnote-ref-2" rel="noopener noreferrer" target="_blank">[2]</a></sup>
 </caption> 
 <tbody> 
  <tr style="border-color: #000000;"> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Fragment length (sec)</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Storage cost ($) for period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Ingest cost ($) per period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Total Kinesis Video Streams cost ($) per period</strong></td> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Benefit Warm/Hot</strong></td> 
  </tr> 
  <tr style="border-color: #000000;"> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">2</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,219</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,419</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">28%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">5</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,687.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$6,888.13</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">48%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">10</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$843.75</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$6,044.38</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">54%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">20</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,201</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,586</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$421.88</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,622.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">57%</td> 
  </tr> 
 </tbody> 
</table> 
<p><strong>Key findings:</strong> Under the same retention period, warm tier benefits are higher for longer fragment lengths. The chart below can assist as decision chart. For 30-day retention, warm tier breaks even at fragment length of 1.06 sec, i.e. total cost is cheaper in warm tier vs hot tier for fragment lengths longer than 1.06 sec. For 60-day retention, that break even happens at 0.68 sec.</p> 
<p><a href="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-2.png"><img alt="Image 2: Cost Comparison Chart - Fragment Length Analysis" class="alignnone wp-image-17414 size-full" height="796" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-2.png" width="1077" /></a></p> 
<h2>3.&nbsp;Understanding storage cost variations by camera resolution</h2> 
<p>The warm tier pricing based on fragment count model brings cost advantages for cameras with higher bitrates. See comparison in table below for 1,000 camera deployment over 30 days with 5-second fragments.</p> 
<table border="1px" cellpadding="10px" class="styled-table" style="border-color: #000000;"> 
 <caption>
  Table-4
 </caption> 
 <tbody> 
  <tr> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Bitrate</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Storage cost ($) for period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Ingest cost ($) per period</strong></td> 
   <td colspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Total Kinesis Video Streams cost ($) per period</strong></td> 
   <td rowspan="2" style="padding: 10px; border: 1px solid #dddddd;"><strong>Benefit Warm/Hot</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Hot</strong></td> 
   <td style="padding: 10px; border: 1px solid #dddddd;"><strong>Warm</strong></td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">1Mbps</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$2,392.29</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,300.16</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$896.48</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,687.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,288.77</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$2,987.66</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">9%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">2Mbps</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,784.58</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$2,600.31</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,792.97</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,687.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$6,577.55</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$4,287.81</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">35%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">4Mbps</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$9,569.16</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$5,200.63</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$3,585.94</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,687.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,155.09</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$6,888.13</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">48%</td> 
  </tr> 
  <tr> 
   <td style="padding: 10px; border: 1px solid #dddddd;">10Mbps</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$23,922.89</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$13,001.57</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$8,964.84</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$1,687.50</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$32,887.74</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">$14,689.07</td> 
   <td style="padding: 10px; border: 1px solid #dddddd;">55%</td> 
  </tr> 
 </tbody> 
</table> 
<p><strong>Key findings:</strong></p> 
<ul> 
 <li><span style="text-decoration: underline;">Higher bitrates yield greater savings:</span> 4 Mbps streams achieve 48% cost reduction, while 1Mbps streams see 9% savings</li> 
 <li><span style="text-decoration: underline;">Consistent ingestion cost advantage:</span> Warm tier ingestion remains at $1,687.50 regardless of bitrate, while hot tier costs scale linearly with data volume</li> 
 <li><span style="text-decoration: underline;">Storage cost scaling:</span> Warm tier storage costs scale proportionally with bitrate, but at significantly lower rates than hot tier</li> 
 <li><span style="text-decoration: underline;">Break-even analysis:</span> All bitrate scenarios demonstrated substantial cost benefits when using Warm tier for extended retention periods</li> 
</ul> 
<p>This fragment-based pricing model enables significantly more cost-effective storage and processing of high-resolution video content with larger data volumes. The higher the video quality and bitrate, the more pronounced the cost advantages become when utilizing warm tier storage.</p> 
<h2>Storage tier settings</h2> 
<p>The storage period settings for existing streams can be easily updated, and the settings are immediately applied to fragments collected after the update.Below is the AWS CLI command for updating or creating a stream in warm tier storage in Amazon Kinesis Video Streams.</p> 
<h3>Update storage configuration:</h3> 
<p>If you have an existing stream in hot tier, you can update the stream settings to warm tier as follows.</p> 
<div class="hide-language"> 
 <pre><code class="lang-php">```bash 
STREAM_INFO=$(aws kinesisvideo describe-stream \
--stream-name "$STREAM_NAME" \
--region $REGION)

CURRENT_VERSION=$(echo "$STREAM_INFO" | jq -r '.StreamInfo.Version')
aws kinesisvideo update-stream-storage-configuration \
--stream-name "$STREAM_NAME" \
--current-version "$CURRENT_VERSION" \
--stream-storage-configuration DefaultStorageTier="WARM" \
--region $REGION
```</code></pre> 
</div> 
<h3>Create stream with warm tier:</h3> 
<p>You can create a new warm tier stream using the following command.</p> 
<div class="hide-language"> 
 <pre><code class="lang-typescript">```bash
aws kinesisvideo create-stream \
--stream-name $STREAM_NAME \
--media-type "video/h264" \
--data-retention-in-hours 720 \
--stream-storage-configuration '{
"DefaultStorageTier": "WARM"
}' \
--region&nbsp;$REGION
```</code></pre> 
</div> 
<p>The storage period settings for created streams can be easily modified, and the settings are immediately applied to fragments collected after the change.&nbsp;Stream playback remains uninterrupted even when storage configuration changes are applied.</p> 
<h2>Cleaning up</h2> 
<p>To avoid ongoing charges, make sure to delete the test streams you created during this walkthrough. Remember that deleting a stream permanently removes all stored video data, regardless of the storage tier configuration.</p> 
<h3>Delete the stream:</h3> 
<div class="hide-language"> 
 <pre><code class="lang-code">```bash
STREAM_INFO=$(aws kinesisvideo describe-stream \
--stream-name "$STREAM_NAME" \
--region $REGION)

STREAM_ARN=$(echo "$STREAM_INFO" | jq -r '.StreamInfo.StreamARN')
CURRENT_VERSION=$(echo "$STREAM_INFO" | jq -r '.StreamInfo.Version')

aws kinesisvideo delete-stream \
--stream-arn "$STREAM_ARN" \
--current-version "$CURRENT_VERSION" \
--region $REGION
```</code></pre> 
</div> 
<h3><strong>Important notes:</strong></h3> 
<ul> 
 <li>Stream deletion is irreversible and removes all associated video data</li> 
 <li>Both Hot and Warm tier data will be permanently deleted</li> 
 <li>Ensure you have backed up any important video data before deletion</li> 
 <li>The deletion process may take a few minutes to complete</li> 
</ul> 
<h2>Conclusion</h2> 
<p>Amazon Kinesis Video Streams’ tiered storage capability delivers a cost-effective approach for managing video data. Through this feature organizations can dramatically reduce storage costs while maintaining operational excellence. These costs can be managed by controlling three variables: fragment length, retention period, and resolution bitrate. (a) Under the same fragment length, warm tier cost benefits are higher for longer retention periods, typically starting from 30 days; (b) under the same retention period, warm tier benefits are higher for longer fragment lengths; (c) under the same fragment length and retention period, warm tier cost benefits are higher as the bitrates increase.</p> 
<p>The key to successful implementation lies in accurately understanding your organization’s specific access patterns, retention requirements, and cost optimization goals. Start with pilot implementation on non-critical streams, monitor results, and gradually expand to your entire video infrastructure. Kinesis Video Streams tiered storage is not merely a cost reduction tool. It’s a strategic enabler that makes sustainable growth of video-enabled IoT applications economically viable.</p> 
<h2>Next steps</h2> 
<p>Are you ready to optimize costs for your video solution with Kinesis Video Streams tiered storage? Here’s your path forward:</p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/what-is-kinesis-video.html" rel="noopener noreferrer" target="_blank">Amazon Kinesis Video Streams documentation</a></li> 
 <li><a href="https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/tiered-storage.html" rel="noopener noreferrer" target="_blank">Amazon Kinesis Video Streams Tiered Storage</a></li> 
 <li><a href="https://aws.amazon.com/kinesis/video-streams/pricing/" rel="noopener noreferrer" target="_blank">Amazon Kinesis Video Streams Pricing</a></li> 
</ul> 
<hr /> 
<ol> 
 <li id="footnote-1">Cost per GB/Month is calculated as:<br /> Regional Storage Cost per GB * Total Capacity (GB)/day * Number of Cameras * Storage Period (days). For Warm tier, if the storage period is less than 30 days, the cost is frozen at the 30-day rate. <a href="#footnote-ref-1" rel="noopener noreferrer" target="_blank">↑</a></li> 
 <li id="footnote-2"><em>Monthly ingestion cost is calculated as follows:</em><em>Hot tier Monthly Ingestion Cost: Monthly Ingestion Cost (GB) * Bitrate ÷ 8 (convert to bytes) × Daily Collection Period (Hours) × 3600 (convert to seconds) ÷ 1024 (convert to GB) × Number of Cameras × Storage Period (days)</em><em>Warm tier Monthly Ingestion Cost: Monthly Ingestion Cost (GB) × Daily Collection Period (Hours) × 3600 (convert to seconds) ÷ Fragment Length (sec) ÷ 1000 (billing unit) × Number of Cameras × Storage Period (days)</em> <a href="#footnote-ref-2" rel="noopener noreferrer" target="_blank">↑</a></li> 
</ol> 
<hr /> 
<h3>About the authors</h3> 
<p style="clear: both;"><strong><img alt="Profile photo for Andre Sacaguti" class="alignleft wp-image-17422" height="100" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-5-1.jpeg" width="100" />Andre Sacaguti</strong> <a href="https://www.linkedin.com/in/andresacaguti/" rel="noopener noreferrer" target="_blank">Andre Sacaguti</a> is a Sr. Product Manager at AWS, working on Kinesis Video Streams. He helps organizations turn video data into actionable insights, exploring how AI agents can make streaming video smarter and more interactive. Before joining AWS, Andre built and launched IoT products at T-Mobile and Qualcomm, helping connected devices work smarter and more securely.</p> 
<p style="clear: both;"><strong><img alt="Profile photo for Jinseon Lee" class="alignleft wp-image-17423" height="100" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2026/01/02/IOTB-910-image-6-1.jpeg" width="100" />Jinseon Lee</strong> <a href="https://www.linkedin.com/in/jinseon-lee-160a2a13b/" rel="noopener noreferrer" target="_blank">Jinseon Lee</a> is a Senior IoT GTM Solutions Architect specializing in IoT and Robotics at AWS APJ. With over 12 years of experience in technology and software development, Jinseon has worked in diverse roles helping clients design and implement optimal cloud architectures.</p>
