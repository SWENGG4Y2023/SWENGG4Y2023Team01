**Design Document for Netflix**

**Index**

Introduction

1\. Purpose

2\.Scope

3\.Document Conventions

4\.System Overview

4\.1 System Context

4\.2 Key Components

5\.Architectural Design

5\.1 Architectural Overview

5\.2 Design Principles

6\.Detailed Design

6\.1 User Management

6\.2 Content Management

6\.3 Recommendation Engine

6\.4 Streaming Infrastructure

6\.5 Billing and Subscription

6\.6 Analytics and Insights

6\.7 Design Decisions and Trade-offs

7\.Deployment Architecture

7\.1 Overview

7\.2 Client Devices

7\.3 Content Delivery Networks (CDNs)

7\.4 Backend Services

8\. Data Design

8\.1 Database

8\.2Data Processing

8\.3Video Recommendation System

9\. User Interface Design

9\.1 User Interface Wireframes

9\.2 User Interaction Patterns

10\. Security Design

10\.1. Authentication and Authorization

Data protection

Secure Communication

11\. Error Handling

11\.1 Exception Handling

11\.2 Logging and Monitoring

12\. Conclusion

**1. Introduction**


1. **Purpose**

The purpose of the Netflix design document is to provide a comprehensive overview of the design principles and architecture for the Netflix software system. It serves as a guide for the development team in creating a robust, scalable, and maintainable system. The document outlines the high-level design of the system, key components, and design principles to ensure clarity and consistency during the development process.

1. **Scope**

The Netflix design document covers the following aspects of the software system:

- High-level architectural overview: Describing the overall structure and components of the system.
- Design principles: Identifying and applying relevant design principles from the Software Engineering Body of Knowledge (SWEBOK).
- Key modules and components: Providing an overview of the major functional components of the system.
- Deployment architecture: Explaining how the system is deployed, including client devices, CDNs, backend services, and databases.
- Data design: Describing the database schema, data flow, and interactions between modules and databases.
- User interface design: Presenting wireframes or mockups illustrating the user interface design.
- Security design: Outlining the mechanisms and protocols used for user authentication, data protection, and secure communication.
- Performance optimization: Detailing caching strategies, CDNs, and data indexing techniques used to enhance system performance.
- Error handling and logging: Describing the approach to handling exceptions, errors, and system logging.

**1.3 Document Conventions**

The Netflix design document follows the following conventions:

- UML notation: Using UML diagrams, such as class diagrams and deployment diagrams, to visualize the system design.
- Code snippets: Providing relevant code snippets for illustration purposes, especially in sections discussing specific modules or components.
- SWEBOK references: Referring to the Software Engineering Body of Knowledge (SWEBOK) for identifying and applying design principles.
- Visual aids: Including wireframes, mockups, and diagrams to enhance the understanding of the system design.
- Structured sections: Organizing the document into sections, such as introduction, architectural design, detailed design, and conclusion, for better readability and clarity.


**2. System Overview**

**2.1 System Context**

Netflix is an online video streaming platform that allows users to access a vast library of movies and TV shows on various devices. The system consists of a backend infrastructure, client applications, and content delivery networks (CDNs) to ensure reliable and efficient streaming.

The Netflix software system operates within a broader system context, which includes various external entities and interfaces. These elements interact with the Netflix system to fulfill its functionalities. The system context includes:

- User Devices: Various client devices, such as smartphones, tablets, smart TVs, and web browsers, through which users access the Netflix platform.
- Content Providers: Third-party content providers that license movies and TV shows to be included in the Netflix library.
- Payment Gateways: External payment service providers that handle payment processing for user subscriptions and transactions.
- Content Delivery Networks (CDNs): Infrastructure and services that assist in the efficient and reliable delivery of streaming content to users across different geographical locations.
- Social Media Platforms: Interfaces with social media platforms to enable users to share their viewing activities and recommendations.


**2.2 Key Components**

