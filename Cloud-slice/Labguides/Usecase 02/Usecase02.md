# Usecase 02- Build Fabric Data Agent using Mirrored Azure SQL Database in Microsoft Fabric

## Introduction

Modern organizations require intelligent systems that can quickly
analyze operational data and provide meaningful insights without complex
data movement. In this use case, Microsoft Fabric is used to mirror data
from an Azure SQL Database into a Fabric environment and create a Fabric
Data Agent that can query and analyze the mirrored data.

The process begins with creating an Azure SQL Database containing sample
business data. This database is then mirrored into Microsoft Fabric
using Azure SQL Mirroring, enabling near real-time access to operational
data within the Fabric workspace. Once the mirrored database is
available, a Fabric Data Agent is configured to connect to the data
source and answer natural language queries.

This approach enables users to interact with enterprise data through
intelligent agents, allowing faster insights into product performance,
customer distribution, and sales trends without writing complex SQL
queries.

## Lab Objectives

In this lab, you will complete the following tasks:

- Task 1: Create a single database - Azure SQL Database
- Task 2: Create a Solution to Mirror Data using Azure SQL Mirroring
- Task 3: Create a Data Agent and Connect the Mirrored Database
- Task 4: Test the agent and Validate the agent responses using sample analytical questions.

## Task 1: Create a single database - Azure SQL Database

In this task, you will create a fully configured Azure SQL Database with
sample data. You will deploy the AdventureWorksLT sample schema, verify
tables, and prepare your server connection details for later mirroring
in Fabric.

1. Open a web browser and navigate to the following URL:

    ```
    https://portal.azure.com
    ```
2. Log in using the credentials provided below.

    - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

        ![Enter Your Username](../masterdoc/media/odlusr.png)

     - **Password:** <inject key="AzureAdUserPassword"></inject>

        ![Enter Your Password](../masterdoc/media/password.png)

3. Search for **Azure SQL Database (1)** and select it from the results **Azure SQL Database (2)**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/azure-sql-database.png)

3. Click on **+ Create**

    ![](./media/image7.png)

4. On **Create a storage account** window, under the **Basics** tab, enter the below details to create a storage account and then click on **Next: Networking**

    | Setting | Value |
    |--------|-------|
    | **Subscription** | Leave as default |
    | **Resource group** | Select **labvm rg** (1) |
    | **Database name** | sqldatabase-<inject key="DeploymentID" enableCopy="false"></inject> (2) |
    | **Server** | Select **Create new** (3) |
    | **Server name** | sqlserver<inject key="DeploymentID" enableCopy="false"></inject> (4) |
    | **Location** | <inject key="Region" enableCopy="false" ></inject>(5) |
    |**Authentication method**| Use SQL Authentication (6) |
    | **Server admin login** | sqladmin (7) |
    | **Password** | `password321!` (8) |
    | **Confirm password** | `password321!` (9) |
    | **Action** | Click **OK** (10) |

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

5. In the Compute + Storage section, click on **Configure database (1)**.

    ![](./media/image10.png)

6. For Service tier from the dropdown select **Standard(Budget Friendly) (1)** and for **DTU enter 100 (2)** and click **Apply (3)**

    ![](./media/image11.png)

    ![](./media/image12.png)

7. On the **Networking** tab, select **Public endpoint (1)**, set **Allow Azure services and resources** to **Yes** **(2)**, enable **Add current client IP address**, and then click **Next: Security\> (3)**

    ![](./media/image13.png)

8. On the **Security** page, after reviewing, select **Next : Additional settings**

    ![](./media/image14.png)

9.  On the **Additional settings** tab, select **Sample (1)** under **Use existing data**, choose **AdventureWorksLT (2)** when prompted, click **OK**, and then select **Review + create (3)** to proceed.

    ![](./media/image15.png)

10. On the **Review + create** page, after reviewing, select **Create**

    ![](./media/image16.png)

    ![](./media/image17.png)

11. On **Microsoft.SQLDatabase** window, after the deployment is completed, click on the **Go to resource** button.

    ![](./media/image18.png)

12. In SQL database page select **Query editor**.

    ![](./media/image19.png)

13. In the **Query editor (preview) (1)**, switch to the **Classic experience (2)**.

     ![](../Usecase%2002/media/classic-experience.png)

14. Enter the **SQL Server login** as 

    - **Username**: 

        ```
        sqladmin 
        ```

    - **Password**:
    
        ```
        password321!
        ```

        ![](../Usecase%2002/media/image20.png) 

14. Make sure all the sample tables have been successfully deployed.

    ![](./media/image21.png)

15. Go back to your SQL Database. Copy **Server name** (1) and **SQL Database name** (2), paste them in a notepad, and then **Save** the notepad to use the information in the upcoming task.

    ![](./media/image22.png)

1. Click **Home** to return to the main page

    ![](./media/image23-replace.png)

2. Choose on **Resource groups**.Click on the **lab-vm** resource group. 

    ![](./media/resource-groups.png)

    ![](./media/lab-vm.png)

4. Select **SQL server**.

    ![](./media/sqlserver.png)

