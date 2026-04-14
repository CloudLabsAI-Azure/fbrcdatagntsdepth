# Usecase 04-Integrate Fabric Data Agent with Microsoft Teams for actionable insights and agent-to-agent collaboration using Copilot Studio

## Introduction

In today’s competitive digital marketplace, e-commerce companies
generate large volumes of data from customer transactions, product
catalogues, website interactions, and payment systems. Extracting
meaningful insights from this data is essential for improving customer
experience, optimizing operations, and increasing revenue. However,
managing and analyzing large datasets from multiple sources can be
complex without a unified analytics platform.

**Zava**, a rapidly growing e-commerce company, sells various consumer
products through its online platform and processes thousands of orders
every day. The company collects data from different systems including
order management, customer profiles, product inventory, and payment
transactions. As Zava’s business grows, the organization faces
challenges in analyzing this data efficiently and providing real-time
insights to business teams.

To address these challenges, Zava implements a modern analytics solution
using **Microsoft Fabric**. Fabric provides a unified platform that
integrates data engineering, data storage, data transformation, and
business intelligence capabilities within a single environment. Zava
stores its raw and processed e-commerce data in the Fabric Lakehouse,
enabling scalable data management and analytics.

In addition, Zava leverages **Microsoft Fabric Data Agents** to enhance
data accessibility and insights generation. Fabric Agents allow business
users and analysts to interact with enterprise data using natural
language queries. Instead of manually searching through reports or
writing complex queries, users can simply ask questions such as:

- “What are the top selling products this month?”

- “Which region generated the highest sales revenue?”

- “What is the customer order trend over the last quarter?”

The Fabric Agent automatically retrieves relevant data from the
Lakehouse and generates insights, helping teams quickly understand
business performance. This intelligent interaction improves productivity
and enables faster decision-making across departments.

With this solution, Zava’s business users, analysts, and management
teams can easily explore data, monitor key performance indicators, and
gain real-time insights into sales performance, customer behaviour, and
product demand. By combining advanced analytics with AI-powered Fabric
Agents, Zava builds a scalable and intelligent e-commerce analytics
platform that supports data-driven growth and operational excellence.

## Lab Objectives

In this lab, you will complete the following tasks:

- Exercise 1: Creating and Configuring the Fabric Data Agent
    - Task 1: Create a lakehouse and Ingest sample data
    - Task 2: Create a Fabric data agent
    - Task 3: Optimizing with Meta-Prompts
    - Task 5: Publishing the Agent
- Exercise 2: Connecting Fabric Agent to Copilot Studio
    - Task 1:Creating the Copilot Studio Agent
    - Task 2: Adding the Fabric Agent as a connected agent for Copilot Studio
    - Task 3: Testing the connected Fabric Data Agent
 - Exercise 3: Connect the Fabric Data Agent to Teams
    - Task 1: Add Copilot Capabilities
    - Task 2: Testing the connected Fabric Data Agent

# Exercise 1: Creating and Configuring the Fabric Data Agent

In this exercise, you will establish the foundational components in Microsoft Fabric. You will create a workspace, set up a lakehouse, ingest sample CSV datasets, generate a semantic model, and configure a Fabric Data Agent capable of answering analytical questions. This provides the core data intelligence layer used throughout the rest of the lab.

## Task 1: Create a lakehouse and Ingest sample data

In this task, you set up a lakehouse and ingest the NYC Taxi sample data
along with additional CSV files. This establishes your raw dataset
foundation inside Fabric, enabling you to start transformations and
queries later.

1. Create a new lakehouse by clicking on the **+ New item** button in the navigation bar.

    ![](./media/new-item.jpg)

2. On the **Filter by item type** search box, enter **Lakehouse** and select the lakehouse item.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./media/image7.png)

3. On the **New lakehouse** dialog box,
 enter **fabricagent_lakehouse** in the **Name** field, click on the **Create** button and open the new lakehouse.

    ![](./media/image8.png)
    
    ![](./media/image9.png)

