# Visualization, Business Intelligence, and AI in Cloud Analytics

## Learning Goals
* Explain the role of reporting and dashboards in unlocking business value from raw data.
* Describe the characteristics and advantages of managed, cloud-native Business Intelligence (BI) platforms.
* Identify how traditional machine learning and generative AI enhance data querying and analysis.

## Vocabulary and Synonyms
| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Business Intelligence (BI) Platform** | A system used to translate raw data into accessible visual narratives and interactive dashboards. | BI Tool, Reporting Suite | We use a BI platform to visualize our monthly sales trends. |
| **Drill Down** | The ability to click on a data point in a report or dashboard to access more detailed information. | Data Exploration, Interactive Analysis | Our sales dashboard allows users to drill down into specific regions to see individual store performance. |
| **Text-to-SQL** | An AI-driven process that translates natural language questions into database queries. | Natural Language Querying | Text-to-SQL allows our marketing team to ask questions without knowing how to write database code. |

## Translating Data with Reporting and Dashboards

Data has no value on its own. Its true worth is unlocked when decision-makers can understand and act upon it. The consumption layer of a cloud analytics architecture bridges the gap between raw datasets and actionable business insights by using business intelligence (BI) platforms, data visualization, and artificial intelligence.

If we just hand stakeholders a raw table of millions of rows, they cannot easily make decisions. Visualizations translate complex data into accessible visual narratives, making it easier for users to identify trends, patterns, and anomalies. For example, a hospital network can plot patient intake volumes on a simple bar chart to assist planning staffing levels, which is much faster than attempting to spot trends in raw text logs.

Reporting generally falls into three categories.
1. **Static reports** (such as PDFs) provide a fixed point-in-time snapshot of information.
2. **Interactive reports** provide self-service tools where users can apply filters or drill down into details.
3. **Dashboards** act as high-level visual representations of key business metrics.

![Three panels showing different report types: Static Reports (a static PDF document), Interactive Reports (a computer screen with an interactive chart featuring dropdown filters) with Drill Down and other Self Service Tools, and Dashboards (a large monitor displaying a dashboard with multiple high-level metric summaries) displaying Key Business Metrics.](./assets/analytics-report-types.png)  
*Fig. The three main categories of reporting: static reports, interactive reports, and dashboards.*

## Delivering Insights via Business Intelligence Platforms

Building custom web applications to visualize every new dataset can be slow and expensive. To deliver insights securely and at scale, we rely on **Business Intelligence (BI)** platforms.

BI platforms provide a wide variety of benefits to help enable insights for business users.

- **Dashboards and Interactive Reporting:** Users can design, customize, and publish highly formatted, infographic-style dashboards and reports that feature interactive drill-down and filtering capabilities.
- **Automated Delivery and Alerts:** Dashboards and reports can be scheduled for email delivery, and users can set up threshold-based alerts to be notified immediately of important changes in the data.
- **Embeddable Insights:** Organizations can embed interactive visualizations, dashboards, and even full authoring or natural language querying capabilities directly into their own custom applications and portals.
- **Connectivity:** They can securely connect to and combine petabytes of data across multiple data sources, including data warehouses, data lakes, and external providers.
- **Secure Access Control:** They offer robust governance tools, including Row-Level Security (RLS), which ensures that individual users only see the specific slices of data they are authorized to view within a shared dashboard.
- **Performance and Efficiency:** To speed data retrieval at scale and minimize repetitive queries to the underlying data sources, BI tools often utilize fast, in-memory calculation engines.

The specific features and capabilities of BI platforms can vary widely, but they all share the common goal of making data more accessible and actionable for business users.

In addition to their rich feature sets, modern cloud BI platforms are usually fully managed and can be serverless, allowing them to scale automatically to support thousands of concurrent users without the need to provision infrastructure.

## Enhancing Analytics with Machine Learning and AI

Many BI platforms have started to integrate machine learning and generative AI capabilities to enhance the analytics experience. In addition to helping us monitor current business operations through dashboards, these AI features allow us to anticipate issues and interact with our data more naturally.

BI tools leverage integrated machine learning models to automatically perform anomaly detection, alerting users to sudden data changes. They also power predictive forecasting based on historical trends. For instance, AI could highlight a sudden drop or spike in sales of particular offerings, which might indicate a problem in some market sector, or a new opportunity to pursue!

Another way that AI is integrated into modern analytics platforms is through providing AI assistants that allow non-technical business users to ask plain-language questions, such as "What were the top sales regions last month?". The AI assistant translates these natural language prompts into SQL queries using Text-to-SQL, immediately generating custom charts and insights.

