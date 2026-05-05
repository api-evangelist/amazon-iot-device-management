---
title: "Securing Vehicle Identification Number (VIN) with Reference ID in connected vehicle platforms with AWS IoT"
url: "https://aws.amazon.com/blogs/iot/securing-vehicle-identification-number-with-reference-id-in-connected-vehicle-platforms-with-aws-iot/"
date: "Wed, 10 Sep 2025 09:16:30 +0000"
author: "Paritosh Mehta"
feed_url: "https://aws.amazon.com/blogs/iot/feed/"
---
<p>With over 470 million connected cars expected by end of 2025, protecting sensitive vehicle data, particularly Vehicle Identification Numbers (VINs), has become crucial for automakers. VINs serve as unique identifiers in automotive processes from manufacturing to maintenance, making them <a href="https://www.tripwire.com/state-of-security/vin-cybersecurity-exploits-and-how-address-them">attractive targets for cybercriminals</a>. This post explores how automakers can help securing VINs in connected vehicle platforms using AWS IoT helping ensure both data protection and system functionality.</p> 
<p>This solution introduces Reference IDs as pseudonyms for VINs, helping enable secure vehicle data interactions without exposing actual VINs. Using AWS IoT services, we’ll demonstrate how this architecture helps automakers protect sensitive data while maintaining full functionality across automotive use cases.</p> 
<h2>Introduction</h2> 
<p>The solution uses a Reference ID system where each vehicle receives a unique identifier during provisioning, acting as a VIN proxy in all platform interactions. A vehicle registry database stores both hashed and encrypted versions of VINs, mapped to their Reference IDs. When clients present a VIN, the system hashes it to retrieve the corresponding Reference ID, enabling secure integration with existing processes.</p> 
<p>The encrypted VIN is added as a fail-safe measure, encrypted during provisioning using a secure AWS Key Management Service (AWS KMS). In cases where the plain text value of the VIN needs to be retrieved, it can be done by decrypting this value, ensuring that the actual VIN is accessible when absolutely necessary while maintaining strong security measures.</p> 
<p><img alt="" class="aligncenter wp-image-17207 size-full" height="1178" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/09/10/PII_BLOG-Page-1.drawio-1.png" width="2192" /></p> 
<p>VINs contain critical vehicle information (manufacturer, model, year) and can be linked to personal data. Unprotected VINs in cloud environments risk identity theft, vehicle theft, insurance fraud, privacy violations, and regulatory non-compliance (GDPR, CCPA).</p> 
<p>By implementing a Reference ID system for VIN protection in cloud-based connected vehicle platforms, automakers can help enhance data security while maintaining the functionality and efficiency required for modern automotive operations:</p> 
<ul> 
 <li>They act as proxies for VINs, enhancing security and data minimization</li> 
 <li>Support compliance with data protection regulations</li> 
 <li>Provide flexible access control and improved audit-ability</li> 
 <li>Offer scalability for large vehicle fleets and easier system interoperability</li> 
 <li>Allow for revocation without changing the underlying VIN</li> 
 <li>Enable detailed auditing and logging of VIN access and transformations, providing visibility into who/what has authorization to convert between Reference IDs and VINs</li> 
</ul> 
<h2>Architecture walkthrough</h2> 
<h3>1. Reference ID</h3> 
<p>A Reference ID is a UUID generated during vehicle provisioning that serves as a VIN proxy throughout the vehicle’s lifecycle, creating an abstraction layer that protects sensitive VIN data.</p> 
<h3>2. Vehicle registry database</h3> 
<p>The vehicle registry database serves as a centralized repository for vehicle information throughout its platform lifetime. Key features include:</p> 
<ul> 
 <li>Reference ID to hashed VIN mapping</li> 
 <li>Encrypted VIN storage</li> 
 <li>Vehicle provisioning and state change tracking</li> 
 <li>Device change history</li> 
 <li>Vehicle attributes and configurations</li> 
</ul> 
<p>VIN hashing enables secure verification without exposing actual values. This centralized approach provides a single source of truth while enabling secure remote diagnostics and over-the-air updates.</p> 
<table border="1" style="height: 281px;" width="279"> 
 <tbody> 
  <tr> 
   <td><strong>Vehicle Registry DB</strong></td> 
  </tr> 
  <tr> 
   <td>referenceId <em>– <span style="color: #ff0000;">Partition key</span></em></td> 
  </tr> 
  <tr> 
   <td>deviceId <em>– <span style="color: #ff0000;">Global secondary index</span></em></td> 
  </tr> 
  <tr> 
   <td>hashedVin <em>– <span style="color: #ff0000;">Global secondary index</span></em></td> 
  </tr> 
  <tr> 
   <td>tenantId</td> 
  </tr> 
  <tr> 
   <td>encryptedVin</td> 
  </tr> 
 </tbody> 