- User Management: Handles user authentication, registration, and profile management. It includes functionalities like user sign-up, login, password recovery, and user profile customization.
- Content Management: Manages the ingestion, categorization, and storage of movies and TV shows within the Netflix library. It encompasses content metadata management, content recommendation integration, and content search functionalities.
- Recommendation Engine: Generates personalized recommendations for users based on their viewing history, preferences, and behavior. It employs algorithms and machine learning techniques to provide accurate and relevant recommendations to individual users.
- Streaming Infrastructure: Provides the necessary infrastructure for streaming videos to different client devices. This component includes video encoding, adaptive bitrate streaming, content caching, and network optimization techniques to ensure smooth and high-quality video playback.
- Billing and Subscription: Handles payment processing, subscription management, and user billing. It integrates with external payment gateways to handle subscription upgrades, downgrades, and payment transactions securely.
- Analytics and Insights: Collects and analyzes user data, viewing patterns, and system performance metrics to derive valuable insights for business decisions, content recommendations, and system optimization.


**3. Architectural Design**

**3.1 Architectural Overview**

The Netflix system follows a microservices architecture, where different functional components are decoupled and independently deployable. This architecture allows for scalability, fault tolerance, and easy integration with third-party services. The system also leverages cloud computing resources for elasticity and cost efficiency.

Netflix works on two clouds **AWS** and **Open Connect**. These two clouds work together as the backbone of Netflix and both are highly responsible for providing the best video to the subscribers. 

The application has mainly 3 components.

- **Client:** Device (User Interface) which is used to browse and play Netflix videos. TV, XBOX, laptop or mobile phone, etc
- **OC (Open Connect) or Netflix CDN:** CDN is the network of distributed servers in different geographical locations, and Open Connect is Netflix’s own custom global CDN (Content delivery network). It handles everything which involves video streaming. It is distributed in different locations and once you hit the play button the video stream from this component is displayed on your device. So if you’re trying to play the video sitting in North America, the video will be served from the nearest open connect (or server) instead of the original server (faster response from the nearest server).
- **Backend (Database)**: This part handles everything that doesn’t involve video streaming (before you hit the play button) such as onboarding new content, processing videos, distributing them to servers located in different parts of the world, and managing the network traffic. Most of the processes are taken care of by Amazon Web Services.

Netflix frontend is written in **ReactJS** for mainly three reasons like start up speed, runtime performance, and modularity. Let’s discuss the components and working of Netflix.  

**Onboarding a Movie:**

` `Netflix performs high-quality video transcoding to support over 2200 different devices with varying resolutions and formats. Transcoding involves converting videos into different formats and resolutions. Multiple replicas of each movie are created, requiring extensive transcoding and pre-processing. The transcoded files are distributed to Open Connect servers worldwide for efficient streaming. AWS instances handle tasks like login, recommendations, and billing. During playback, Netflix dynamically adjusts streaming formats based on network conditions. User data is stored in AWS for personalized movie recommendations. Open Connect offers cost-effectiveness, improved quality, and scalability.

![Netflix-Transcoding](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.001.png)

**1. Elastic Load Balancer**

![Netflix-System-Design-Elastic-Load-Balancer](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.002.jpeg)

ELB in Netflix is responsible for routing the traffic to front-end services. ELB performs a two-tier load-balancing scheme where the load is balanced over zones first and then instances (servers).  

- The First-tier consists of basic DNS-based Round Robin Balancing. When the request lands on the first load balancing ( see the figure), it is balanced across one of the zones (using round-robin) that your ELB is configured to use.
- The second tier is an array of load balancer instances, and it performs the Round Robin Balancing technique to distribute the request across the instances that are behind it in the same zone.


**2. ZUUL**

ZUUL is a gateway service that provides dynamic routing, monitoring, resiliency, and security. It provides easy routing based on query parameters, URL, and path. Let’s understand the working of its different parts…  

