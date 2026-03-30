# Lab 3: Building and Securing a Safe Clinical Report Search API

In this lab, participants step into a real-world healthcare scenario
where a hospital needs a secure and intelligent way to search clinical
discharge summaries using natural language. Using Microsoft SQL Server
2025, SQL Server Management Studio Copilot, Azure OpenAI Service, and
Data API Builder, learners will build a secure semantic search API that
enables staff to retrieve relevant clinical reports while ensuring
patient data privacy through masking, RBAC, and managed identity. This
hands-on experience demonstrates how modern AI capabilities can be
integrated directly into SQL Server to create enterprise-ready, secure
healthcare solutions.

## Objectives:

By the end of this lab, participants will be able to:

- Enable AI and vector search in Microsoft SQL Server 2025

- Generate embeddings using Azure OpenAI Service

- Implement semantic search with vector indexing

- Secure clinical data using masking, RBAC, and Managed Identity

- Expose search functionality as a REST API using Data API Builder

- Accelerate development with SQL Server Management Studio Copilot

## Exercise 1: Provision SQL Server on Azure VM

1. On your LabVM, click on the **Azure Portal** icon.

    ![Image](../Lab%201/media/94.png)

1. On the **Sign in to Microsoft Azure** tab, you will see the login screen. Enter the following email/username, and click on **Next (2)**. 

   - **Email/Username**: <inject key="AzureAdUserEmail"></inject> **(1)**
   
      ![Image](../Lab%201/media/95.png)
     
1. Now enter the following Temparory Access Pass and click on **Sign in (2)**.
   
   - **Temporaray Access Pass**: <inject key="AzureAdUserPassword"></inject> **(1)**

      ![Image](../Lab%201/media/96.png)
     
1. If you see the pop-up **Stay Signed in?**, select **No**.

   ![Image](../Lab%201/media/97.png)

1. In the Azure portal, type **Azure SQL (1)** in the top search bar and select **Azure SQL (2)** from the Services list.

    ![](./media/new0.png)

1. In the Azure portal, expand **SQL Server (1)**, select **SQL Server on Azure VMs (2)**, and then click **+ Create (3)** to create a new **SQL Server Virtual Machine (4)**.

    ![](./media/new1.png)

1. In the **Select an image offer** dropdown **(1)**, choose **Free SQL Server License: SQL Server 2025 Enterprise Developer on Windows Server 2025 (2)**.

    ![](./media/new2.png)

1. After selecting the image offer **(3)**, click **Create virtual machine (4)** to proceed with the deployment.

    ![](./media/new3.png)

1. On the **Basics** tab, provide the following details to configure the virtual machine:

    - **Subscription**: Select the available Azure subscription **(1)**
    - **Resource group**: Choose an existing resource group **AIDeveloper (2)**
    - **Virtual machine name**: Enter a name **sqlvm-<inject key="Deployment ID" enableCopy="false"/> (3)**
    - **Region**: Select **Central US (4)**
    - **Availability options**: Select **Availability zone (5)**
    - **Zone options**: Choose **Self-selected zone (6)**
    - **Availability zone**: Select **Zone 1 (7)**

      ![](./media/new4.png)

1. Set the **Security type** to **Standard (1)**, ensure the correct **SQL Server 2025 Enterprise Developer image (2)** is selected, and then click **See all sizes (3)** to choose an appropriate VM size.

    ![](./media/new5.png)

1. In the **Select a VM size** window, search for **E4ds_v5 (1)**, expand **E-Series v5 (2)**, select **E4ds_v5 (3)**, and then click **Select (4)**.

    ![](./media/new6.png)

1. On the **Basics** tab, configure the following settings:

    - **Size**: Verify the selected size (**Standard_E4ds_v5 - 4 vCPUs, 32 GiB memory**) **(1)**
    - **Username**: Enter **sqlvmuser (2)**
    - **Password**: Enter **AZvmsql12345 (3)**
    - **Confirm password**: Enter **AZvmsql12345 (4)**
    - **Public inbound ports**: Select **Allow selected ports (5)**
    - **Select inbound ports**: Choose **RDP (3389) (6)**
    - Then, click **Next: Disk > (7)**.

      ![](./media/new7.png)

1. Keep the default disk settings and click **Next: Networking >**.

    ![](./media/new8.png)

1. Keep the default networking settings, ensure **Allow selected ports** with **RDP (3389)** is selected, and then click **Next: Management**.

    ![](./media/new9.png)