</table> 
<p><strong>Note:</strong> deviceId and hashedVin being Global Secondary Indexes enables querying vehicle details by either field.</p> 
<h3>3. Vehicle provisioning</h3> 
<p>Vehicle provisioning establishes secure vehicle management and implements the reference ID system through data validation, secure storage, and AWS IoT integration.</p> 
<p>Let’s walk through the key steps of this process to understand how it safeguards vehicle information while enabling seamless connectivity and management:</p> 
<h4>3.1 Data validation:</h4> 
<ol> 
 <li>The provisioning infrastructure hashes the VIN and queries the vehicle registry DB to check if it’s a first-time provisioning.</li> 
 <li>For new vehicles, DEVICE ID can be validated against existing data made available by the TCU Manufacturer.</li> 
 <li>It also checks if the DEVICE is already attached to another vehicle by querying the vehicle registry DB with DEVICE ID.</li> 
</ol> 
<h4>3.2 Reference ID generation:</h4> 
<ol> 
 <li>A query is performed against the vehicle registry DB to validate if vehicle is already provisioned using hashed VIN.</li> 
 <li>If vehicle is not provisioned already, a new UUID is generated as the Reference ID.</li> 
 <li>The Reference ID, hashed VIN and encrypted VIN (via KMS) are stored in the vehicle registry DB along with other vehicle information. In the rare event of a UUID collision, the request can be re-tried to generate a new UUID as Reference ID.</li> 
 <li>A final query is performed by Reference ID in the vehicle registry DB to ensure uniqueness. If UUID collision is detected, a new UUID is generated.</li> 
 <li>For previously provisioned vehicles, the incoming payload is simply validated against the registry DB entry.</li> 
</ol> 
<h4>3.3 Certificate generation:</h4> 
<ul> 
 <li>Certificates are generated using ACM PCA with Common Name = Reference ID.</li> 
</ul> 
<h4>3.4 AWS IoT integration:</h4> 
<ol> 
 <li>An <a href="https://aws.amazon.com/iot-core/">AWS IoT</a> Thing is created with Thing name = Reference ID.</li> 
 <li>An <a href="https://aws.amazon.com/iot-fleetwise/">AWS IoT FleetWise</a> Vehicle is created with Vehicle Name = Reference ID.</li> 
</ol> 
<h4>3.5 Response payload:</h4> 
<ol> 
 <li>After successful provisioning the vehicle is provided with Certificates and Reference ID.</li> 
 <li>The vehicle can connect to AWS IoT FleetWise using the returned certificates and ClientId = ReferenceID.</li> 
</ol> 
<p>This process helps ensure secure provisioning of vehicles while protecting sensitive VIN information using Reference IDs, leveraging AWS services for robust identity and access management. The vehicle can provide a Certificate Signing Request (CSR), which the provisioning infrastructure uses to generate the certificate.</p> 
<h3>4. Data collection and storage</h3> 
<p>Data collection and storage is an essential component where Reference IDs ensure secure handling of vehicle data throughout its lifecycle – from transmission to storage and retrieval. This system helps protect VIN information while enabling efficient data operations.</p> 
<h4>4.1 Vehicle to AWS IoT FleetWise:</h4> 
<ol> 
 <li>Vehicle connects to AWS IoT FleetWise using the Reference ID as the client ID.</li> 
 <li>All data sent from the vehicle is associated with the Reference ID, as the vehicle name in AWS IoT FleetWise = Reference ID.</li> 
</ol> 
<h4>4.2 AWS IoT FleetWise to data platform:</h4> 
<ul> 
 <li>Data flowing from AWS IoT FleetWise is enriched with the vehicle name (Reference ID).</li> 
</ul> 
<h4>4.3 Data storage and retrieval:</h4> 
<ol> 
 <li>Data in the data platform is stored using the Reference ID as the identifier.</li> 
 <li>Mobile app queries the data platform via the API Platform using the Reference ID to retrieve vehicle data.</li> 