- **The Netty server** takes responsibility to handle the network protocol, web server, connection management, and proxying work. When the request will hit the Netty server, it will proxy the request to the inbound filter.
- **The inbound filter** is responsible for authentication, routing, or decorating the request. Then it forwards the request to the endpoint filter.
- **The endpoint filter** is used to return a static response or to forward the request to the backend service (or origin as we call it). Once it receives the response from the backend service, it sends the request to the outbound filter.
- **An outbound filter** is used for zipping the content, calculating the metrics, or adding/removing custom headers. After that, the response is sent back to the Netty server and then it is received by the client.


**3. Hystrix**

In a complex distributed system a server may rely on the response of another server. Dependencies among these servers can create latency and the entire system may stop working if one of the servers will inevitably fail at some point. To solve this problem we can isolate the host application from these external failures. Hystrix library is designed to do this job. It helps you to control the interactions between these distributed services by adding latency tolerance and fault tolerance logic. Hystrix does this by isolating points of access between the services, remote system, and 3rd party libraries. The library helps.  

- Stop cascading failures in a complex distributed system.
- control over latency and failure from dependencies accessed (typically over the network) via third-party client libraries.
- Fail fast and rapidly recover.
- Fallback and gracefully degrade when possible.
- Enable near real-time monitoring, alerting, and operational control.
- Concurrency-aware request caching. Automated batching through request collapsing 


**4. Microservice Architecture of Netflix** 

![Microservice-Architecture-of-Netflix](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.003.jpeg)

Netflix’s architectural style is built as a collection of services. This is known as microservices architecture and this power all of the APIs needed for applications and Web apps. When the request arrives at the endpoint it calls the other microservices for required data and these microservices can also request the data from different microservices. After that, a complete response for the API request is sent back to the endpoint. 

**5. EV Cache**

In most applications, some amount of data is frequently used. For faster response, these data can be cached in so many endpoints and it can be fetched from the cache instead of the original server. This reduces the load from the original server but the problem is if the node goes down all the cache goes down and this can hit the performance of the application. To solve this problem Netflix has built its own custom caching layer called EV cache. EV cache is based on **Memcached** and it is actually a wrapper around Memcached. 

![System-Design-Netflix-–-EV-Cache](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.004.jpeg)



**3.2 Design Principles**

The design principles used in the Netflix system are based on the SWEBOK guide. The following principles are applied:

Modularity: The system is decomposed into smaller, loosely coupled modules (microservices) that can be developed, tested, and deployed independently.

Separation of Concerns: Each module focuses on a specific functionality or business domain, ensuring clear responsibilities and reducing complexity.

Scalability: The system is designed to handle a large number of concurrent users and content items by distributing the load across multiple instances.

Fault Tolerance: The system incorporates redundancy, replication, and monitoring mechanisms to handle failures and ensure high availability.

Security: The system applies secure communication protocols, access controls, and encryption to protect user data and prevent unauthorized access.

Performance Optimization: The system employs caching, content delivery networks (CDNs), and data indexing techniques to optimize performance and minimize latency.



**4. Detailed Design**

1. User Management:
   1. Purpose: Handles user authentication, registration, and profile management.
   1. Functionality: Provides functionalities such as user sign-up, login, password recovery, and user profile customization.
   1. Interfaces: Integrates with authentication services (e.g., OAuth) for user authentication. Communicates with the database for user data storage and retrieval.

1. Content Management:
   1. Purpose: Manages the ingestion, categorization, and storage of movies and TV shows within the Netflix library.
   1. Functionality: Includes content metadata management, content recommendation integration, and content search functionalities.
   1. Interfaces: Integrates with content providers for acquiring licensed content. Communicates with the database for content storage and retrieval. Interfaces with recommendation engines for personalized content suggestions.