1. On the **Management** tab, enable **System assigned managed identity (1)** and **Enable periodic assessment (2)**, then click **Next: Monitoring > (3)**.

    ![](./media/new10.png)

1. Keep the default monitoring settings and click **Next: Advanced >**.

    ![](./media/new11.png)

1. On the **SQL Server settings** tab, set **SQL connectivity** to **Public (Internet) (1)**, enable **SQL Authentication (2)**, and then click **Review + create (3)**.

    ![](./media/new12.png)

1. Review the configuration details, ensure validation is passed, and then click **Create** to deploy the virtual machine.

    ![](./media/new13.png)

1. Once the deployment is complete, click **Go to resource** to access the created virtual machine.

    ![](./media/new14.png)

1. On the virtual machine **Overview** page, copy the **Public IP address** and paste in **Notepad** to use for connecting to the SSMS in the next steps.

    ![](./media/new15.png)

## Exercise 2: Create Azure OpenAI resource and deploy embedding models

1. In the Azure portal, type **Azure OpenAI (1)** in the top search bar and select **Azure OpenAI (2)** from the Services list.

    ![](./media/latest3.png)

1. In the **Azure OpenAI** page, click **+ Create (1)** and select **Azure OpenAI (2)** from the dropdown to create a new Azure OpenAI resource, which will be used to deploy models and generate embeddings in the lab.

    ![](./media/new16.png)

1. On the **Basics** tab, provide the following details to create the Azure OpenAI resource for deploying models used in the lab:

    - **Subscription**: Select your Azure subscription **(1)**

    - **Resource group**: Choose an existing resource group **AIDeveloper (2)**

    - **Region**: Select **East US (3)**

    - **Name**: Enter **azsqlaoai-<inject key="Deployment ID" enableCopy="false"/> (4)**

    - **Pricing tier**: Select **Standard S0 (5)**

    - Click **Next (6)** to proceed.

      ![](./media/new17.png)

1. On the **Review + submit** tab, verify the provided details and click **Create** to deploy the Azure OpenAI resource.

    ![](./media/new18.png)

1. Once the deployment is complete, click **Go to resource** to open the Azure OpenAI resource.

    ![](./media/new19.png)

1. In the Azure OpenAI resource, navigate to **Keys and Endpoint (1)** from the left navigation pane, copy the **Key1 (2)** and **Endpoint (3)** values and paste them in **Notepad** to use in the next steps for authentication and integration.

    ![](./media/new20.png)

1. From the Azure OpenAI resource **Overview (1)** page, click **Go to Foundry portal (2)** to access model deployments and manage AI models required for the lab.

    ![](./media/new21.png)

1. In the Foundry portal, navigate to **Deployments (1)**, click **+ Deploy model (2)**, and select **Deploy base model (3)** to deploy a model for use in the lab.

    ![](./media/new22.png)

1. In the **Select a model** window, search for **text-embedding-3-small (1)**, select it **(2)**, and click **Confirm (3)** to use it for generating embeddings in the lab.

    ![](./media/new23.png)

1. In the **Deploy text-embedding-3-small** window, click **Customize** to adjust limits.

    ![](./media/new24.png)

1. In the **Deploy text-embedding-3-small** window, set the **Tokens per Minute Rate Limit** to **100K (1)**, keep the default settings, and click **Deploy (2)** to complete the model deployment.

    ![](./media/new25.png)

1. After the model deployment is complete, navigate to **Deployments**, open the deployed model, and copy the **Endpoint (Target URI)** and paste it in the **Notepad** to use in the next steps for integration.

    ![](./media/new26.png)

## Exercise 3: Create Storage account and Store the file

1. In the Azure portal, type **Storage accounts (1)** in the search bar and select **Storage accounts (2)** from the Services list to create and manage storage resources for the lab.

    ![](./media/new27.png)

1. Click **+ Create** to initiate the creation of a new storage resource.   

    ![](./media/latest4.png)

1. On the **Basics** tab, provide the following details to create the storage account for storing lab data:

    - **Subscription**: Select your Azure subscription **(1)**
    - **Resource group**: Choose an existing resource group **AIDeveloper (2)**
    - **Storage account name**: Enter **storage<inject key="Deployment ID" enableCopy="false"/> (3)**
    - **Region**: Select **(US) East US (4)**
    - **Preferred storage type**: Select **Azure Blob Storage or Azure Data Lake Storage Gen2 (5)**
    - **Performance**: Select **Standard (6)**
    - **Redundancy**: Select **Locally-redundant storage (LRS) (7)**
    - Click **Review + create (8)**, then proceed to create the storage account.

      ![](./media/new28.png)

