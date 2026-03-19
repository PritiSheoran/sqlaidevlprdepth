# Day 01 : Implement AI Solutions with Azure SQL Database and SQL Server 2025

### Overall Estimated Duration: 4 Hours

## Overview
 
In this lab, you will design and implement an AI-powered medical search solution that enables intelligent, context-aware retrieval of healthcare information. Working with a realistic scenario at Contoso Medical College Hospital, you will build a Medical Research Assistant that allows users to ask natural language questions and receive clinically relevant, research-backed responses. Instead of relying solely on traditional keyword searches, the solution leverages SQL Server 2025 integrated with Azure OpenAI to generate and store vector embeddings directly within the database. This SQL-first semantic retrieval approach transforms the database into an AI-enabled healthcare data platform. By combining secure data storage, intelligent search, and AI capabilities, you will create a system that delivers accurate answers grounded strictly in verified medical research and patient case data.

## Objective

By the end of this lab, participants will be able to:

  - Building a Semantic Patient Case Search Engine for Healthcare Using SQL Server 2025

  - Developing a Knowledge-Augmented Medical Library Research Assistant with RAG and Azure SQL Database

## Pre-requisites

Participants should have:

- A working knowledge of Microsoft Azure services, including resource groups, virtual machines, Azure SQL, and Azure OpenAI.

- Basic experience with SQL Server or Azure SQL Database, including writing T-SQL queries and managing databases using SSMS.

- Familiarity with AI concepts such as embeddings, vector search, and Retrieval-Augmented Generation (RAG).

- Understanding of authentication methods, networking configuration, and secure access to Azure resources.

- General awareness of how AI services integrate with databases for intelligent search and chat-based applications.

## Explanation of Components

The architecture for this lab involves the following key components:

1. **Azure SQL / SQL Server 2025:**
The main data platform used to store patient cases and medical research data.  
    - In Exercise 1 uses SQL Server 2025 on Azure VM with in-database AI features.  
    - In Exercise 2 uses Azure SQL Database for structured research storage.  
    - Supports secure storage, querying, and relational filtering.

1. **Azure OpenAI Service:**
Provides AI models for embeddings and chat.  
    - Converts text into vector embeddings for semantic search.  
    - Generates natural language responses based on retrieved content.

1. **Vector Embeddings:**
Text data is converted into numeric vectors.  
    - Stored in SQL tables or indexed in Azure AI Search.  
    - Enables similarity search using cosine distance.

1. **Azure AI Search**
Used for intelligent document retrieval.  
    - Performs hybrid search (keyword + vector).  
    - Returns relevant documents for grounded responses. 

1. **RAG (Retrieval-Augmented Generation):**
Combines retrieval with AI response generation.  
    - Retrieves trusted data first.  
    - Ensures answers are based only on enterprise medical content.

1. **AI Logic Layer**
Handles search and response control.  
    - In Exercise 1 uses stored procedures for semantic case retrieval.  
    - In Exercise 2 uses an AI agent with grounding policies.   

## Getting Started with Lab
Once you're ready to dive in, your virtual machine and **Guide** will be right at your fingertips within your web browser.

![Image](../Lab%20Guides/Lab%201/media/102.png)

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕ : 100%** icon located next to the timer in the lab environment.

![Image](../Lab%20Guides/Lab%201/media/90.png)

## Virtual Machine & Lab Guide
Your virtual machine is your workhorse throughout the workshop. The guide is your roadmap to success.

## Exploring Your Lab Resources
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.

![Image](../Lab%20Guides/Lab%201/media/91.png)

## Utilizing the Split Window Feature
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the top right corner.

![Image](../Lab%20Guides/Lab%201/media/92.png)

## Managing Your Virtual Machine
Feel free to **start, restart, or stop (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!

![Image](../Lab%20Guides/Lab%201/media/93.png)

## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the **Azure Portal** icon.

    ![Image](../Lab%20Guides/Lab%201/media/94.png)

1. On the **Sign in to Microsoft Azure** tab, you will see the login screen. Enter the following email/username, and click on **Next (2)**. 

   * **Email/Username**: <inject key="AzureAdUserEmail"></inject> **(1)**
   
      ![Image](../Lab%20Guides/Lab%201/media/95.png)
     
1. Now enter the following Temparory Access Pass and click on **Sign in (2)**.
   
   * **Temporaray Access Pass**: <inject key="AzureAdUserPassword"></inject> **(1)**

      ![Image](../Lab%20Guides/Lab%201/media/96.png)
     
1. If you see the pop-up **Stay Signed in?**, select **No**.

   ![Image](../Lab%20Guides/Lab%201/media/97.png)

1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

1. If a **Welcome to Microsoft Azure** popup window appears, select **Maybe Later** to skip the tour.

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: cloudlabs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

Click **Next >>** from the bottom right corner to embark on your Lab journey!

![Image](../Lab%20Guides/Lab%201/media/98.png)

### Happy Learning!!