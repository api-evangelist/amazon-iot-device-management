---
title: "AWS IoT Services Alignment with the European Union Cyber Resilience Act (EU CRA)"
url: "https://aws.amazon.com/blogs/iot/eu-cra/"
date: "Wed, 24 Dec 2025 09:29:27 +0000"
author: "Syed Rehan"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<h2>Introduction</h2> 
<p>In today’s digital world, Internet of Things (IoT) security and compliance continues to evolve. The&nbsp;<a href="https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act">European Union’s Cyber Resilience Act (CRA)</a>&nbsp;is reshaping how IoT manufacturers, developers, and service providers approach their work. Let’s explore what this means for AWS IoT customers and manufacturers using connected devices.</p> 
<h3>Understanding the CRA’s impact</h3> 
<p>The CRA was enacted on December 10, 2024, and its requirements begin to go into effect in September 2026 (for vulnerability reporting obligations) and December 2027 (full compliance). The CRA requires comprehensive cybersecurity for products with digital elements. This act aims to address the growing risks associated with the digitalization of hardware and software and the rising number of cyberattacks targeting connected devices.</p> 
<p>Historically, many consumers and industrial IoT products were developed without adequate security controls. Now, through its security-by-design and security-by-default requirements, the CRA helps to ensure a higher level of trust, resilience, and accountability throughout the product lifecycle.</p> 
<h2>What is the CRA?</h2> 
<p>Regulation (EU) 2024/2847, also titled the Cyber Resilience Act, is a regulation of the European Union that introduces EU-wide cybersecurity requirements for “products with digital elements,” hardware or software “intended for connection to a device or network” and made available within the EU. The CRA includes “essential cybersecurity requirements” for the design and development of products with digital elements and for a manufacturer’s processes. It also includes required vulnerability reporting obligations when a product with digital elements is experiencing a “severe incident” or “actively exploited vulnerability.”</p> 
<p>In addition to a broad category of product with digital elements, the CRA also describes additional requirements for “important” products with digital elements, and “critical” products with digital elements. Manufacturers should look to the CRA to determine what steps are needed to comply with the CRA based on the type of product with digital elements they offer in the EU.</p> 
<h3>Planning for CRA Compliance for IoT Manufacturers</h3> 
<p>AWS provides a comprehensive suite of services that can help IoT manufacturers implement measures needed to address the CRA’s essential cybersecurity requirements across all product categories.</p> 
<h3>Planning for compliance</h3> 
<p>AWS IoT services offer solutions to help meet the CRA requirements across different product classifications while manufacturers prepare for the CRA’s implementation timeline.</p> 
<h3>Security requirements:</h3> 
<ul> 
 <li>Use AWS IoT Core with X.509 certificates for authentication and access control.</li> 
 <li>Implement TLS 1.2 encryption for data in transit with AWS IoT Core.</li> 
 <li>Enable AWS IoT policies for access control and data protection.</li> 
 <li>Use AWS IoT Device Defender for monitoring and security assessment.</li> 
 <li>Implement AWS IoT Device Management for secure updates.</li> 
</ul> 
<h3>Vulnerability handling requirements:</h3> 
<ul> 
 <li>Use AWS Security Hub and Amazon Detective for vulnerability detection.</li> 
 <li>Implement Amazon EventBridge for incident workflow automation.</li> 
 <li>Use AWS IoT Device Defender for continuous security monitoring.</li> 
 <li>Store vulnerability and incident data in Amazon Security Lake for documentation.</li> 