1. On the **Review + create** tab, verify the configuration details and click **Create** to deploy the storage account.

    ![](./media/new29.png)

1. Once the deployment is complete, click **Go to resource** to open the storage account.

    ![](./media/new30.png)

1. In the storage account, navigate to **Containers (1)** under **Data storage** and click **+ Add container (2)** to create a new container for storing lab files.

    ![](./media/new31.png)

1. In the **New container** pane, enter the container name as **public (1)** and click **Create (2)** to create the container for storing lab files.

    ![](./media/new32.png)

1. In the **public** container, click **Upload (1)**, then select **Browse for files (2)** to upload the *clinical_reports.csv* file.

    ![](./media/new33.png)

1. In the file explorer, navigate to **C:\LabFiles (1)**, select the **clinical_reports.csv (2)** file, and click **Open (3)** to upload it to the container.

    ![](./media/new34.png)

1. In the **Upload blob** pane, verify the selected file and click **Upload** to upload the file to the container.

    ![](./media/new35.png)

1. In the public container, navigate to **Shared access tokens (1)** under settings, and click **Generate SAS token and URL (2)** to create a SAS token for accessing the uploaded file.

    ![](./media/new36.png)

1. After generating the SAS token, copy the **Blob SAS token** value and save it, as it will be used later to access the storage data.

    ![](./media/new37.png)

## Exercise 4: Connect SQL server 2025 via SSMS 

1. In the LabVM search bar, type **SSMS (1)** and select **SQL Server Management Studio 22 (2)** to open the application.

    ![](./media/new38.png)

1. In the **Sign in to SQL Server Management Studio** window, click **Sign in with Microsoft** to continue.

    ![](./media/new39.png)

1. Select **Work or school account (1)** and click **Continue (2)** to sign in using your assigned credentials.

    ![](./media/new40.png)

1. On **Sign in** page, enter the following email/username, and click on **Next (2)**. 

   * **Email/Username**: <inject key="AzureAdUserEmail"></inject> **(1)**
   
      ![Image](../Lab%201/media/95.png)
     
1. Now, enter the following Temparory Access Pass and click on **Sign in (2)**.
   
   * **Temporaray Access Pass**: <inject key="AzureAdUserPassword"></inject> **(1)**

      ![Image](../Lab%201/media/96.png)   

      > **Note:** I may ask you to select the user **<inject key="AzureAdUserEmail"></inject>**.

1. If the sign-in prompts, click **Yes** to enable sign-in to all apps and websites on this device.

    ![Image](../Lab%201/media/latest1.png)

1. If the confirmation screen prompts, click **Done** to complete the account setup and start accessing your organization’s apps and services.  

    ![Image](../Lab%201/media/latest2.png)

1. In the **SSMS** **Connect** window, provide the following details to connect to the SQL Server:

    - **Server name**: Enter the **Public IP address with port 1433** that you copied in Exercise 1 **(1)**
    - **Authentication**: Select **SQL Server Authentication (2)**
    - **User name**: Enter **sqlvmuser (2)**
    - **Password**: Enter the **AZvmsql12345 (3)**
    - **Trust Server Certificate**: Check this option **(4)**
    - Click **Connect (5)** to access the SQL Server.

        ![](./media/new41.png)

## Exercise 5: Enable SQL Server 2025 AI Capabilities

1. In SSMS, click **New Query (1)** to open a query window, then enter the following SQL commands **(2)** to create a new database and set it as active:

    ```
    CREATE DATABASE ContosoClinicalReports;
    GO
    USE ContosoClinicalReports;
    GO
    ```

1. Click **Execute (3)** to run the query and verify that the message **Commands completed successfully** **(4)** appears, confirming the database creation

    ![A screenshot of a computer Description automatically
    generated](./media/Q1.png)

    >**Note:** For each query below, please select **New Query** in the tool bar.

2. Run below query to enable outbound REST.

    ```
    USE master;
    GO
    sp_configure 'external rest endpoint enabled', 1;
    GO
    RECONFIGURE WITH OVERRIDE;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image52.png)

3. Run below query to **enable preview features in database.** This enables VECTOR type , VECTOR INDEX , AI_GENERATE_EMBEDDINGS and VECTOR_SEARCH .

    ```
    USE ContosoClinicalReports;
    GO
    ALTER DATABASE SCOPED CONFIGURATION  
    SET PREVIEW_FEATURES = ON;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image53.png)