</ol> 
<p>The pseudonymous Reference ID contains no vehicle-specific information and serves as the primary identifier across <a href="https://aws.amazon.com/iot-core/">AWS IoT Core</a>, <a href="https://aws.amazon.com/iot-fleetwise/">AWS IoT FleetWise</a>, and associated data stores. This information-neutral approach helps ensure VIN protection while enabling seamless data operations across the platform.</p> 
<h3>5. Client application interactions:</h3> 
<p>Client applications, such as Customer Relationship Management (CRM) systems or platforms managing user-to-VIN mappings, typically deal with plain text VIN numbers. To maintain the security benefits of this system while accommodating these applications, a streamlined process for client interactions is implemented with the connected vehicles platform.</p> 
<h4>5.1 VIN to Reference ID conversion:</h4> 
<ol> 
 <li>The client application, after verifying vehicle ownership, makes an API call to the platform to convert between hashed VIN and Reference ID.</li> 
 <li>The API queries the vehicle registry DB to retrieve the corresponding Reference ID.</li> 
 <li>The Reference ID is then returned to the client application.</li> 
</ol> 
<p>Security considerations:</p> 
<ul> 
 <li>Access to this conversion API must be strictly controlled through robust authentication and authorization.</li> 
 <li>All conversion requests should be logged for audit purposes and monitored for suspicious patterns.</li> 
 <li>Implementation should include rate limiting and other security measures to protect against DoS/DDoS attacks and unauthorized bulk conversion attempts.</li> 
 <li>Since this API enables re-identification of vehicle data, access should be limited to authorized applications with legitimate business needs.</li> 
</ul> 
<h4>5.2 Once the client application has obtained the Reference ID corresponding to the VIN, it can:</h4> 
<ol> 
 <li>Retrieve data from the data platform using the Reference ID.</li> 
 <li>Perform operations directly on the vehicle by passing the Reference ID such as remote commands.</li> 
</ol> 
<p>This approach helps enhance platform security by eliminating VIN usage in API calls and maintaining separation between VINs and Reference IDs. The system helps enable secure client application interactions while providing a robust framework for cloud-based vehicle management.</p> 
<h3>6. Telematics control unit change:</h3> 
<p>The TCU (Telematics Control Unit) change flow is a critical process in the connected vehicle platform, addressing scenarios where a vehicle’s TCU needs to be updated or replaced. This can occur either before the vehicle leaves the manufacturing facility or after a user has taken ownership and an issue with the TCU is discovered, requiring replacement at a service center.</p> 
<p>The TCU Change flow can be made available as an API call with one of 2 functions:</p> 
<ol> 
 <li>Update the DEVICE ID in the vehicle registry DB to a new DEVICE ID.</li> 
 <li>Simply delete the DEVICE ID in the vehicle registry DB entry of the vehicle i.e. mark it as NULL.</li> 
</ol> 
<h4>6.1 TCU update:</h4> 
<ol> 
 <li>Inputs: hashed VIN (or Reference ID), existing DEVICE ID, new DEVICE ID.</li> 
 <li>The API: 
  <ul> 
   <li>Verifies hashed VIN exists and matches existing DEVICE ID in registry database</li> 
   <li>Checks new DEVICE ID is not associated with another vehicle.</li> 
   <li>Updates DEVICE ID in registry database.</li> 
   <li>Revokes and deletes the vehicle’s existing certificates (issued during provisioning and registered in AWS IoT Core) since the private keys are stored within the TCU hardware itself, requiring new certificates for the replacement TCU.</li> 
  </ul> </li> 
 <li>New TCU goes through provisioning process to connect to cloud.</li> 
</ol> 
<h4>6.2 TCU delete:</h4> 
<ol> 
 <li>Inputs: hashed VIN (or Reference ID), existing DEVICE ID.</li> 
 <li>The API: 
  <ol> 
   <li>Verifies hashed VIN exists and matches DEVICE ID in registry database.</li> 
   <li>Removes DEVICE ID from registry database entry.</li> 
   <li>Revokes and deletes the vehicle’s existing certificates (issued during provisioning and registered in AWS IoT Core)</li> 
  </ol> </li> 
