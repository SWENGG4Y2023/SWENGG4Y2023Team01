# Software Requirement Specification

## Table of Contents
-  [1. Introduction](#1-introduction)
   - [1.1 Purpose](#11-purpose)
   - [1.2 Intended Audience](#12-intended-audience)
   - [1.3 Intended Use](#13-intended-use)
   - [1.4 Scope](#14-scope)
   - [1.5 Definitions, Acronyms and Abbreviations](#15-definitions-acronyms-and-abbreviations)
- [2.Overall Description](#overall-description)
   -  [2.1 Product Perspective](#21-product-perspective)
   -  [2.2 Product Features](#22-product-features)
   -  [2.3 User Classes and Characteristics](#23-user-classes-and-characteristics)
   -  [2.4 Operating Environment](#24-operating-environment)
   -  [2.5 Design and Implementation Constraints](#25-design-and-implementation-constraints)
   -  [2.6 Assumptions & Dependencies](#26-assumptions--dependencies)
-  [3. System Features](#3-system-features)
   -  [3.1 Functional Requirements](#31-functional-requirements)
## 1. Introduction

   - ### **1.1 Purpose**
      The purpose of this document is to give a detailed description of therequirements for “Netflix – Movie and TV Streaming”. It willillustrate the purpose and complete declaration for the development ofsystem. It will also explain system constraints, interface andinteractions with other external applications. This document is primarily intended to be proposed to the customer company for theirapproval and a reference for development of the system.

   -  ### **1.2 Intended Audience**
      The intended audience for Netflix, as a streaming service, is primarily consumers or end-users who are interested in watching movies, TV shows, documentaries, and other types of video content online. Netflix is designed to cater to a wide range of users, including individuals, families, and even businesses, who have varying interests, preferences, and viewing habits.

   -  ### **1.3 Intended Use**
      The intended use of Netflix is to provide a subscription-based streaming service that allows users to access a vast library of movies, TV shows, documentaries, and other video content on-demand, anytime and anywhere, using internet-connected devices such as smart TVs, computers, tablets, and smartphones. Users can create personalized profiles, customize their viewing preferences, and interact with the system's features to discover, select, and watch content according to their interests and preferences.

   -  ### **1.4 Scope**
      Scope of this product include the development of a good database andan attractive interface so that people can easily use it. The user has tosubscribe a plan and after that user can watch according to the planhe/she subscribed. The user also has the facility to see the list ofMovies and Tv Serials which they had watched previouslyrecommended Movies and Tv Serials. By this user will find the showswhich they like without wasting their time, It’s beneficial for bothcompany and users.


   -  ### **1.5 Definitions, Acronyms and Abbreviations**

      #### Mylist
      - Mylist is a location or folder where user can savetheir videos things for watching later. When user wants towatch his or her favorite shows.

      
      #### Account
      - Account term basically refers to an identity on Netflix website. A user can easily make an account on Netflixwebsite by just giving some credentials like E-mail and password. User can store list of videos in mylist for watchingafterwards.

      
      #### Categories
      - It is the classification of all of the types ofshows which are available on website for watching. Sametype of videos putted in same category.
      
      #### Offline Mode
      - Offline mode is a mode where user can savehis or her favorite videos for watching offline withoutopening website.

      
      #### Download
      - It usually refers to transfer data from a larger"host" system(especially a server or mainframe) to a smaller"client" system, especially a microcomputer or mobiledevice.
      
      #### Rating
      - A classification or ranking of someone orsomething based on a comparative assessment of theirquality, standard, or performance.
      
      #### Subscribe
      - An arrangement to receive something, typicallya publication, regularly by paying in advance.



##  **Overall Description**

   - ### **2.1 Product Perspective**
      Netflix is developed to everyone to allow our customers to watch a wide variety of award-winning TV shows, movies, documentaries, and more on thousands of internetconnected devices. With Netflix, you can enjoy unlimited viewing of our content without having to watch a single commercial.

   -  ### **2.2 Product Features**
         **Content Library:** Netflix offers a vast collection of movies, TV shows, documentaries, and other forms of content from various genres, including drama, comedy, action, horror, sci-fi, and more.

         **Personalized Recommendations:** Netflix uses algorithms to provide personalized recommendations to users based on their viewing history, ratings, and preferences, helping users discover new content that aligns with their interests.

         **Multiple Profiles:** Netflix allows users to create multiple profiles under one account, making it possible for different family members or individuals to have their personalized viewing history, recommendations, and watchlists.

         **Offline Viewing:** Netflix allows users to download select content to watch offline, making it convenient for users to enjoy their favorite shows and movies without requiring an internet connection.

         **Continuous Playback:** Netflix automatically plays the next episode of a TV series, making it easy for binge-watchers to seamlessly watch multiple episodes in a row without having to manually start each episode.

         **Parental Controls:** Netflix offers parental controls that allow parents or guardians to set up age-appropriate content restrictions and create profiles specifically for kids, ensuring a safe viewing experience.

         **Search and Filtering:** Netflix provides a search feature that allows users to search for specific titles, actors, or genres. Users can also filter content based on different criteria such as release year, ratings, and language.

         **Cross-Platform Access:** Netflix is available on multiple devices, including smartphones, tablets, smart TVs, gaming consoles, and web browsers, allowing users to access their content across different platforms.
           
   -  ### 2.3 **User Classes and Characteristics**
      - Subscribers: These are the end-users who subscribe to the Netflix service and pay for access to the content. They may have different preferences and interests in terms of the types of content they want to watch, such as movies, TV shows, documentaries, or children's programming. 
      - Content Creators: These are the individuals or companies who create the content that Netflix streams. They may have specific technical requirements for the video and audio quality, encoding formats, and other aspects of the content delivery process. 
      - Content Providers: These are the companies that license content to Netflix, such as movie studios, TV networks, and independent production companies. They may have specific requirements for the terms of the licensing agreement, payment schedules, and content usage restrictions. 
      - Business Partners: These are the companies that work with Netflix to provide complementary services, such as internet service providers, payment processors, and technology vendors. They may have specific technical requirements for integrating their systems with Netflix's platform. 
      
         **characteristics of these user classes could include:**
         * Subscribers: They may have different levels of technical expertise, but all expect an easy-to-use and reliable streaming service that offers a broad range of high-quality content. 
         * Content Creators: They may have specific technical requirements for video and audio quality, as well as a desire to protect their intellectual property rights and control how their content is distributed. 
         * Content Providers: They may have specific contractual requirements and business goals related to licensing their content to Netflix, such as maximizing revenue and protecting their brand image. 
         * Business Partners: They may have technical requirements related to integrating their systems with Netflix's platform, as well as contractual requirements related to payment schedules and data sharing.

   -  ### **2.4 Operating Environment**
         PC or Laptops :

            Windows 7, Windows 8, Windows 8.1, Windows 10, Mac OS X, Linux,
            Smart TV’s, Smart Phones, Tablets (Android or IOS) etc.


   -  ### **2.5 Design and Implementation Constraints**
      Server capacity is how many users can access or can be online once. More is the number of users more will be the network traffic and hence the serve comes in a down state. Personal firewall and updating is a tough task, it should be such that it should not block the network traffic,making the system slower. Firewall of the server should not collide with the firewall of the user system.

   - ### **2.6 Assumptions & Dependencies**

      **Assumptions:**
      1. Internet Access: It is assumed that users of Netflix will have reliable internet access to stream content online.

      2. Device Compatibility: It is assumed that users will access Netflix on compatible devices, such as smart TVs, computers, tablets, and smartphones.

      3. User Authentication: It is assumed that users will need to create an account and sign in to access Netflix content, and that appropriate authentication mechanisms will be in place to ensure user privacy and security.

      4. Content Availability: It is assumed that Netflix will have the necessary licenses and agreements in place to make content available for streaming in the target regions.

      5. Payment Processing: It is assumed that Netflix will have secure payment processing mechanisms in place for users to subscribe to the service and make payments for subscription fees.

      6. Content Categorization: It is assumed that content on Netflix will be properly categorized and tagged for easy search and discovery by users.

      7. Content Quality: It is assumed that content on Netflix will meet certain quality standards, such as resolution, audio quality, and subtitle availability, to ensure a satisfactory user experience.

      **Dependencies:**

      1. Third-party APIs: Netflix may depend on third-party APIs, such as payment gateways or content delivery networks (CDNs), for secure payment processing and efficient content delivery.

      2. Content Licensing: Netflix may depend on content licensing agreements with studios, production companies, and other content providers to legally stream their content on the platform.

      3. Regional Regulations: Netflix may be subject to regional regulations and compliance requirements, such as content censorship laws, which may impact the availability of certain content in specific regions.

      4. Technology Infrastructure: Netflix may depend on reliable and scalable technology infrastructure, including servers, databases, and networking components, to support its streaming service and handle the large volume of user requests.

      5. User Devices: Netflix may depend on users' devices, such as smart TVs, computers, tablets, and smartphones, to meet certain technical requirements, such as screen size, resolution, and audio capabilities, to ensure an optimal viewing experience.

      6. Internet Service Providers: Netflix may depend on reliable internet service providers (ISPs) to ensure that users have sufficient bandwidth and network stability to stream content seamlessly.

# 3. System Features

   - ## **3.1 Functional Requirements**
     
      1. **User Registration and Authentication:** Users should be able to create accounts, sign in, and manage their authentication credentials securely.

      2. ***Content Catalog:*** The service should have a comprehensive catalog of movies, TV shows, and other video content, organized by genres, release dates, and other relevant criteria. Users should be able to browse and search for content easily.

      3. ***Streaming and Playback:*** Users should be able to stream video content in various quality levels, depending on their internet connection, and have controls for playback, such as play, pause, rewind, and fast forward. The service should also support multiple devices, such as web browsers, mobile devices, smart TVs, and streaming media players.

      4. ***Personalization and Recommendations:*** The service should provide personalized recommendations to users based on their viewing history, ratings, and preferences. Users should be able to create profiles, manage their viewing history, and customize their recommendations.

      5. ***Content Management:*** Administrators should be able to upload, manage, and organize video content, including metadata such as titles, descriptions, images, and trailers. The service should also support content scheduling, content ratings, and content localization.

      6. ***User Interaction:*** Users should be able to rate and review content, add content to their watchlist, create playlists, and share content with others through social media or other channels. The service should also provide features for user feedback, reporting issues, and customer support.

      7. ***Billing and Subscriptions:*** The service should support various subscription plans, with different pricing tiers, billing cycles, and payment methods. Users should be able to manage their subscription status, update their payment information, and receive notifications about billing-related events.

      8. ***Social Features:*** The service may include social features, such as user profiles, social media integration, and social sharing of content. Users should be able to connect with friends, see their friends' activities, and discover new content based on their friends' recommendations.

      9. ***Search and Recommendations:*** The service should provide robust search capabilities, including filters, sorting, and advanced search options. Users should be able to find content easily based on title, actors, directors, and other criteria. The service should also provide relevant recommendations based on user preferences and viewing history.

      10. ***Offline Viewing:*** The service may provide an option for users to download content for offline viewing, allowing them to watch content without an internet connection. Users should be able to manage their downloaded content, including viewing history and storage management.

      11. ***Accessibility:*** The service should be designed to be accessible to users with disabilities, following accessibility guidelines and standards. This may include features such as closed captioning, audio descriptions, and other accessibility options.

      12. ***Security and Privacy:*** The service should implement robust security measures to protect user data, including authentication, encryption, and data privacy. The service should also comply with applicable privacy regulations, such as GDPR or CCPA, and provide options for users to manage their privacy settings, data sharing, and consent preferences.
