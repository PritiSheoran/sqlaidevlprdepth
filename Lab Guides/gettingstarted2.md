# Day 02 : Implement AI Solutions with Azure SQL Database and SQL Server 2025

### Overall Estimated Duration: 4 Hours

## Overview
 
In this lab, you will design and build a secure, AI-powered database solution that enables intelligent search and modern API integration. Working across healthcare and retail scenarios, you will use SQL Server 2025, GitHub Copilot/SSMS Copilot, Azure OpenAI Service, and Data API Builder to develop semantic search and inventory management capabilities. You will integrate AI directly into the database to support natural language queries and automate development tasks. The lab also emphasizes enterprise-grade security through RBAC, data masking, and managed identity. Finally, you will expose your database as REST and GraphQL APIs, demonstrating how modern SQL development combines AI, security, and API-driven architecture.

## Objective

By the end of this lab, participants will be able to:

  - Building and Securing a Safe Clinical Report Search API

  - Building a Secure Inventory System with SQL Server 2025, GitHub Copilot, and Data APIs

## Pre-requisites

Participants should have:

- A basic understanding of Microsoft Azure services, including Virtual Machines, Azure OpenAI, and identity management concepts.

- Working knowledge of SQL Server 2025, including creating databases, tables, stored procedures, and writing T-SQL queries.

- Familiarity with database security concepts such as Role-Based Access Control (RBAC), logins, users, permissions, and Dynamic Data Masking.

- Basic understanding of AI concepts such as embeddings, vector search, and how AI integrates with databases.

- Experience using development tools like SQL Server Management Studio (SSMS), Visual Studio Code, GitHub Copilot, and command-line tools.

## Explanation of Components

The architecture for this lab involves the following key components:

1. **SQL Server 2025:**  
The core database platform used to store clinical reports and inventory data.  
    - Supports AI features such as vector data types and semantic search.  
    - Handles schema design, stored procedures, and enterprise-grade security controls.

2. **Azure OpenAI Service:**  
Provides AI capabilities for generating embeddings.  
    - Converts text into vector representations for semantic search.  
    - Integrates securely with SQL Server using API keys or Managed Identity.

3. **Vector Search & Indexing**  
Enables intelligent similarity-based search.  
    - Embeddings are stored in vector columns.  
    - DiskANN indexing improves performance for large datasets.  
    - Supports both exact and approximate nearest neighbor (ANN) search.

4. **Security Layer (RBAC & Data Masking):**  
Protects sensitive business and patient data.  
    - Role-Based Access Control restricts user permissions.  
    - Dynamic Data Masking hides sensitive fields from unauthorized users.  
    - Managed Identity secures service-to-service communication.

5. **GitHub Copilot / SSMS Copilot:** 
Provides AI-assisted development.  
    - Generates queries, stored procedures, and schema designs.  
    - Improves productivity and reduces manual coding effort.

6. **Data API Builder:**  
Exposes database objects as secure REST and GraphQL APIs.  
    - Allows applications to access SQL data through modern API endpoints.  
    - Connects SQL databases with web and AI applications.

## Getting Started with Lab
Once you're ready to dive in, your virtual machine and **Guide** will be right at your fingertips within your web browser.

![Image](../Lab%20Guides/Lab%201/media/gs.png)

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