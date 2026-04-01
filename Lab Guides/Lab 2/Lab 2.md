# Lab 2: Developing a Knowledge-Augmented Medical Library Research Assistant with RAG and Azure SQL Database

In this lab, you take on the role of building a Medical Research
Assistant that helps users explore trusted medical research more
efficiently. Instead of manually browsing through multiple documents,
users can ask questions in natural language and receive clear,
research-backed answers. By integrating database storage, intelligent
search, and AI capabilities, you create a secure solution that ensures
responses are grounded only in verified medical research data.

**Objectives:**

By completing this lab, you will be able to:

- Create and configure an Azure SQL Database to store medical research
  data

- Import and manage structured research documents

- Generate embeddings for research content

- Configure vector and hybrid search for intelligent data retrieval

- Implement a Retrieval-Augmented Generation (RAG) architecture

- Build and test an AI-powered chat agent

- Ensure responses are grounded strictly in trusted enterprise data

## Exercise 1: Create SQL Database Server 

    
1. In the Azure portal, type **Azure SQL** into the search bar and select it from the results.

    ![](./media/image1.png)

1. Expand **Azure SQL Database | SQL logical servers** from the left menu and select **Create.**

    ![](./media/image2.png)

1. Enter the values below and click on **Next: Networking**

    - Subscription : **Default Subscription**

    - Resource Group: **AIDeveloper**

    - Server Name: **azdbsqlserver<inject key="DeploymentID" enableCopy="false"/>**

    - Region: **<inject key="Region" enableCopy="false"></inject>**

    - Authentication method: Use SQL authentication

    - Server admin login: **sqladmin**

    - Password: **Pa55w0rd12345**

      ![](./media/image3.png)

      ![](./media/image4.png)

1. Toggle **Yes** to Allow Azure services and resources to access this
    server, and then click on **Next: Security.**

    ![](./media/image5.png)

1. Click on the **Configure identities** link under the Identity
    section.

    ![](./media/image6.png)

1. Select **Status On** and click on Apply.

    ![](./media/image7.png)

1. Click on **Review +create.**

    ![](./media/image8.png)

1. Review the details and click on **Create.**

    ![](./media/image9.png)

1. Wait for the deployment to be completed, and then click on **Go to resource.**

    ![](./media/image10.png)

    ![](./media/image11.png)

1. Click on Networking from the left navigation under security, click on Add your
    client and then click on save.

    ![](./media/image12.png)

1. Click on Overview and copy the **server name** to **Notepad** to use
    in the next task.

    ![](./media/image13.png)

## Exercise 2: Create an Azure SQL Database

1. Enter **Azure SQL** in the search bar and select **Azure SQL database.**

    ![](./media/image14.png)

1. Select **Create -\> SQL database.**

    ![](./media/image15.png)

1. Select the following values, keep all other settings as default, and then click **Next: Networking >**:

    - Name: **ContosoMedicalResearch**

    - Server: **azdbsqlserver<inject key="DeploymentID" enableCopy="false"/>**

      ![](./media/image16.png)

1. Enable Add current client IP address and then click **Review + create.**

    ![](./media/image18.png)

1. Review the details and click on Create.

    ![](./media/image19.png)

1. Wait for the deployment and click on **Go to resource.**

    ![](./media/image20.png)

## Exercise 3: Create a database via SSMS and upload a CSV file.

1. From the LabVM, search for **SQL Server Management Studio 22** and open it.

1. Enter below details and click **Continue**:

    - Server name: **The Server name you copied previously**

    - Authentication : **SQL Server Authentication**

    - Username: **sqladmin**

    - Password: **Pa55w0rd12345**

    - Select **Trust Server certificate** checkbox

      ![](./media/image26.png)

      ![](./media/image27.png)

1. Select the Copilot icon in the top right, Prompt Copilot to create a database:

    ```
    @workspace In the ContosoMedicalResearch database, write a T-SQL query to create a table named LibraryData with the following columns:
    - BookID INT PRIMARY KEY
    - Title NVARCHAR(200) NOT NULL
    - Author NVARCHAR(100)
    - Genre NVARCHAR(100)
    - PublishedYear INT
    - Summary NVARCHAR(MAX)
    Make the query executable and include IF NOT EXISTS check.
    ```

    > **Note:** It may not generate an executable query. You can copy the generated query and run in the query editor. This is completely optional. You can proceed with the next step.

    > **Note:** Write any prompt and send it, and it will ask you to sign in first. Use your personal or work GitHub credentials to log in to use GitHub Copilot features.
 
    ![](./media/image28.png)

    ![](./media/latest1.png)

1. Right-click **DB → Tasks → Import Flat File** as shown in the image below.

    ![](./media/image31.png)

1. Click on **Next** on the Introduction page.

    ![A screenshot of a computer Description automatically
    generated](../Lab%201/media/image61.png)

1. **Browse** the file on the Specify Input file section.

    ![A screenshot of a computer Description automatically
    generated](./media/latest2.png)

1. Select **library_books.csv (2)** file from **C:\Labfiles (1)** folder and click **Open (3)**.

    ![](./media/latest3.png)    