1. Recommendation Engine:
   1. Purpose: Generates personalized recommendations for users based on their viewing history, preferences, and behavior.
   1. Functionality: Utilizes machine learning algorithms and data analytics to analyze user data and provide accurate and relevant content recommendations.
   1. Interfaces: Interfaces with the user management and content management modules to access user profiles and content metadata. Communicates with the database for storing and retrieving recommendation data.

1. Streaming Infrastructure:
   1. Purpose: Provides the necessary infrastructure for streaming videos to different client devices.
   1. Functionality: Includes video encoding, adaptive bitrate streaming, content caching, and network optimization techniques to ensure smooth and high-quality video playback.
   1. Interfaces: Interfaces with the user management module for user authentication. Communicates with the content management module to access video files. Integrates with CDNs for efficient content delivery.

1. Billing and Subscription:
   1. Purpose: Handles payment processing, subscription management, and user billing.
   1. Functionality: Manages user subscriptions, handles payment transactions securely, and facilitates subscription upgrades and downgrades.
   1. Interfaces: Integrates with external payment gateways for payment processing. Communicates with the user management module for user subscription data.

1. Analytics and Insights:
   1. Purpose: Collects and analyzes user data, viewing patterns, and system performance metrics to derive valuable insights for business decisions, content recommendations, and system optimization.
   1. Functionality: Gathers and processes user activity data, generates reports, and provides insights for content curation and performance optimization.
   1. Interfaces: Interfaces with user management, content management, and recommendation engine modules to access relevant data. Utilizes data analysis tools and algorithms for generating insights.

**Design Decisions and Trade-offs:**

- Scalability: The system architecture is designed to handle a large user base and high traffic demands by utilizing cloud-based infrastructure, auto-scaling mechanisms, and distributed caching techniques.
- Performance vs. Storage: Transcoding and preprocessing multiple copies of video files require significant computational resources and storage space, balancing the trade-off between optimal video quality and efficient storage utilization.
- Personalization: The recommendation engine utilizes machine learning algorithms and data analytics to provide personalized content suggestions, striking a balance between accuracy and computational complexity.
- Network Optimization: The streaming infrastructure employs adaptive bitrate streaming and content caching to optimize network utilization and enhance the user streaming experience, considering trade-offs between bandwidth usage and quality.
- Data Privacy and Security: User data is securely stored and accessed using encryption and authentication mechanisms to ensure privacy and comply with data protection regulations, with trade-offs between data security and system performance.

These design decisions and trade-offs were made to create a scalable, personalized, and secure streaming platform while optimizing performance and enhancing user experience.

![image](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.005.png)


**5. Deployment Architecture**

**5.1 Overview**

The deployment architecture of Netflix involves a distributed system setup that encompasses various components and infrastructure to ensure efficient content delivery and availability. Here are the key elements of the deployment architecture:

1. Client Devices:
   1. Client devices include smartphones, tablets, smart TVs, web browsers, and other devices through which users access the Netflix platform.
   1. Netflix supports a wide range of devices, each with different resolutions, screen sizes, and capabilities.
1. Content Delivery Networks (CDNs):
   1. CDNs play a crucial role in the deployment architecture by distributing content geographically closer to the end-users, reducing latency and improving streaming performance.
   1. Netflix has built its own CDN called Open Connect, consisting of numerous caching servers deployed in data centers worldwide.
   1. These servers store and deliver the most popular and frequently accessed content, reducing the load on the backend infrastructure.
1. Backend Services:
   1. The backend services form the core of Netflix's deployment architecture, handling various functionalities such as user management, content management, recommendation engines, streaming infrastructure, billing, and analytics.
   1. These services are typically deployed on cloud platforms such as Amazon Web Services (AWS) or other cloud providers.
   1. They are designed to be scalable and resilient, capable of handling a large number of concurrent users and high traffic volumes.
1. Microservices Architecture:
   1. Netflix adopts a microservices architecture, breaking down the system into small, loosely coupled services that can be developed, deployed, and scaled independently.
   1. Each microservice focuses on a specific business capability and communicates with others through well-defined APIs and protocols.
   1. This architecture promotes flexibility, scalability, and fault isolation, allowing Netflix to evolve and update its system components without affecting the entire system.