5. Navigate to **Identity(1)** switch the System assigned managed identity status to **On (2)**, and then click **Save (3)** to apply the change.

    ![](./media/image27.png)

    ![](./media/image28.png)

## Task 2: Create a Solution to Mirror Data using Azure SQL Mirroring

In this task, you will connect the Azure SQL Database to Microsoft
Fabric using Azure SQL Mirroring. You will select tables, create the
mirrored database, and validate that the data has synced successfully.

1. Open your web browser, go to the address bar, and enter the following URL:

    ```
    https://app.fabric.microsoft.com/
    ```

2. Sign in using below credentials.

    - **Email/Username**:<inject key="AzureAdUserEmail"></inject>

    - **Password/TAP**:<inject key="AzureAdUserPassword"></inject>

3. In the navigation bar, click on the **+ New item** button within the workspace **Fabric Data agent-<inject key="DeploymentID" enableCopy="false"></inject>**.

    ![](../Usecase%2001/media/image15.png)

1. In the **Filter by keyword** search box, enter **Mirrored Azure SQL Database (1)** and select the **Mirrored Azure SQL Database (2)**
item.

    ![](./media/sql-database-1.png)

2. In the **Choose a database connection to get started** window, select **Azure SQL database**

    ![](./media/image36.png)

3. In Connection settings tab enter the below detail and click on Connect button

    | Setting   | Value |
    |-----------|-------|
    | **Server**   | Enter the SQL Server URL saved in **Task 2 → Step 15** **(1)** |
    | **Database** | Enter **sqldatabase<inject key="DeploymentID" enableCopy="false"></inject>** **(2)** |
    | **Username** | Enter `sqladmin` **(3)**|
    | **Password** | Enter `password321!` **(4)** |
    |**Connect**| Click on Connect **(5)**

    ![](./media/sqlserver-1.png)

7. In the **Choose data** window, select **Select all (1)** and click on **Connect (2)** button

    ![](./media/image38.png)

8. In the Destination tab, click on **Create mirrored database**

    ![](./media/image39.png)

9. Click **Refresh** to update and view the latest changes.

    ![](./media/image40.png)

    ![](./media/image41.png)

1. In the left-sided navigation menu, navigate and click on **Fabric data agent-<inject key="DeploymentID" enableCopy="false"></inject>**

## Task 3: Create a Data Agent and Connect the Mirrored Database

In this task, you will create a new Fabric Data Agent and configure it to use
the mirrored Azure SQL Database as its data source. This agent will
respond to natural language prompts using the mirrored data.

1. In the **Fabric** home page, select **+New item.**

    ![](../Usecase%2001/media/image15.png)

3. In the **Filter by item type** search box, enter **Data agent (1)** and select the **Data agent (2)**

    ![](./media/data-agent.png)

4. Enter **FabricDataAgent-<inject key="DeploymentID" enableCopy="false"></inject>** **(1)** as the Data agent name and select **Create (2)**.

    ![](./media/data-agent-02.png)

5. Select **Add data source** to configure a new data source.

    ![](./media/image46.png)

6. Select **sqldatabse<inject key="DeploymentID" enableCopy="false"></inject>** Mirrored database resource.

    ![](./media/image47.png)

    ![](./media/image48.png)

## Task 4: Test the agent and Validate the agent responses using sample analytical questions.

In this task, you will test the Data Agent by asking analytical questions like:

- Which product categories generate the highest sales?

- List products with high list price but low sales volume.

- Which cities have the highest number of customers?

This validates the agent’s ability to understand and respond to business
queries.

1. Select the **SalesLT (1)** schema for all tables.

2. In the query panel of your Fabric data agent, **Paste below the question (2)** 

    ```
    Which product categories generate the highest sales?
    ```

3. Click on **Send (3)** icon to view the agent’s response

    ![](./media/image49.png)

    ![](./media/image50.png)

3.  To test the agent, run the application and enter the sample questions to verify the responses.

    ```
    List products with high list price but low sales volume. 
    ```

    ![](./media/image51.png)

    ![](./media/image52.png)
  
    ```
     List the cities with the highest number of customers
    ```

    ![](./media/image53.png)

    ![](./media/image54.png)

4.  Click **Agent instructions** from top menu.

    ![](./media/image55.png)

5.  Click Publish from the top menu and select **Publish**.

    ![](./media/image56.png)

    ![](./media/publish.png)

    ![](./media/image58.png)

6.  Now, click on **Fabric Data agent-<inject key="DeploymentID" enableCopy="false"></inject>** on the left-sided navigation pane.

    ![](./media/workspace-fabric.png)

## Summary

In this lab, you successfully created an Azure SQL Database and mirrored
its data into Microsoft Fabric using Azure SQL Mirroring. You then
configured a Fabric Data Agent to connect to the mirrored database and
analyze the data through natural language queries.

The agent was able to answer analytical questions such as identifying
high-selling product categories, products with high prices but low
sales, and cities with the largest number of customers. This
demonstrates how Microsoft Fabric can integrate operational data sources
with intelligent agents to simplify data exploration and enable faster
business insights.

This use case highlights the power of combining **data mirroring and AI-powered data agents** to create interactive and intelligent data
experiences within the Microsoft Fabric ecosystem.

### You have successfully completed the lab.