4. Wait for the notification stating **Successfully created SQL endpoint**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./media/image10.png)

5. In the **lakehouse** page, navigate to **Get data in yourlakehouse** section, and click on **Upload files**

    ![](./media/image11.png)

6. On the Upload files tab, click on the folder under the Files

    ![](./media/image12.png)

7. Browse to **C:\LabFiles\lab file **(1)**** on your VM, then select **customers.csv, Orders_Data.csv** and **products.csv** **(2)** file and click
on **Open (3)** button.

    ![](./media/labfiles.jpg)

8. Then, click on the **Upload** button and close

    ![](./media/image14.png)

    ![](./media/image15.png)

9. Click and select refresh on the **Files**. The file appears.

    ![](./media/image16.png)

10. In the **Lakehouse** page, Under the Explorer pane select Files. Now, however your mouse **Orders_Data.csv** file. Click on the horizontal ellipses **(…)** beside **Orders_Data.csv** . Navigate
and click on **Load Table**, then select **New table**.

    ![](./media/image17.png)

11. In the **Load file to new table** dialog box, click on the **Load** button.

    ![](./media/image18.png)

    ![](./media/image19.png)

12. Repeat the same process for **customers.csv** and **products.csv** to convert them into tables.

    ![](./media/image20.png)

13. Click on **Load** by keeping **New table name** as **customers**

    ![](./media/image21.png)

 14. Click on the horizontal ellipses (…) beside **customers.csv** 

        ![](./media/image22.png)

15. Navigate and click on **Load Table**, then select  New table

    ![](./media/image23.png)

16. Click on **Load** by keeping New table name as **products**

    ![](./media/image24.png)

    ![](./media/image25.png)

13. Select **SQL analytics endpoint** from the **Lakehouse** drop-down menu at the top right of the screen.

    ![](./media/image26.png)

14. From the lakehouse **Home** tab, select **New semantic model** and select the tables that you want to add to the semantic model.

    ![](./media/image27.png)

16. In the **New semantic model** dialog enter  **E-commerce Dataset Report**  and then select the **all** tables from the list of tables and select **Confirm** to create the new model.

    ![](./media/image28.png)

15. From the left menu, select **Fabricagent<inject key="DeploymentID" enableCopy="false"></inject>** workspace icon and then select workspace name.

    ![](./media/image29.png)

## Task 2: Create a Fabric data agent

1. In the **Fabricagent<inject key="DeploymentID" enableCopy="false"></inject>** workspace page, navigate and click on **+New item** button, then select **Data agent**

    ![](./media/image30.png)

2. Click on **Skip for now**

2. Provide a name as **DataAgent_<inject key="DeploymentID" enableCopy="false"></inject>** and click **Create**

    ![](./media/image31.png)

3. Select **Add data source** to configure a new data source.

    ![](./media/image32.png)

4. Select **E-commerce Order Dataset** (Type: Semantic Model) from the results.

    ![](./media/image33.png)

    ![](./media/image34.png)

5. When you first ask the questions with the listed tables select **all tables** the data agent answers them fairly well.

6. For instance, for the question.

    ```
    Who are the top 10 customers by total purchase amount? 
    ```

    ![](./media/image35.png)

7. Run the application and enter the sample questions to verify the responses.

    ```
    Which day has the highest sales?
    ``` 

    ![](./media/image37.png)

## Task 3: Optimizing with Meta-Prompts

1. In the **Setup** section, locate the **Agent instructions** field.(Alternatively, you can also locate the **Agent instructions** in the navigation bar on the top.)

    ![](./media/image38.png)
    
    ![](./media/image39.png)

