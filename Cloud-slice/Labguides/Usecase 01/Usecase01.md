# Usecase 01- Build sales analytics with AdventureWorks dataset using Fabric data agent

## Introduction

Contoso Analytics, a retail insights team, is transitioning its
reporting workflows to **Microsoft Fabric** to improve data
accessibility for analysts and business managers. The team wants to
enable natural‑language data exploration so non‑technical users can get
insights without writing SQL or navigating dashboards.

To achieve this, the team decides to build an **intelligent analytics
assistant** powered by a **Fabric Data Agent**. The first step in this
process is to prepare the underlying data in a **Fabric Lakehouse**. As
outlined in the Fabric data agent tutorial, they begin by **creating and populating a Lakehouse**, which will hold curated retail datasets such as sales transactions, product inventory, and store profiles. This
Lakehouse serves as the governed, centralized data source for downstream
tasks.

Once the Lakehouse is set up, the next step is to make it accessible to
conversational systems and automation tools. The team accomplishes this
by **creating a Fabric Data Agent** and **adding the Lakehouse as its connected data source**, enabling secure, governed access to the data.
This configuration allows the Data Agent to understand and query the
Lakehouse content, forming the foundation for building natural‑language
experiences across the organization.

With the Lakehouse connected through the Fabric Data Agent, Contoso can
now integrate the agent into analytics applications, Copilot
experiences, and internal tools empowering business users to ask
questions like *“Show me today’s sales for the south region”* or
*“Identify the lowest‑stock products across all stores”* and receive
data‑driven answers instantly.

## Lab Objectives

In this lab, you will complete the following tasks:

- Task 1: Create a Fabric workspace
- Task 2: Create a lakehouse with AdventureWorksLH
- Task 3: Create and configure a Fabric Data Agent connected to Lakehouse tables.
- Task 4: Improve the agent’s responses using instructions and example queries.
- Task 5: Publish the agent and test it programmatically via API calls inside a Fabric notebook.


## Task 1: Create a Fabric workspace

In this task, you will set up the foundational environment by creating a
Fabric workspace that will host the Lakehouse, notebooks, and the Data
Agent. This workspace acts as the central container for all assets used
throughout the use case.

1. Open your web browser and go to the address bar. Type or paste the provided URL, then press **Enter** to proceed.

    ```
    https://app.fabric.microsoft.com  
    ```

2. In the **Microsoft Fabric** window, enter your credentials, and click on the **Submit** button.

    - **Email/Username** : <inject key="AzureAdUserPassword"></inject> 

        ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

3. Then, In the **Microsoft** window enter the password and click on the **Sign in** button.

   - **Password**  : <inject key="AzureAdUserPassword"></inject>

        ![A login screen with a red box and blue text AI-generated content may
be incorrect.](./media/image8.png)

4. In **Stay signed in?** window, click on the **Yes** button.

5. You’ll be directed to Power BI Home page.

    ![](./media/image9.png)

6. Fabric home page, select **+New workspace** tile.

    ![](./media/image10.png)

7. In the **Create a workspace** pane that appears on the right side, enter the following details, and click on the **Apply** button.

    | Property | Value |
    |---------|-------|
    | Name | **Fabric Data agent-<inject key="DeploymentID" enableCopy="false"></inject>**|
    | Advanced | Under **License mode**, select **Fabric** |
    | Default storage format | Small dataset storage format |
    | Template apps | Check **Develop template apps** |

    ![](./media/image11.png)

    ![](./media/image13.png)

8. Wait for the deployment to complete. It takes 1-2 minutes to complete.

    ![](./media/image14.png)

## Task 2: Create a lakehouse with AdventureWorksLH

In this task, you will create a new Lakehouse and populate it with AdventureWorks tables using a Fabric notebook, establishing a structured data foundation that the Data Agent can query.

1. Create a new lakehouse by clicking on the **+New item** button in the navigation bar.

    ![](./media/image15.png)

2. Click on the "**Lakehouse**" tile.

    ![](./media/image16.png)

3. In the **New lakehouse** dialog box, enter the following name in the **Name** field:

    ```
    AdventureWorksLH
    ```

    Then, click **Create** and open the newly created lakehouse.

    ![](./media/image17.png)

    ![](./media/image18.png)

4. You will see a notification stating **Successfully created SQL endpoint**.

    ![](./media/image19.png)