1. Load Balancing and Auto Scaling:
   1. Load balancing techniques are employed to distribute incoming requests across multiple backend servers, ensuring efficient resource utilization and high availability.
   1. Auto scaling mechanisms automatically adjust the number of server instances based on the current demand, scaling up or down to accommodate traffic fluctuations.
1. Data Storage and Databases:
   1. Netflix utilizes various types of databases and data storage systems to manage user profiles, content metadata, viewing history, and other data.
   1. Relational databases (e.g., MySQL, PostgreSQL) and NoSQL databases (e.g., Cassandra, Amazon DynamoDB) are used based on specific data requirements.
   1. Data is often replicated across multiple data centers to ensure data availability, resilience, and disaster recovery.
1. Monitoring and Logging:
   1. Netflix incorporates robust monitoring and logging systems to track system performance, detect issues, and troubleshoot problems.
   1. Various monitoring tools and services are employed to collect metrics, log events, and generate alerts for timely response and system optimization.

The deployment architecture of Netflix is designed to provide a seamless streaming experience, ensuring content availability, scalability, and performance across a diverse range of client devices. It leverages CDNs, microservices, load balancing, auto scaling, and cloud infrastructure to deliver content efficiently and handle high user demands.



**5.2 Deployment Diagram**

![System Design Netflix - A Complete Architecture - GeeksforGeeks](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.006.png)

![Netflix Service Cloud Architecture [13] | Download Scientific Diagram](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.007.png)




**6. Data Design** 

**6.1 Database** 


Netflix uses two different databases 

- MySQL(RDBMS) 
- ` `Cassandra(NoSQL) for different purposes.  

**EC2 Deployed MySQL**

Netflix saves data like billing information, user information, and transaction information in MySQL because it needs ACID compliance. Netflix has a master-master setup for MySQL and it is deployed on Amazon’s large EC2 instances using InnoDB. 

The setup follows the “**Synchronous replication protocol**” where if the writer happens to be the primary master node then it will be also replicated to another master node. The acknowledgment will be sent only if both the primary and remote master nodes’ write have been confirmed. This ensures the high availability of data. 
Netflix has set up the read replica for each and every node (local, as well as cross-region). This ensures high availability and scalability. 

![System-Design-Netflix-–-MySQL](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.008.jpeg)

All the read queries are redirected to the read replicas and only the write queries are redirected to the master nodes. In the case of a primary master MySQL failure, the secondary master node will take over the primary role, and the route53 (DNS configuration) entry for the database will be changed to this new primary node. This will also redirect the write queries to this new primary master node.  


**Cassandra**

Cassandra is a NoSQL database that can handle large amounts of data and it can also handle heavy writing and reading. When Netflix started acquiring more users, the viewing history data for each member also started increasing. This increases the total number of viewing history data and it becomes challenging for Netflix to handle this massive amount of data. Netflix scaled the storage of viewing history data-keeping two main goals in their mind.

- Smaller Storage Footprint.
- Consistent Read/Write Performance as viewing per member grows (viewing history data write-to-read ratio is about 9:1 in Cassandra).

![System-Design-Netflix-Cassandra-Service-Pattern](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.009.png)


**Total Denormalized Data Model**  

- Over 50 Cassandra Clusters
- Over 500 Nodes
- Over 30TB of daily backups
- The biggest cluster has 72 nodes.
- 1 cluster over 250K writes/s


Initially, the viewing history was stored in Cassandra in a single row. When the number of users started increasing on Netflix the row sizes as well as the overall data size increased. This resulted in high storage, more operational cost, and slow performance of the application. The solution to this problem was to compress the old rows.


Netflix divided the data into two parts.