</ol> 
<p><strong>Note:</strong> Either hashed VIN or Reference ID can be used to identify the vehicle. Using hashed VIN is acceptable due to SHA256’s extremely low collision probability.</p> 
<p>Both flows help ensure a secure and trackable TCU change process, with the registry database maintaining a history of TCU changes for each vehicle. This approach maintains the integrity of the system while accommodating necessary hardware updates in the vehicle fleet</p> 
<h2>Security, performance, and scalability considerations</h2> 
<p>The Reference ID system enhances VIN protection by minimizing VIN exposure in daily operations. The vehicle registry DB stores only hashed and encrypted VINs, while Reference IDs handle all platform interactions. Security is further enhanced through AWS KMS encryption and strict access control policies. For optimal performance and scalability, the system uses efficient UUID generation and global secondary indexes from DynamoDB for rapid queries.</p> 
<p>Looking to the future, this VIN management system has the potential to integrate with emerging technologies such as blockchain or distributed registry technology for tamper-proof VIN records, further enhancing security and traceability. The wealth of data automakers can collect through this system also opens possibilities for advanced analytics and machine learning applications, potentially offering insights into vehicle performance, maintenance needs, and user behavior patterns.</p> 
<p>To assist with ongoing compliance with evolving data protection regulations like GDPR and CCPA, it is recommended to employ the latest hashing and encryption algorithms, implement granular access controls, and regularly audit your data handling practices.</p> 
<p>This comprehensive approach not only helps safeguard VIN data but also positions the platform for future innovations in connected vehicle management.</p> 
<h2>Conclusion</h2> 
<p>This post demonstrated how Reference IDs can help automakers enhance VIN security in connected vehicle platforms on AWS. This architecture helps protect sensitive vehicle data while maintaining full functionality across automotive use cases. By leveraging AWS services like AWS IoT Core and Amazon DynamoDB, this solution scales efficiently for large vehicle fleets.</p> 
<p>As the number of connected vehicles grows, robust security measures become crucial for automakers. This Reference ID system not only helps automakers safeguard VINs but also helps them meet compliance standards for data protection regulations. It provides a flexible framework for managing vehicle identity throughout its lifecycle, including scenarios like TCU changes.</p> 
<p>You’re encouraged to explore how this approach can be adapted to your connected vehicle solutions.&nbsp;For more information on AWS IoT services and connected vehicle best practices, visit the <a href="https://docs.aws.amazon.com/iot-fleetwise/latest/developerguide/what-is-iotfleetwise.html">AWS IoT FleetWise documentation</a> and <a href="https://aws.amazon.com/blogs/industries/tag/connected-vehicle/">related blog posts</a></p> 
<h2></h2> 
<h2>About the authors</h2> 
<p>
 <!-- First Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="mparitos_zoom_125x125.jpg"><img alt="Paritosh Mehta" class="wp-image-1288 size-thumbnail alignleft" height="125" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/06/02/mparitos_zoom_125x125.jpg" width="125" /></p> 
 <h3 class="lb-h4">Paritosh Mehta</h3> 
 <p style="color: #879196; font-size: 1rem;"><span style="color: #879196;">Paritosh Mehta is a Delivery Consultant at AWS Professional Services, leading connected vehicle and industrial IoT implementations across Asia Pacific. As ProServe India’s IoT technical lead, he architects transformative solutions for automotive OEMs and manufacturers, specializing in vehicle telematics, real-time data platforms, and manufacturing systems integration.</span></p> 
</div> 
<p>
 <!-- Second Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="ankur_square_125x125.jpg"><img alt="" class="alignleft wp-image-16813" height="104" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/06/03/ankur_square_125x125.jpg" width="125" /></p> 
 <h3 class="lb-h4">Ankur Pannase</h3> 
 <p style="color: #879196; font-size: 1rem;"><span style="color: #879196;">Ankur is a Security Architect in Professional Services at AWS. He works closely with customers to design and implement cloud security solutions tailored to their technical, regulatory, and business needs. Ankur specializes in helping organizations build secure, scalable, and compliant environments in the cloud.</span></p> 
</div> 
<p>
 <!-- Third Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="jongchun-blog.jpeg"><img alt="" class="alignleft wp-image-16813" height="125" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/06/02/jongchun-blog.jpeg" width="125" /></p> 
 <h3 class="lb-h4">Jay Chung</h3> 
 <p style="color: #879196; font-size: 1rem;"><span style="color: #879196;">Jay is as a Senior Delivery Consultant at AWS Professional Services, where he helps customers architect and implement transformative cloud solutions. Jay is an Automotive enthusiast with over a decade of experience as product manager and software engineer in the Automotive testing tool industry.</span></p> 
</div> 
<p>
 <!-- Fourth Author --></p> 
<div class="blog-author-box" style="border: 1px solid #d5dbdb; padding: 15px;"> 
 <p class="robin_square_125x125.png"><img alt="" class="alignleft wp-image-16813" height="104" src="https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2025/06/03/robin_square_125x125.png" width="125" /></p> 
 <h3 class="lb-h4">Robin Francis</h3> 
 <p style="color: #879196; font-size: 1rem;"><span style="color: #879196;">Robin works at AWS as a Cloud Application Architect within the Professional Services Team, helping some of the biggest enterprises globally in building efficient, innovative solution on cloud. An autodidactic, polymath and polyglot, he constantly ventures into different domains of arts and science. Outside of work, he is into making music, learning foreign languages, cooking, surfing and traveling.</span></p> 
</div>