4. Run below query to create ClinicalReports table

    ```
    CREATE TABLE dbo.ClinicalReports
    (
        ReportID      INT           NOT NULL PRIMARY KEY,
        AgeGroup      NVARCHAR(50)  NULL,
        Department    NVARCHAR(100) NULL,
        ReportDate    DATE          NULL,
        ReportText    NVARCHAR(MAX) NULL,   -- discharge summary / note text
        PatientMRN    NVARCHAR(20)  NULL,   -- will be masked later
        Diagnosis     NVARCHAR(500) NULL
    );
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image54.png)

5. Run below query to **Create Master Key.**

    >**Note:** If you have used different password then replace the password in the query below.

    ```
    USE ContosoClinicalReports;
    GO
    IF NOT EXISTS (SELECT 1 FROM sys.symmetric_keys WHERE name = '##MS_DatabaseMasterKey##')
    BEGIN
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'AZvmsql12345';
    END
    GO 
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image55.png)

6. Run below query to create Database Scoped Credential.

    >**Note**: Replace CREDENTIAL value with OpenAI Endpoint and SECRET value
    OpenAI Key that you copied and pasted in the Notepad in Exercise 2.

    ```
    CREATE DATABASE SCOPED CREDENTIAL [Paste OpenAI Endpoint] 
    WITH 
       IDENTITY = 'HTTPEndpointHeaders', 
       SECRET   = '{"api-key":"Paste OpenAI Key"}' 
    GO 
    ```

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image56.png)

7. Run below query to create External Model.

    >**Note**: Replace Location with text-embedding URL from Microsoft Foundry
    Portal and Credential value with OpenAI Endpoint that you copied and pasted in the Notepad in Exercise 2.

    ```
    CREATE EXTERNAL MODEL ClinicalEmbeddingModel 
    WITH ( 
        -- Full embeddings endpoint URL: host + deployment + path + api-version 
        LOCATION   = '<Replace with text-embedding Target URI>', 
        API_FORMAT = 'Azure OpenAI', 
        MODEL_TYPE = EMBEDDINGS, 
        MODEL      = 'text-embedding-3-small', 
        -- Reference the credential we created above (named by host URL) 
        CREDENTIAL = [Paste OpenAI Endpoint], 
        PARAMETERS = '{ "sql_rest_options": { "retry_count": 10 } }'  
    ); 
    GO 
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image58.png)

8. Run below query to create Credential for Blob Storage.

	>**Note**: Replace the SECRET value with Storage account SAS token that you copied and pasted in Notepad in Exercise 3.

    ```
    -- Create a credential for Blob storage
    CREATE DATABASE SCOPED CREDENTIAL ClinicalReportsBlobCred
    WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
        SECRET = '<Paste the SAS Token>';
    GO
    ```

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image59.png)

9. Run below query to create the external data source credential.

	>**Note**: Replace the Location value with your storage account name.

    ```
    -- Now create the external data source using that credential
    CREATE EXTERNAL DATA SOURCE ClinicalReportsBlob
    WITH (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://<Replace with your storage account name>.blob.core.windows.net/public',
        CREDENTIAL = ClinicalReportsBlobCred
    );
    GO
    ```

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image60.png)

10. Run below query to bulk insert:

    ```
    TRUNCATE TABLE dbo.ClinicalReports;
    GO
    BULK INSERT dbo.ClinicalReports
    FROM 'clinical_reports.csv'
    WITH
    (
        DATA_SOURCE       = 'ClinicalReportsBlob',
        FORMAT            = 'CSV',
        FIRSTROW          = 2,
        FIELDTERMINATOR   = ',',
        ROWTERMINATOR     = '0x0a',
        FIELDQUOTE        = '"',
        CODEPAGE          = '65001',
        TABLOCK
    );
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image61.png)

11. Run below query to verify the table data:

    ```
    SELECT COUNT(*) FROM dbo.ClinicalReports;
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image62.png)

## Exercise 6: Azure OpenAI Integration (SQL Server 2025 Pattern)

1. Run below query to test the embeddings

    ```
    SELECT AI_GENERATE_EMBEDDINGS
    (
    N'Patient with fever and cough' 
    USE MODEL ClinicalEmbeddingModel
    ) AS Emb;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image63.png)