- **Live Viewing History (LiveVH):** This section included the small number of recent viewing historical data of users with frequent updates. The data is frequently used for the ETL jobs and stored in uncompressed form.
- **Compressed Viewing History (CompressedVH):** A large amount of older viewing records with rare updates is categorized in this section. The data is stored in a single column per row key, also in compressed form to reduce the storage footprint.


**Data Processing in Netflix Using Kafka And Apache Chukwa**

When you click on a video Netflix starts processing data in various terms and it takes less than a nanosecond. Let’s discuss how the evolution pipeline works on Netflix. 

Netflix uses Kafka and Apache Chukwe to ingest the data which is produced in a different part of the system. Netflix provides almost **500B** data events that consume **1.3 PB/day** and 8 million events that consume 24 GB/Second during peak time. These events include information like:

- Error logs
- UI activities
- Performance events
- Video viewing activities
- Troubleshooting and diagnostic events.


**Apache Chukwe** is an open-source data collection system for collecting logs or events from a distributed system. It is built on top of HDFS and Map-reduce framework. It comes with Hadoop’s scalability and robustness features. Also, it includes a lot of powerful and flexible toolkits to display, monitor, and analyze the result. Chukwe collects the events from different parts of the system and from Chukwe you can do monitoring and analysis or you can use the dashboard to view the events. Chukwe writes the event in the Hadoop file sequence format (S3). After that Big Data team processes these S3 Hadoop files and writes Hive in Parquet data format. This process is called batch processing which basically scans the whole data at the hourly or daily frequency. 

To upload online events to EMR/S3, Chukwa also provide traffic to Kafka (the main gate in real-time data processing). Kafka is responsible for moving data from fronting Kafka to various sinks: S3, Elasticsearch, and secondary Kafka. Routing of these messages is done using the **Apache Samja** framework. Traffic sent by the Chukwe can be full or filtered streams so sometimes you may have to apply further filtering on the Kafka streams. That is the reason we consider the router to take from one Kafka topic to a different Kafka topic.  


**Elastic Search**

In recent years we have seen massive growth in using Elasticsearch within Netflix. Netflix is running approximately **150** clusters of elastic search and **3, 500** hosts with instances. 

Netflix is using elastic search for data visualization, customer support, and for some error detection in the system. For example, if a customer is unable to play the video then the customer care executive will resolve this issue using elastic search. The playback team goes to the elastic search and searches for the user to know why the video is not playing on the user’s device. They get to know all the information and events happening for that particular user. They get to know what caused the error in the video stream. Elastic search is also used by the admin to keep track of some information. It is also used to keep track of resource usage and to detect signup or login problems.  

**Apache Spark For Movie Recommendation**

Netflix uses Apache Spark and Machine learning for Movie recommendations. Let’s understand how it works with an example. When you load the front page you see multiple rows of different kinds of movies. Netflix personalizes this data and decides what kind of rows or what kind of movies should be displayed to a specific user. This data is based on the user’s historical data and preferences. Also, for that specific user, Netflix performs sorting of the movies and calculates the relevance ranking (for the recommendation) of these movies available on their platform. 