2. Generate agent-level instructions using this meta-prompt in the **test pane** (on the right where it says **Test the agent's responses**):

    ```
    Meta-Prompt: Generate Agent-Level Instructions:
    Analyze your available data sources and create agent-level instructions for yourself (max 15000 chars).

    Objective: {AGENT_OBJECTIVE}
    Users: {USER_PERSONA}

    Examine your data sources: list all sources, types, and primary use. Analyze domain, time coverage, and main themes.

    Generate instructions with:
    ## Objective
    ## Data Sources (list with priority)
    ## Key Terminology (infer from columns/measures)
    ## Response Guidelines
    Style: {RESPONSE_STYLE}
    ## Handling Common Topics (3-5 based on available data)

    Custom terms: {CUSTOM_TERMINOLOGY}
    ```

3. When using this meta-prompt, replace the variables manually in the
prompt as per the values below **OR** paste these in the Test:

    - {AGENT_OBJECTIVE}: "E-commerce analytics agent for business
    intelligence"

    - {USER_PERSONA}: "Business analysts and sales teams"

    - {RESPONSE_STYLE}: "Clear summaries with data citations and trend
    analysis"

    - {CUSTOM_TERMINOLOGY}: Leave empty or add your domain-specific terms

        ![](./media/image40.png)

8. Test the enhanced agent with more complex queries:

    ```
    How many orders are placed each day? 
    ```

    ![](./media/image42.png)

    ```
    Which products have the lowest stock levels? 
    ```

    ![](./media/image44.png)

## Task 5: Publishing the Agent

1. Generate the agent description using this meta-prompt inside the test pane of your Fabric Agent

    ```
    Meta-Prompt: Generate Agent Description
    Create a 1-2 sentence description of yourself as a Fabric Data Agent (max 200 chars).

    Analyze your data sources and describe: what data domain you cover and what questions you answer.

    Example: "Fabric Data Agent for retail sales. Answers questions about revenue, products, customers, and orders"

    Output plain text only.
    ```

    ![](./media/image45.png)

2. Click **Publish** and paste the generated description in the purpose and capabilities field.

    ![](./media/image46.png)

    ![](./media/image47.png)

# Exercise 2: Connecting Fabric Agent to Copilot Studio

This exercise focuses on enabling Copilot Studio to communicate with the Fabric Data Agent. You will create a Copilot agent, configure its behaviour, link it to the Fabric Agent, and ensure that both agents collaborate to produce richer insights. This establishes multi‑agent communication across platforms.

## Task 1:Creating the Copilot Studio Agent

1. Open a new browser tab and navigate to 
  
    ```
    https://copilotstudio.microsoft.com/
    ```

    ![](./media/get-started.png)

    ![](./media/skip.png)

2. In the left navigation, select **Agents**

    ![](./media/image49.png)

3. Click the blue **+ Create blank** agent button and fill the name as **E-commerce RAG Agent** and click on **create**.

    ![](./media/image50.png)

    ![](./media/E-commercerag.png)

4. Click **Edit** to modify the settings.

    ![](./media/image52.png)

5. Configure your agent with these settings:

    - **Name**: E-commerce RAG Agent

    - **Description**: An agent connected to a Microsoft Fabric data
      agent specializing in e-commerce business knowledge and support

    - Select your agent’s model, select **Claude Sonnet 4.5**

    - **Instructions**: Copy the instructions from the code block below

        ![](./media/image53.png)
    
        ![](./media/image54.png)
    
        ![](./media/image55.png)
    
        ![](./media/image56.png)

6. Click **Publish** in the top-right corner.

    ![](./media/image57.png)
    
    ![](./media/image58.png)

## Task 2: Adding the Fabric Agent as a connected agent for Copilot Studio

1. After agent creation, navigate to the **Agents** tab and click **+Add agent**

    ![](./media/image59.png)

2. Click **Connect to an external agent** and select **Microsoft Fabric (preview)**.

    ![](./media/image60.png)

3. If it say's **Connection : Not connected* then click the drop down next to *Not connected** and select **Create new connection**. Verify it's showing as the email for your account, then click **Next**.

    ![](./media/image61.png)

4. Click **Create** and sign in using the same account used for this lab

    ![](./media/image62.png)

    ![](../masterdoc/media/odlusr.png)

    ![](./media/image64.png)

5. Select your Fabric Data Agent

    - Look for the agent name you created in Exercise \#1\ Task 3

    - Click to select it.

        ![](../masterdoc/media/odlusr.png)

6. Enter the **Agent name** as **DataAgent- <inject key="DeploymentID" enableCopy="false"></inject>**,verify the **connection**, and then click **Add and configure** to proceed with the agent setup.

    ![](./media/image66.png)

7. Click **Publish** to make the agent available for use

    ![](./media/image67.png)

    ![](./media/publish-ss.png)

    ![](./media/image69.png)

## Task 3: Testing the connected Fabric Data Agent

1. Test the Fabric Data Agent connection with progressive queries:

   ```
   What are the top 10 highest value orders?
   ```

    ![](./media/image70.png)

2. Click **Allow** to grant the required permissions

    ![](./media/image71.png)

    ![](./media/image72.png)

    ![](./media/image73.png)

    ![](./media/image74.png)

    >**Note:** In case it takes more than 5-6 minutes to complete open a new test session and try again. Since the feature is in preview, it may not always work as expected.

    ```
    What is the average price per category? 
    ```

    ![](./media/image75.png)

    ![](./media/image76.png)

   ```
   What percentage of orders use credit card vs PayPal vs debit card?
   ```

    ![](./media/image77.png)

    ![](./media/image78.png)

   ```
   What is the revenue by payment method?
   ```

    ![](./media/image79.png)

    ![](./media/image80.png)

# Exercise 3: Connect the Fabric Data Agent to Teams

In this exercise, you will publish the Copilot agent to Teams, enabling business users to access enterprise data directly from within their collaboration app. You will validate the agent’s functionality by running several BI queries and observing real‑time responses inside Teams.

## Task 1: Add Copilot Capabilities

1. In the **E-commerce RAG Agent**, click the **+ (Add)** icon and select **Channels** to configure the agent channel settings.

    ![](./media/image81.png)

    ![](./media/image82.png)

2. Select **Teams and Microsoft 365 Copilot**

    ![](./media/image83.png)

3. Click Add Channel

    ![](./media/image84.png)

4. Select **See agent in Teams** to open and test the agent in **Microsoft Teams**.

    ![](./media/image85.png)

5. Click **Open Microsoft Teams**

    ![](./media/image86.png)

    ![](./media/image87.png)

6. Click **Sign in**

    ![](./media/image88.png)

7. Enter the credentials to sign in and continue
 
    - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

    - **Password:** <inject key="AzureAdUserPassword"></inject>

        ![](./media/image92.png)

8. Click **Add**

    ![](./media/image93.png)

9. After the app is added successfully, click the **Open** button to launch the E‑commerce RAG Agent in Microsoft Teams

    ![](./media/ragagent.png)

## Task 2: Testing the connected Fabric Data Agent

1. Test the Fabric Data Agent connection with progressive queries:

   ```
   What is the revenue trend over time?
   ``` 
 
    ![](./media/over-time.png)

2. Click **Allow** to grant the required permissions

    ![](./media/allow.png)
  
    ```
    What are the top 10 highest value orders? 
    ```

    ![](./media/value-orders.png)

    ```
    Which payment method is used the most?
    ``` 

    ![](./media/used-most.png)

# Summary

This use case focuses on how an e‑commerce organization can integrate
**Microsoft Fabric Data Agents** with **Microsoft Teams** through
**Copilot Studio** to deliver real‑time insights, natural‑language
analytics, and multi‑agent collaboration. By combining a unified
analytics platform (Microsoft Fabric) with conversational AI (Copilot
Studio and Teams), business users can seamlessly access sales trends,
product insights, and customer behaviour without writing queries. The
solution demonstrates how AI agents can retrieve data from Fabric
Lakehouse, enrich responses using instructions, and work alongside other
agents to streamline business intelligence workflows.

### You have successfully completed the lab. 