## Exercise 7: Create Embeddings Table

1. Run below query

    ```
    DROP TABLE IF EXISTS dbo.ReportEmbeddings;
    GO

    CREATE TABLE dbo.ReportEmbeddings
    (
        ReportID INT PRIMARY KEY,
        Embedding VECTOR(1536)
    );
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image64.png)

2. Run below query

    ```
    INSERT INTO dbo.ReportEmbeddings (ReportID, Embedding)
    SELECT ReportID,
        AI_GENERATE_EMBEDDINGS (ReportText USE MODEL ClinicalEmbeddingModel)
    FROM dbo.ClinicalReports;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image65.png)

    >**Note**: As the number of rows are too much, session can be interrupted
    sometimes. In this case, you can load the embeddings in two batches.
    First load 100 rows and then again 100 rows with the help of below
    query: (Run the below query twice to load 200 records)

    ```
    INSERT INTO dbo.ReportEmbeddings (ReportID, Embedding)
    SELECT TOP (100) ReportID,
        AI_GENERATE_EMBEDDINGS (ReportText USE MODEL ClinicalEmbeddingModel)
    FROM dbo.ClinicalReports
    WHERE ReportID NOT IN (SELECT ReportID FROM dbo.ReportEmbeddings);
    ```

	You can check how many rows are processed:

    ```
    SELECT COUNT(*) AS ProcessedReports
    FROM dbo.ReportEmbeddings;
    ```

    ![A screenshot of a computer Description automatically
    generated](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image66.png)

## Exercise 8: Create Vector Index (DiskANN) and Exact vs ANN Search

1. Run below query to create vector index

    ```
    CREATE VECTOR INDEX IX_ReportEmbeddings
    ON dbo.ReportEmbeddings (Embedding)
    WITH (METRIC = 'cosine', TYPE = 'diskann');
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image67.png)

2. Run below query for exact search

    ```
    DECLARE @query NVARCHAR(MAX) =
    N'post-operative wound infection';

    DECLARE @qvec VECTOR(1536);
    SELECT @qvec = AI_GENERATE_EMBEDDINGS(@query USE MODEL ClinicalEmbeddingModel);
    SELECT TOP 5
        r.ReportID,
        r.Department,
        VECTOR_DISTANCE('cosine', @qvec, e.Embedding) AS Distance
    FROM dbo.ReportEmbeddings e
    JOIN dbo.ClinicalReports r ON r.ReportID = e.ReportID
    ORDER BY Distance ASC;
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image68.png)

3. Run the query to ANN Search (DiskANN)

    ```
    USE ContosoClinicalReports;   -- use your exact DB name
    GO

    -- 1) Doctor query
    DECLARE @prompt NVARCHAR(MAX) =
    N'post-operative wound infection with fever and pain';

    -- 2) Create the query embedding
    DECLARE @qvec VECTOR(1536);
    SELECT @qvec = AI_GENERATE_EMBEDDINGS(@prompt USE MODEL ClinicalEmbeddingModel);

    -- 3) ANN search using VECTOR_SEARCH (DiskANN index recommended)
    SELECT TOP (5)
        r.ReportID,
        r.Department,
        r.ReportDate,
        LEFT(r.ReportText, 250) AS ReportPreview,
        s.distance AS CosineDistance,
        CAST(1.0 - s.distance AS DECIMAL(19,6)) AS Similarity
    FROM VECTOR_SEARCH(
            TABLE      = dbo.ReportEmbeddings AS t,
            COLUMN     = Embedding,
            SIMILAR_TO = @qvec,
            METRIC     = 'cosine',
            TOP_N      = 5
        ) AS s
    JOIN dbo.ReportEmbeddings e
    ON t.ReportID = e.ReportID
    JOIN dbo.ClinicalReports r
    ON r.ReportID = e.ReportID
    ORDER BY s.distance ASC;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image69.png)

4. Run below query Measure Recall (Exact vs ANN)

    ```
    CREATE OR ALTER PROCEDURE dbo.MeasureRecall
    @query NVARCHAR(MAX),
    @top INT = 5
    AS
    BEGIN
        DECLARE @qvec VECTOR(1536);
        SELECT @qvec = AI_GENERATE_EMBEDDINGS(@query USE MODEL ClinicalEmbeddingModel);
        WITH exact AS (
            SELECT TOP (@top) ReportID
            FROM dbo.ReportEmbeddings
            ORDER BY VECTOR_DISTANCE('cosine', @qvec, Embedding)
        ),
        ann AS (
            SELECT TOP (@top) t.ReportID
            FROM VECTOR_SEARCH(
                TABLE = dbo.ReportEmbeddings AS t,
                COLUMN = Embedding,
                SIMILAR_TO = @qvec,
                METRIC = 'cosine',
                TOP_N = @top
            ) AS s
        )
        SELECT COUNT(*) * 1.0 / @top AS Recall
        FROM exact
        WHERE ReportID IN (SELECT ReportID FROM ann);
    END
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image70.png)