In Netflix, Apache Spark is used for content recommendations and personalization. A majority of the [machine learning](https://www.geeksforgeeks.org/machine-learning/) pipelines are run on these large spark clusters. These pipelines are then used to do row selection, sorting, title relevance ranking, and artwork personalization among others. 

**Artwork Personalization**

When you open the Netflix front page you might have noticed the images for each video…these images are called header images (thumbnails). Netflix wants maximum clicks for the videos from the users and these clicks are dependent on the header images. Netflix has to choose the right compelling header image for a specific video. To do that Netflix creates multiple artworks for a specific movie and they display these images to the users randomly. For the same movie, images can be different for different users. Based on your preferences and viewing history Netflix predicts what kind of movies you like best or which actors you like the most in a movie. According to users’ tastes, the images will be displayed to them. 

For example, suppose you see 9 different images for your favorite movie Good will hunting in three rows (If you like comedies then images of Robin Williams for this movie will be shown. If you like romantic movies then Netflix will show you the image of Matt Damon and Minnie Driver). Now, Netflix calculates the number of clicks a certain image receives. If the clicks for the center image of the movie are 1, 500 times and the other images have fewer clicks then Netflix will make the center image a header image for the movie Good Will Hunting forever. This is called **data-driven** and Netflix performs the data analytics with this approach. To make the right decision data is calculated based on the number of views associated with each picture. 

**Video Recommendation System**

If a user wants to discover some content or video on Netflix, the recommendation system of Netflix helps users to find their favorite movies or videos. To build this recommendation system Netflix has to predict the user interest and it gathers different kinds of data from the users such as:

- User interaction with the service (viewing history and how the user rated other titles)
- Other members with similar tastes and preferences.
- Metadata information from the previously watched videos for a user such as titles, genre, categories, actors, release year, etc.
- The device of the user, at what time a user is more active, and for how long a user is active.

Netflix uses two different algorithms to build a recommendation system:

**1. Collaborative filtering:** The idea of this filtering is that if two users have similar rating histories then they will behave similarly in the future. For example, consider there are two-person. One person liked the movie and rated the movie with a good score. Now, there is a good chance that the other person will also have a similar pattern and he/she will do the same thing that the first person has done. 

**2. Content-based filtering:** The idea is to filter those videos which are similar to the video a user has liked before. Content-based filtering is highly dependent on the information from the products such as movie title, release year, actors, the genre. So to implement this filtering it’s important to know the information describing each item and some sort of user profile describing what the user likes is also desirable.  



**6.2 Data Flow**

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.010.png)







**7. User Interface Design** 

**7.1 User Interface Wireframes** 

Lo-fi Wireframes

It’s useful to draw all the screeens we will need with those essential elements that the wireframes should represent:

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.011.jpeg)

User Interface Elements

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.012.jpeg)

Many of the **user interface (UI)** elements I’ve used to create the wireframes have been repeated throughout the screens, however, this is a list with the elements I’ve used:

**Input Controls** allow to the user to perform a variety of functions (e.g. check a box) or select items from a list dropdown. Elements used: lists, list boxes, buttons.

**Navigational Components** help to the user to move around the screen. Elements used: breadcrumb, slider, tags, icons.

**Informational Components** share information (e.g. help, suggestions) with the user. Elements used: icons, progress bar, notifications.

**Containers** hold related content together. Element used: accordion.




**7.2 User Interaction Patterns** 

**Tasks** are services encapsulating the business logic that runs outside the Conductor server and are implemented as a Microservice, Lambda, or Worker. Workers run outside the Conductor server and can be implemented in any supported language. A workflow can contain multiple workers written in different languages.

**Operators** are primitives from programming languages that are used to control the flow of execution inside a workflow. Conductor supports operators such as switch, loop, fork/join, and sub-workflows, allowing you to define complex workflows.

Netflix Conductor is a powerful platform that lets you create the most complex workflows while making it very easy to handle runtime scenarios with powerful debugging and visualization tools, reducing the mean time to detect and resolve issues in the production environment.
**


**8. Security Design** 

**8.1 Authentication and Authorization** 

**1. [Kerberos](https://www.geeksforgeeks.org/kerberos/) :** 

Kerberos is a protocol that aids in network authentication. This is used for validating clients/servers during a network employing a cryptographic key. It is designed for executing strong authentication while reporting to applications. The overall implementation of the Kerberos protocol is openly available by MIT and is used in many mass-produced products. 

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.013.png)

**Some advantages of Kerberos :** 

- It supports various operating systems.
- The authentication key is shared much efficiently than public sharing.


**8.2 Data Protection** 

Netflix uses a technology called digital rights management (DRM) to encrypt its content and prevent unauthorized users from accessing it. When you take a screenshot of Netflix content, the DRM software prevents the screenshot from being saved or shared.

**8.3 Secure Communication** 