5. Click on **Open notebook**, then from the dropdown menu select **New notebook** in the lakehouse where you want to create your Fabric data agent.

    ![](./media/image20.png)

6. Update the code in the **cell** with the following code and click on **▷ Run cell** that appears to the left of the cell.

    ```
    import pandas as pd
    from tqdm.auto import tqdm
    base = "https://synapseaisolutionsa.z13.web.core.windows.net/data/AdventureWorks"

    # load list of tables
    df_tables = pd.read_csv(f"{base}/adventureworks.csv", names=["table"])

    for table in (pbar := tqdm(df_tables['table'].values)):
        pbar.set_description(f"Uploading {table} to lakehouse")

        # download
        df = pd.read_parquet(f"{base}/{table}.parquet")

        # save as lakehouse table
        spark.createDataFrame(df).write.mode('overwrite').saveAsTable(table)
    ```

    ![](./media/image21.png)

    ![](./media/image22.png)

    ![](./media/image23.png)

    ![](./media/image24.png)

    ![](./media/image25.png)

    > **Note:** Wait until the progress reaches 100%.

## Task 3: Create Data agent

In this task, you will create a Fabric Data Agent and connect it to the
Lakehouse. You will select the required Dimension and Fact tables to
enable the agent to answer a wide range of sales‑related analytics
questions.

1. Now, click on **Fabric Data agent- <inject key="DeploymentID" enableCopy="false"></inject>** on the left-sided navigation pane.

    ![](./media/image26.png)

2. In the **Fabric** home page, select **+New item.**

    ![](./media/image27.png)

3. In the **Filter by item type** search box, enter **data agent (1)** and select the **Data agent (2)**

    ![](./media/data-agent.png)

4. Enter **AI-agent** as the Data agent name and select **Create**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

5. In AI-agent page, select **Add data (1)** and from the dropdown **choose data source(2)**.

    ![](./media/add-data-source.png)

6. In the **OneLake catalog** tab, select the **AdventureWorksLH (1)** and select **Add (2)**.

    ![](./media/adventureworks-lh.png)

7. You must then select the tables for which you want the AI skill to have available access.

    This lab uses these tables:

    - DimCustomer

    - DimDate

    - DimGeography

    - DimProduct

    - DimProductCategory

    - DimPromotion

    - DimReseller

    - DimSalesTerritory

    - FactInternetSales

    - FactResellerSales

        ![](./media/image34.png)

## Task 4: Provide instructions

In this task, you will enrich the Data Agent by adding natural language questions along with their corresponding SQL queries. These examples help the agent understand domain-specific context and generate more accurate SQL responses for real-world queries. You will add relevant question–SQL pairs, validate their accuracy, test how the agent responds to similar queries, and refine the examples to improve the agent’s performance.


1. When you first ask the questions with the listed tables select **factinternetsales**, the data agent answers them fairly well.

2. For instance, for the question :

    ```
    What is the most sold product?
    ```

    ![](./media/image35.png)

    ![](./media/image36.png)

3. Copy the question and SQL queries and paste them in a notepad and then Save the notepad to use the information in the upcoming tasks.

    ![A screenshot of a computer Description automatically
generated](./media/image37.png)

    ![A screenshot of a computer Description automatically
generated](./media/image38.png)

4. Select **FactResellerSales** and enter the following text and click on the **Submit icon** as shown in the below image.

    ```
    What is our most sold product?
    ```

    ![A screenshot of a computer Description automatically
generated](./media/image39.png)

    ![A screenshot of a computer Description automatically
generated](./media/image40.png)

    As you continue to experiment with queries, you should add more
instructions.

5. Select the **dimcustomer** , enter the following text and click on
  the **Submit icon**
    
    ```
    how many active customers did we have June 1st, 2013?
    ```

    ![A screenshot of a computer Description automatically
generated](./media/image41.png)

    ![A screenshot of a computer Description automatically
generated](./media/image42.png)

6. Copy the all question and SQL queries and paste them in a notepad and then Save the notepad to use the information in the upcoming tasks.

    ![A screenshot of a computer Description automatically
 generated](./media/image43.png)

    ![A screenshot of a computer Description automatically
generated](./media/image44.png)