For AI-powered queries to scale reliably without hallucinations, these systems rely on a high-quality semantic layer. Whether by leveraging resources like a data catalog, or using features included within the BI platform, having specific definitions and mappings for business terms (e.g., "revenue") ensures that the AI assistant can accurately retrieve the correct data and provide trustworthy insights.

![A flow diagram showing a user typing a plain-text question. The user asks "What were the top sales regions last month?". This is processed by an AI assistant using a governed semantic layer providing the business context mapping "revenue" to its data source. The AI translates the text into a SQL query using Text-to-SQL and sends the query to the database to perform data retrieval. The data retrieved from the database is used by the BI platform to produce Visual Output, comprising Charts & Insights. These are returned to the user, providing Immediate Insights.](./assets/analytics-text-to-sql.png)
*Fig. How Text-to-SQL translates natural language questions into visual insights using a semantic layer.*

## Summary

We learned that raw data only becomes valuable when it is translated into accessible visualizations, reports, and dashboards. By utilizing business intelligence platforms, we can securely deliver interactive insights to thousands of users through high-performance visualization tools, automated reporting, and embeddable analytics, all while maintaining robust security. Finally, integrating machine learning and generative AI empowers us to forecast trends and use Text-to-SQL to ask plain-language questions, as long as we maintain a well-governed semantic layer to ensure accuracy.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 91d676d0-3e43-431c-b38c-86c2d660f573
* title: Visualization, Business Intelligence, and AI in Cloud Analytics

##### !question

Which feature of modern cloud Business Intelligence (BI) platforms ensures that a single global sales dashboard only displays relevant regional data to a specific regional manager?

##### !end-question

##### !options

a| In-memory calculation engines
b| Anomaly detection
c| Text-to-SQL translation
d| Row-level security (RLS)

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

Row-level security (RLS) is a critical feature of modern cloud BI platforms that ensures users only see the data they are authorized to view. In the context of a global sales dashboard, RLS allows regional managers to access the same central dashboard while only displaying the metrics specific to their own geographic territories. This means that when a regional manager logs in, they will only see data relevant to their region, ensuring data privacy and relevance without needing to create separate dashboards for each region.

<br>

In-memory calculation engines enhance performance but do not control data access. Anomaly detection helps identify unusual data patterns but does not restrict data visibility. Text-to-SQL translation allows natural language queries to be converted into SQL but does not manage user permissions or data access.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 4012f633-2598-41a2-8296-039535352575
* title: Visualization, Business Intelligence, and AI in Cloud Analytics

##### !question

Which of the following are necessary components for a generative AI assistant to reliably generate immediate charts from natural language questions (like "What were our top sales last month?") without hallucinating?

##### !end-question

##### !options

a| Text-to-SQL capabilities to translate the prompt into database queries.
b| A highly governed semantic layer that consistently defines business terms across the organization.
c| An infrastructure team to manually provision servers every time a user asks a question.
d| A static PDF report of last month's data.

##### !end-options

##### !answer

a|
b|

##### !end-answer

##### !explanation

To reliably generate immediate charts from natural language questions, a generative AI assistant requires:
- **Text-to-SQL capabilities**: This allows the AI to translate the user's plain-language question into a structured SQL query that can retrieve the correct data from the database.
- **A highly governed semantic layer**: This ensures that business terms (like "revenue") are consistently defined across the organization, providing the AI with the necessary context to fetch accurate data and avoid hallucinations.

<br>

Manual server provisioning is not necessary for a generative AI assistant to function, as modern BI platforms are serverless and can scale automatically. A static PDF report would not allow for the dynamic generation of charts based on user queries.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 207e9573-3c85-4319-a46d-ac5fa6d9a444
* title: Visualization, Business Intelligence, and AI in Cloud Analytics

##### !question

Arrange the following steps to accurately represent how a generative AI assistant processes a natural language question to deliver a reliable visual insight.

##### !end-question

##### !answer

1. Data engineers define consistent business terms (like "revenue") within a semantic layer.
1. A non-technical business user types a plain-language question into the BI interface.
1. The AI assistant translates the natural language prompt into a SQL query.
1. The AI assistant outputs a generated chart or insight answering the user's question.

##### !end-answer

##### !explanation

The correct order of steps is as follows:
1. **Data engineers define consistent business terms within a semantic layer**: This foundational step ensures that the AI has a clear and consistent understanding of key business concepts, which is crucial for accurate data retrieval.
2. **A non-technical business user types a plain-language question into the BI interface**: The user interacts with the system using natural language, which is the starting point for the AI assistant's processing.
3. **The AI assistant translates the natural language prompt into a SQL query**: Using Text-to-SQL capabilities, the AI converts the user's question into a structured query that can be executed against the database.
4. **The AI assistant outputs a generated chart or insight answering the user's question**: Finally, the results from the SQL query are visualized and presented to the user as an immediate insight.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