</ul> 
<h2>Implementation example: Smart Thermostat (Class I important product)</h2> 
<p>Securely implementing a smart thermostat as a Class I product under the EU CRA begins with its design and development. AWS customers can use AWS IoT Core’s just-in-time Registration (JITR) for secure provisioning, while using <a href="https://aws.amazon.com/certificate-manager/">AWS Certificate Manager</a> to handle certificate management or AWS IoT Core directly when using certificates managed by AWS IoT. Access control can be enforced through <a href="https://docs.aws.amazon.com/iot/latest/developerguide/iot-policies.html">AWS IoT policies</a> to ensure proper authorization.</p> 
<p>Data protection is implemented through multiple security layers. AWS IoT Core enforces TLS 1.2 encryption for secure data transmission while strict topic access controls govern data access. In addition, AWS IoT Device Defender provides continuous security monitoring to detect and prevent potential threats.</p> 
<p>Customers can use <a href="https://aws.amazon.com/iot-device-management/">AWS IoT Device Management</a> to manage the device lifecycle through the required 5-year minimum support period. This includes maintaining device security through secure over-the-air (OTA) updates with signed firmware and tracking software states to maintain version control.</p> 
<p><a href="https://aws.amazon.com/iot-device-defender/">AWS IoT Device Defender</a> can help customers perform continuous security metric monitoring while <a href="https://aws.amazon.com/eventbridge/">Amazon EventBridge</a> can enable customers to implement automated incident detection. <a href="https://aws.amazon.com/cloudwatch/">AWS CloudWatch</a> and A<a href="https://aws.amazon.com/sns/">mazon Simple Notification Service</a> (Amazon SNS) can enable customers to set up security alerts. Customers can use <a href="https://aws.amazon.com/lambda/">AWS Lambda</a> to implement automated remediation actions, which could include certificate revocation or device quarantine when security issues are detected.</p> 
<p>Amazon EventBridge can help customers create a structured report to incident reporting with notification workflows. Customers can also use Amazon Security Lake for comprehensive record-keeping and secure documentation storage.</p> 
<h2>Looking ahead: The impact of CRA on IoT security</h2> 
<p>AWS IoT customers must review the CRA to determine their compliance obligations under the Act. The CRA also creates a strategic opportunity to enhance security practices and build stronger trust with end-users through certified compliance measures.</p> 
<p>The regulation excludes specific domains that already have comprehensive regulatory frameworks. For example, medical devices fall under the Medical Devices Regulation (MDR), while automotive systems follow&nbsp;<a href="https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A02019R2144-20240707">(EU) 2019/2144&nbsp;</a>standards. The CRA covers products with digital elements at a broader level. This broad scope demonstrates how the regulation will shape the future of IoT security and product development.</p> 
<p>Organizations leveraging AWS IoT solutions should view CRA compliance as an investment in product quality and market competitiveness. CRA standards will help establish more secure and reliable IoT products, which will benefit both manufacturers and consumers while raising the bar for IoT security across the industry.</p> 
<h2>Conclusion</h2> 
<p>As manufacturers face new cybersecurity challenges under the CRA, AWS IoT services can help deliver the security foundation they need. These services combine built-in security features, automated monitoring, and comprehensive documentation to help manufacturers meet CRA requirements with confidence. By implementing AWS IoT’s security-first approach, manufacturers can transform regulatory compliance from a challenge into a competitive advantage.</p> 
<p>As you prepare for the 2027 implementation deadline, early adoption of these AWS IoT security features can help establish the necessary infrastructure for compliance with the CRA’s essential requirements, vulnerability handling processes, and incident reporting obligations. This proactive approach not only supports regulatory compliance but also enhances overall product security and customer trust in the increasingly connected digital marketplace.</p> 
<p>Important&nbsp;reminder: While AWS services can help implement technical controls, you as the customer are solely responsible for ensuring full compliance with all EU CRA requirements including proper product classification, conformity assessment procedures, and ongoing maintenance of required documentation. Importantly, even if your products don’t fall within specific categories, you may still need to comply with the EU CRA regulation, and you must carefully review the law to understand how it applies to your specific use cases.</p> 
<h2>Related links</h2> 
<p>To learn more about the technologies or features used in this blog, explore the following pages:</p> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/iot/latest/developerguide/security-best-practices.html">AWS IoT Security Best Practices</a></li> 
 <li><a href="https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act">European Union Cyber Resilience Act Overview</a></li> 
 <li><a href="https://catalog.us-east-1.prod.workshops.aws/workshops/1d50f8ef-5030-42c9-9488-a2187472c9e6/en-US">AWS IoT Zero Trust workshop</a></li> 
 <li><a href="https://docs.aws.amazon.com/wellarchitected/latest/iot-lens/iot-lens.html">Internet of Things (IoT) Lens</a></li> 
 <li><a href="https://docs.aws.amazon.com/greengrass/">AWS IoT Greengrass for Edge Compliance</a></li> 
 <li><a href="https://aws.amazon.com/compliance/programs/">AWS Compliance programs</a></li> 
 <li><a href="https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html">AWS Security Hub</a></li> 
 <li><a href="https://aws.amazon.com/audit-manager/">AWS Audit Manager </a></li> 
 <li><a href="https://aws.amazon.com/architecture/security-identity-compliance/">AWS Security Best Practices</a></li> 
 <li><a href="https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Checklist.pdf">AWS Security Checklist</a></li> 
 <li><a href="https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html">AWS Well-Architected Framework – Security Pillar</a></li> 
 <li><a href="https://docs.aws.amazon.com/whitepapers/latest/overview-aws-cloud-adoption-framework/security-perspective.html">AWS Cloud Adoption Framework – Security Perspective</a></li> 
 <li><a href="https://docs.aws.amazon.com/whitepapers/latest/navigating-gdpr-compliance/encrypt-data-at-rest.html">Encrypting Data at Rest</a></li> 
 <li><a href="https://aws.amazon.com/blogs/iot/setting-up-just-in-time-provisioning-with-aws-iot-core/">Setup Just-in-Time Provisioning with AWS IoT Core</a></li> 
 <li><a href="https://catalog.us-east-1.prod.workshops.aws/workshops/6d30487a-48e1-4631-b6bc-5602582800b5/en-US/">Get started with AWS IoT workshop</a></li> 
 <li><a href="https://catalog.workshops.aws/greengrass">AWS IoT Greengrass workshop</a></li> 
 <li><a href="https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R2847">Regulation of European Parliament on horizontal cybersecurity requirements for products with digital elements</a></li> 
 <li><a href="https://aws.amazon.com/blogs/iot/aws-iot-and-us-cyber-trust-mark/">US Cyber Trust Mark for US market</a></li> 
 <li><a href="https://aws.amazon.com/blogs/iot/securing-the-future-of-mobility-unece-wp-29-and-aws-iot-for-connected-vehicle-cybersecurity/">UNECE WP.29 and AWS IoT for connected vehicle cybersecurity</a></li> 