7. Select the **dimdate**, **FactInternetSales** , enter the following
  text and click on the **Submit icon:**

    ```
    what are the monthly sales trends for the last year?
    ```

    ![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

    ![A screenshot of a computer Description automatically
 generated](./media/image46.png)

8. Select the **dimproduct**,**FactInternetSales** , enter the
  following text and click on the **Submit icon:**
   
    ```
    which product category had the highest average sales price?
    ```

    ![A screenshot of a computer Description automatically
 generated](./media/image47.png)

    ![A screenshot of a computer Description automatically
generated](./media/image48.png)

    >Note: Part of the problem is that "active customer" doesn't have a formal
    definition. More instructions in the notes to the model text box might
    help, but users might frequently ask this question. You need to make
    sure that the AI handles the question correctly.

7. The relevant query is moderately complex, so provide an example by selecting the **Example queries** button from the **Setup** pane.

    ![](./media/image49.png)

8. In the Example queries tab, select the **Add example.**

    ![](./media/image50.png)

9. Here, you should add Example queries for the lakehouse data source
  that you have created. Add the below question in the question field:
    
    ```
    What is the most sold product?
    ```

    ![](./media/image51.png)

10. Add the query1 that you have saved in the notepad:

    ```
    SELECT TOP 1 ProductKey, SUM(OrderQuantity) AS TotalQuantitySold
    FROM [dbo].[factinternetsales]
    GROUP BY ProductKey
    ORDER BY TotalQuantitySold DESC
    ```

    ![](./media/image52.png)

11. To add a new query field, click on **+Add.**

    ![](./media/image53.png)

12. To add a second question in the question field:

    ```
    What are the monthly sales trends for the last year?
    ```

    ![](./media/image54.png)

13. Add the query3 that you have saved in the notepad:

    ```
    SELECT
    d.CalendarYear,
    d.MonthNumberOfYear,
    d.EnglishMonthName,
    SUM(f.SalesAmount) AS TotalSales
    FROM
    dbo.factinternetsales f
    INNER JOIN dbo.dimdate d ON f.OrderDateKey = d.DateKey
    WHERE
    d.CalendarYear = (
        SELECT MAX(CalendarYear)
        FROM dbo.dimdate
        WHERE DateKey IN (SELECT DISTINCT OrderDateKey FROM dbo.factinternetsales)
    )
    GROUP BY
    d.CalendarYear,
    d.MonthNumberOfYear,
    d.EnglishMonthName
    ORDER BY
    d.MonthNumberOfYear
    ```

    ![](./media/image55.png)

14. To add a new query field, click on **+Add.**

    ![](./media/image56.png)

15. To add a third question in the question field:

    ```
    Which product category has the highest average sales price? 
    ```

    ![](./media/image57.png)

16. Add the query4 that you have saved in the notepad:

    ```
    SELECT TOP 1
    dp.ProductSubcategoryKey AS ProductCategory,
    AVG(fis.UnitPrice) AS AverageSalesPrice
    FROM
    dbo.factinternetsales fis
    INNER JOIN
    dbo.dimproduct dp ON fis.ProductKey = dp.ProductKey
    GROUP BY
    dp.ProductSubcategoryKey
    ORDER BY
    AverageSalesPrice DESC
    ```

    ![](./media/image58.png)

17. Add all the queries and SQL queries that you have saved in Notepad,
  and then click on ‘**Export all’**

    ![](./media/image59.png)

    ![](./media/image60.png)

## Task 5: Use the Data agent programmatically

In this task, you will validate and enhance the Data Agent by reviewing its instructions and examples, and by using the AI skill programmatically within a Fabric notebook. You will check whether the AI skill is properly configured and confirm that a published URL is available. You will then use the AI skill within a Fabric notebook to test responses using sample queries, and refine the instructions and examples based on the results to improve its overall effectiveness.

1. In the Data agent Fabric page, in the **Home** ribbon select the **Settings**.

    ![](./media/image61.png)

2. Before you publish the AI skill, it doesn't have a published URL
  value, as shown in this screenshot.

3. Close the AI Skill setting.

    ![](./media/image62.png)

4. In the **Home** ribbon, select the **Publish**.

    ![](./media/image63.png)
 
    ![](./media/image64.png)

5. Click on the **View publishing details**

    ![](./media/image65.png)

6. The published URL for the AI agent appears, as shown in this
  screenshot.

7. Copy the URL and paste that in a notepad and then Save the notepad
  to use the information in the upcoming steps.

    ![](./media/image66.png)

8. Select **Notebook1** in the left navigation pane.

    ![](./media/image67.png)

9. Use the **+ Code** icon below the cell output to add a new code cell
  to the notebook, enter the following code in it and replace
  the **URL**. Click on **▷ Run** button and review the output

    ```
    %pip install "openai==1.70.0" 
    ```

    ![](./media/image68.png)
 
    ![](./media/image69.png)

1. Use the **+ Code** icon below the cell output to add a new code cell
  to the notebook, enter the following code in it and replace
  the **URL**. Click on **▷ Run** button and review the output

    ```
    %pip install httpx==0.27.2 
    ```

    ![](./media/image70.png)

1. Use the **+ Code** icon below the cell output to add a new code cell to the notebook. Enter the following code in the new cell and replace the **URL** as required. Then, click on the **▷ Run** button and review the output.

```
import requests
import json
import pprint
import typing as t
import time
import uuid

from openai import OpenAI
from openai._exceptions import APIStatusError
from openai._models import FinalRequestOptions
from openai._types import Omit
from openai._utils import is_given
from synapse.ml.mlflow import get_mlflow_env_config
from sempy.fabric._token_provider import SynapseTokenProvider

base_url = "https://api.fabric.microsoft.com/v1/workspaces/989ec440-4aca-4a01-b19b-ba778325c043/dataagents/fa0d717a-6457-405c-8856-25bd4aabdd6a/aiassistant/openai"
question = "What datasources do you have access to?"

configs = get_mlflow_env_config()

# Create OpenAI Client
class FabricOpenAI(OpenAI):
    def __init__(
        self,
        api_version: str = "2024-05-01-preview",
        **kwargs: t.Any,
    ) -> None:
        self.api_version = api_version
        default_query = kwargs.pop("default_query", {})
        default_query["api-version"] = self.api_version

        super().__init__(
            api_key="",
            base_url=base_url,
            default_query=default_query,
            **kwargs,
        )

    def _prepare_options(self, options: FinalRequestOptions) -> None:
        headers: dict[str, str | Omit] = (
            {**options.headers} if is_given(options.headers) else {}
        )
        options.headers = headers

        headers["Authorization"] = f"Bearer {configs.driver_aad_token}"

        if "Accept" not in headers:
            headers["Accept"] = "application/json"

        if "ActivityId" not in headers:
            correlation_id = str(uuid.uuid4())
            headers["ActivityId"] = correlation_id

        return super()._prepare_options(options)


# Pretty printing helper
def pretty_print(messages):
    print("---Conversation---")
    for m in messages:
        print(f"{m.role}: {m.content[0].text.value}")
    print()


fabric_client = FabricOpenAI()

# Create assistant
assistant = fabric_client.beta.assistants.create(model="not used")

# Create thread
thread = fabric_client.beta.threads.create()

# Create message on thread
message = fabric_client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content=question,
)

# Create run
run = fabric_client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant.id,
)

# Wait for run to complete
while run.status in ["queued", "in_progress"]:
    run = fabric_client.beta.threads.runs.retrieve(
        thread_id=thread.id,
        run_id=run.id,
    )
    print(run.status)
    time.sleep(2)

# Print messages
response = fabric_client.beta.threads.messages.list(
    thread_id=thread.id,
    order="asc",
)
pretty_print(response)

# Delete thread
fabric_client.beta.threads.delete(thread_id=thread.id)
```

    ![](./media/image71.png)

    ![](./media/image72.png)


## Summary:

In this lab, you learned how to unlock the power of conversational
analytics using Microsoft Fabric’s Data Agent. You configured a Fabric
workspace, ingested structured data into a lakehouse, and set up an AI
skill to translate natural language questions into SQL queries. You also
enhanced the AI agent’s capabilities by providing instructions and
examples to refine query generation. Finally, you called the agent
programmatically from a Fabric notebook, demonstrating end-to-end AI
integration. This lab empowers you to make enterprise data more
accessible, usable, and intelligent for business users through natural
language and generative AI technologies.

### You have successfully completed the lab. Click on Next >> to proceed with the next lab.

![Start Your Azure Journey](../masterdoc/media/PageNo.png)