## Exercise 9: Apply Masking + RBAC

1. Run the query

    ```
    ALTER TABLE dbo.ClinicalReports
    ALTER COLUMN PatientMRN NVARCHAR(20)
    MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');
    GO
    CREATE ROLE HospitalStaffRole;
    GRANT SELECT ON dbo.ClinicalReports TO HospitalStaffRole;
    GO
    ```

    ![](https://raw.githubusercontent.com/technofocus-pte/sqlaidevlprdepth/refs/heads/main/Lab%20Guides/Lab%203/media/image71.png)

## Exercise 10: Build the Core Search Stored Procedure with SSMS Copilot 

**Goal**: Create the semantic search logic — use free SSMS Copilot to
speed up coding.

1. In SSMS, click the **Copilot Chat icon** in the top-right corner to open the GitHub Copilot Chat panel.

    ![](./media/new42.png)

1. Click **Continue with GitHub** to sign in and activate GitHub Copilot.

    ![](./media/new-11.png)

    >**Note:** Use your personal GitHub credentials to sign in the GitHub Copilot.

2. New query window → open Copilot chat pane or type /copilot

3. Ask Copilot:

    ```
    - An EXTERNAL MODEL named ClinicalEmbeddingModel is already created.
    - You MUST use the following syntax for embeddings: 
    AI_GENERATE_EMBEDDINGS(input USE MODEL ClinicalEmbeddingModel)
    - DO NOT use comma-based syntax like AI_GENERATE_EMBEDDINGS(input, 'model_name').
    Assume the following tables exist:
    Table: dbo.ClinicalReports
    Columns:
    - ReportID INT PRIMARY KEY
    - PatientMRN INT
    - ReportText NVARCHAR(MAX) 
    
    Table: dbo.ReportEmbeddings
    Columns:
    - ReportID INT PRIMARY KEY
    - Embedding VECTOR(1536) 
    
    Task:
    Generate a SQL stored procedure named dbo.SearchClinicalReportsSemantic that performs semantic search using vector similarity. 
    
    Requirements:
    1. Accept input parameter: 
    @query NVARCHAR(MAX)
    2. Generate query embedding using: 
    AI_GENERATE_EMBEDDINGS(@query USE MODEL ClinicalEmbeddingModel)
    3. Perform similarity search using: 
    VECTOR_DISTANCE('cosine', query_embedding, Embedding)
    4. Join dbo.ReportEmbeddings with dbo.ClinicalReports using ReportID
    5. Return TOP 5 most similar results with: 
       - ReportID 
       - PatientMRN 
       - ReportText (or preview using LEFT) 
       - SimilarityScore = (1 - cosine distance)
    6. Order results by similarity (highest first)
    7. Handle NULL embeddings safely
    8. Use proper SQL Server 2025 syntax only (no unsupported syntax)
    9. Include TRY-CATCH error handling
    10. Add comments explaining each step 
    
    Output:
    Only provide the final stored procedure. Do not include explanations. 
    ```

    ![](./media/latest.png)

4. Open a **New Query** and click **Apply** on the Copilot response, and then run the query.

    ![](./media/latest1.png)

5. Once Query is add click **Execute** to run the query.

    ![](./media/latest2.png)


## Conclusion:

By completing this lab, participants have successfully provisioned infrastructure, enabled SQL Server 2025 AI features, generated vector embeddings using Azure OpenAI, implemented semantic search with DiskANN indexing, and secured sensitive patient data using masking and role-based access control. They also exposed the search functionality as a secure REST API for hospital applications. Overall, learners gained practical experience in building an end-to-end AI-powered, privacy-compliant clinical search system that combines database intelligence, cloud AI services, and secure API development into one integrated solution.