</ul> 
<h2>About the author</h2> 
<p>
 <!-- First Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="NAME OF YOUR IMAGE FROM MEDIA LIBRARY"><img alt="syed" class="wp-image-16165 size-full alignleft" height="121" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2024/10/01/Syed125px.jpg" width="125" /></p> 
 <h3 class="lb-h4"><a href="https://www.linkedin.com/in/iamsyed/">Syed Rehan</a></h3> 
 <p style="color: #000000;">Syed is a Senior AI Solutions Cybersecurity Product Architect at Amazon Web Services (AWS), operating within the AWS AI Solutions organization. As a published <a href="https://link.springer.com/search?new-search=true&amp;query=&amp;content-type=Book&amp;dateFrom=&amp;dateTo=&amp;contributor=%22Syed+Rehan%22&amp;sortBy=newestFirst">books author</a> on Cybersecurity, Machine Learning and IoT he brings extensive expertise to his global role. Syed serves a diverse customer base, collaborating with security specialists, CISOs, developers, and security decision-makers to promote the adoption of AWS Security services and solutions. With in-depth knowledge of cybersecurity, machine learning, artificial intelligence, IoT, and cloud technologies, Syed assists customers ranging from startups to large enterprises. He enables them to construct secure IoT, ML, and AI-based solutions within the AWS environment</p> 
</div>