Problems with HTTPS

One of the largest problems with HTTPS is the PKI infrastructure. There were a number of short-lived incidents where a renewed server certificate caused outages. We had no good way of handling revocation: our attempts to leverage CRL and OCSP technologies resulted in a complex set of workarounds to deal with infrastructure downtimes and configuration mistakes, which ultimately led to a worse user experience and brittle security mechanism with little insight into errors. Recent security breaches at certificate authorities and the issuance of intermediate certificate authorities means placing trust in one actor requires placing trust in a whole chain of actors not necessarily deserving of trust.

High-level Goals

Before starting to design MSL we had to identify its high-level goals. Other than general best practices when it comes to protocol design, the following objectives are particularly important given the scale of deployment, the fact it must run on multiple platforms, and the knowledge it will be used for future unknown use cases.

**Cross-language**. Particularly subject to JavaScript constraints such as its maximum integer value and native functions found in web browsers.

**Automatic error recovery**. With millions of devices and subscribers we need devices that enter a bad state to be able to automatically recover without compromising security.

**Performance**. We do not want our application performance and responsiveness to be limited any more than it has to be. The network is by far the most expensive performance cost.

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.014.png)

Figure 1: HTTP vs. HTTPS Performance

**Flexible and extensible**. Whenever possible we want to take advantage of security features provided by devices and their software. Likewise if something no longer provides the security we need then there needs to be a migration path forward.

**Standards compatible**. Although related to being flexible and extensible, we paid particular attention to being standards compatible. Specifically we want to be able to leverage the Web Crypto API now available in the major web browsers.

Security Properties

MSL is a modern cryptographic protocol that takes into account the latest cryptography technologies and knowledge. It supports the following basic security properties.

**Integrity protection**. Messages in transit are protected from tampering.

**Encryption**. Message data is protected from inspection.

**Authentication**. Messages can be trusted to come from a specific device and user.

**Non-replayable**. Messages containing non-idempotent data can be non-replayable.

MSL supports two different deployment models, which we refer to as MSL network types. A single device may participate in multiple MSL networks simultaneously.

**Trusted services network**. This deployment consists of a single client device and multiple servers. The client authenticates against the servers. The servers have shared access to the same cryptographic secrets and therefore each server must trust all other servers.

**Peer-to-peer**. This is a typical p2p arrangement where each each side of the communication is mutually authenticated.

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.015.png)

Figure 2: MSL Networks







**9.Error Handling and Logging**

` `**9.1 Exception Handling** 

All handled exceptions in Zuul are *ZuulExceptions*. Now, let's make it clear that ***ZuulException* can't be caught by [*@ControllerAdvice*](https://www.baeldung.com/exception-handling-for-rest-with-spring#controlleradvice) and annotating the method by [*@ExceptionHandling***](https://www.baeldung.com/exception-handling-for-rest-with-spring#exceptionhandler)**. This is because ***ZuulException* is thrown from the error filter**. So, it skips the subsequent filter chains and never reaches the error controller. The following picture depicts the hierarchy of error handling in Zuul:



![Customizing Zuul Exceptions](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.016.png)

**9.2 Logging and Monitoring** 

As the traffic was ramped up, we have been able to scale up very comfortably so far learning, lessons as we went along.

Data is being analyzed multiple ways by our algorithmic teams. For example — which row types (Top 10, most recently watched etc.) did most plays emanate from. How did that vary by country and device. How far did users scroll left / right across devices — and do users ever go beyond a certain point. These and many other data points are being examined to improve our algorithms to provide users with a better viewing experience.

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.017.png)

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.018.png)

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.019.png)

![](Aspose.Words.f480f557-6803-4913-8ca6-6361c71f11bd.020.png)



**10. Conclusion** 

The Software Design Document provides a comprehensive overview of the design principles, architecture, and key components of the Netflix software system. It serves as a guide for the development team to ensure the successful implementation of a robust, scalable, and maintainable system.