1. Browse the file *library_books.csv* form `C:\Labfiles` folder, enter the table name as **MedicalResearch** and click **Next**.

    ![](./media/latest4.png)

1. Preview the data and click Next.

    ![](./media/image34.png)

1. Click Next on Modify column.

    ![](./media/image35.png)

1. On the summary page, click Finish.

    ![](./media/image36.png)

1. Once the operation is completed, close the window.

    ![](./media/image37.png)

1. In **Object Explorer**, right-click the **ContosoMedicalRes** database and select **New Query**.

    ![](./media/image37b.png)

1. Run below queries to verify the data in the table.

    ```
    SELECT COUNT(*) FROM dbo.MedicalResearch;
    SELECT TOP 5 Title, Category FROM dbo.MedicalResearch;
    ```

    ![](./media/image38.png)

## Exercise 4: Create an Azure OpenAI service and deploy chat and embedding models

1. Switch back to Azure and search for **Azure OpenAI** and select it.

    ![](./media/image39.png)

1. Navigate to azsqlaoai<inject key="DeploymentID" enableCopy="false"/> and click on **Go to Foundry portal**

    ![](./media/image39a.png)

1. Click on Deployments under Shared resource from the left navigation
    menu. Select Deploy model-\> Deploy base model.

    ![](./media/image30-2.png)

1. Search for **gpt** models and select the **gpt-5.2-chat** model, and click Confirm.

    ![](./media/image54.png)

1. Click on **Customize** to edit deployment details.

    ![](./media/image55.png)

1. **Increase the Token per Minute Rate limit** and then click on Create resource and deploy.

    ![](./media/image56.png)

    ![](./media/image57.png)

## Exercise 5: Create Azure AI Search Service

1. Switch back to the Azure portal tab. Enter **AI search** in the
    search bar and select AI Search.

    ![](./media/image58.png)

1. Click on Create.

    ![](./media/image59.png)

1. Enter the details below and click **Review + Create**

    - Resource Group: **AIDeveloper**

    - Name: **azsearchrag<inject key="DeploymentID" enableCopy="false"/>**

    - Region: **<inject key="Region" enableCopy="false"></inject>**

    - Pricing tier: **Basic**.

      ![A screenshot of a computer Description automatically generated](./media/latest-2.png)

      ![A screenshot of a computer Description automatically generated](./media/latest-1.png)

1. Review the details and click on Create.

    ![](./media/image61.png)

1. Wait for the deployment to complete and click on **Go to resource**.

    ![](./media/image62.png)

    ![](./media/image63.png)

## Exercise 6: Create Search Index

1. Switch back to the AI Service tab and click on **Import data**

    ![](./media/image64.png)

1. Select the **Azure SQL Database** tile.

    ![](./media/image65.png)

1. Select the **RAG** scenario.

    ![](./media/image66.png)

1. Enter below details and click **Next**.

    - Subscription: **Default Subscription**

    - Azure SQL account type: **SQL database**

    - Server: **azdbsqlserver<inject key="DeploymentID" enableCopy="false"/>**

    - Database: **ContosoMedicalResearch**

    - Table or View: **Table**

    - Schema: **dbo**

    - Table name: **MedicalResearch**

    - SQL server authentication password: **Pa55w0rd12345**

      ![](./media/image67.png)

1. In Vectorize your text, enter the details below and click Next.

    - Column to vectorize: **Summary**

    - Kind: Azure OpenAI

    - Subscription: **Default Subscription**

    - Azure OpenAI Service: **azsqlaoai<inject key="DeploymentID" enableCopy="false"/>**

    - Model deployment : text-embedding-3-small

    - Authentication type : API key

    - Acknowledge the service

      ![](./media/image69.png)

1. Keep all default values and click Next.

	![](./media/image71.png)

1. Review the details and click Create.

	![](./media/image72.png)

1. Click **Go to Search Explorer**.

	![](./media/image73.png)

1. Enter the prompts, check vectors and embeddings and verify vectors and score differences.
    ```
    Recent advances in oncology treatment
    ```
    ```
    Immunotherapy in lung cancer
    ```
    ```
    AI diagnostics in cardiology
    ```
    
    ![](./media/image74.png)

    ![](./media/image75.png)

    ![](./media/image76.png)

## Exercise 7: Build RAG in Azure AI Foundry

1. Open a new tab and go to **https://ai.azure.com** and sign in with
    your Azure subscription account.

1. Click on Start building to navigate to the new Microsoft Foundry
    portal.

    ![](./media/image77.png)

1. **Click on the drop-down and select Create a new project.**

    ![](./media/image78.png)

1. Expand Advanced options.

    ![](./media/image79.png)

1. Enter the details below and click Create.

    - Project Name: **Foundryproj-<inject key="DeploymentID" enableCopy="false"/>**

    - Region: **East US 2**

    - Resource group: **AIDeveloper**

      ![](./media/image80.png)

      >**Note:** Click on **"X"** when it says *Welcome to new Microsoft Foundry*.

      ![](./media/image80a.png)

1. Navigate back to the Azure portal and search for Microsoft Foundry and select the **Foundry-<inject key="Deployment ID" enableCopy="false"></inject>** project.

    ![](./media/1.12.1.png)  

1. Select **Access control (IAM) (1)** from the left navigation pane, click on **+ Add (2)**, and select **Add role assignment (3)** to assign permissions.
     
    ![](./media/1.24.png)

1. On the **Add role assignment** page, search for **Azure AI Owner (1)**, select the **Azure AI Owner** role **(2)**, and click **Next (3)** to continue.

    ![](./media/1.25.png)

1. In Members section, choose **User, group, or service principal (1)**, click **+ Select members (2)**, search for the **<inject key="AzureAdUserEmail" enableCopy="true"/> (3)**, select the user from the list **(4)**, and click **Select (5)** to add the member.

    ![](./media/1.26.png)

1. Verify the selected **Azure AI Owner** role and added member, then click **Review + assign** to complete the role assignment.

     ![](./media/1.27.png)   

1. Navigate back to **Microsoft Foundry** page and from the top navigation menu, select **Discover**

    ![](./media/image81.png)

1. Click on **Models** from the left navigation menu, search for **gpt-5.2-chat** and select it.

    ![](./media/image82.png)

1. Select **Deploy -\> Default settings**.

    ![](./media/image83.png)

1. Click on Knowledge from the left navigation menu.

    ![](./media/image84.png)

1. Select your AI Search service and API key, then click on Connect.

    ![](./media/image85.png)

1. Click on Create a Knowledge base.

    ![](./media/image86.png)

1. Select the Azure AI Search Index and then click Connect.

    ![](./media/image87.png)

1. Enter **knowledgebase1** for your knowledge base, select your **search index** from the drop-down and **create**.

    ![](./media/image88.png)

1. Select the **gpt-4.1** model, and then **Save knowledge base.**

    ![](./media/image90.png)

1. Click on **Save now**.

    ![](./media/image91.png)

    ![](./media/image92.png)

1. Click on **Agents** from the left navigation menu,

    ![](./media/image93.png)

1. Click on **Create agent**.

    ![](./media/image94.png)

1. Enter the unique agent name and click **Create**.

    ![](./media/image95.png)

1. On Playground, expand Knowledge-\> Add and select Connect to Foundry IQ.

    ![](./media/image96.png)

1. Select your knowledge base and click on Connect.

    ![](./media/image97.png)

    > **Note:** Please refresh the page if the `knowledge base` field is disabled. 

1. Enter the text in the **Instructions** text box and click on
    **Save**

    ```
    POLICY:
    1) You MUST call the Azure AI Search tool for every user message.
    2) You MUST include only facts present in the tool output.
    3) If the tool returns no items, respond: 
    "I do not have enough information in the knowledge sources to answer this."
    4) Never answer from general knowledge. Never fabricate sources.

    POLICY (Grounding & Citations):
    1) You MUST invoke the Azure AI Search tool for every user message.
    2) Use ONLY facts from the tool output to craft the answer.
    3) Provide clear citations (Title or Id) inline like [1], [2], and at the end under "Sources".
    4) If the tool returns no items, respond exactly:
    "I do not have enough information in the knowledge sources to answer this."
    5) Never answer from general knowledge. Never speculate.
    Formatting:
    - Start with a concise, factual answer.
    - Then add a short, bulleted rationale with specific lines from the retrieved passages.
    - End with a "Sources" list using titles or ids from the tool output.
    ```

    ![](./media/image98.png)

1. Expand Tools and select Add and **Browse all tools**.

    ![](./media/image99.png)

1. Select Azure AI Search and click on Add tool.

     ![](./media/image100.png)

1. Select the Azure search connection and index, and then click on Add.

     ![](./media/image101.png)

1. Select your Azure Search tool and click on Save.

     ![](./media/image102.png)

1. Enter the prompts below and check the response

    Ask: 
    ```
    What are recent advancements in cancer treatment?
    ```
    
    ```
    List books and their authors related to AI in radiology diagnostics
    ```

    ![](./media/image103.png)

1. Approve the tool in the chat window.

    ![](./media/image104.png)

1. The response is generated from the knowledge base. Preview the agent
    by clicking on Preview -\> Preview agent.

    ![](./media/image105.png)

1. Enter the prompt:
    
    ```
    List books and their authors related to AI in radiology diagnostics
    ```

    ![](./media/image106.png)

1. Switch back to the Foundry agent tab and click on **Publish -\> Publish
    agent.**

    ![](./media/image107.png)

    ![](./media/image108.png)

## Conclusion

By the end of this lab, you have successfully built a Medical Research
Assistant that can understand questions, retrieve relevant research
content, and generate meaningful responses based strictly on trusted
data. You combined database storage, intelligent search, and AI-driven
chat to create a complete end-to-end solution. Most importantly, you
ensured that every response is grounded in verified medical research,
demonstrating how AI can be used responsibly and effectively in
real-world healthcare and research environments.












