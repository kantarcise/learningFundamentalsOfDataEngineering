## 9. Serving Data for Analytics, Machine Learning, and Reverse ETL 🍜

***Serving*** is the final stage of the data engineering lifecycle, where data is delivered to drive insights, predictions, and actions.

It covers use cases like dashboards, machine learning, and feeding transformed data back into operational tools (reverse ETL).

Success here depends on ***data trust***, ***user understanding***, ***performance***, and ***thoughtful system design***.

Let's discover them further 😌

### General Considerations for Serving Data

#### Trust

Trust is the most critical factor when serving data—if users don’t believe the data is accurate or consistent, they won’t use it.

Data validation, observability, and adherence to SLAs/SLOs ensure trustworthiness throughout the lifecycle. Once trust is lost, it’s difficult to regain and often leads to poor adoption and failed data initiatives.

Here is an example on **SLA** and **SLO** at serving stage.

- For example, an **SLA** might state: “Data will be consistently available and of high quality.”

- An **SLO**, which supports the SLA, specifies how performance will be measured—such as ***“Our data pipelines to dashboards or ML workflows will maintain 99% uptime, with at least 95% of data free from defects.”***

Setting an SLA isn’t enough. Clear communication is essential—teams must regularly discuss any risks that could impact expectations and define a process for addressing issues and continuous improvement.

#### What’s the Use Case, and Who’s the User?

Knowing the user and their intended action helps shape data products with real business impact.

Serving data should begin by identifying the use case and working backwards from the decision or trigger it supports.

This user-first approach ensures relevance, usability, and alignment with goals.

#### Data Products

A **data product** is a reusable dataset or service that solves a defined user problem through data.

Building effective products requires collaboration with end users and clarity on their goals and expected outcomes. 

Good data products generate feedback loops, improving themselves as usage increases and needs evolve.

#### Data Definitions and Logic

Definitions like ***“customer”*** or ***“churn”*** must be consistent across systems to ensure correct and aligned usage.

Embedded business logic should be captured and centralized to avoid ambiguity and hidden institutional knowledge. 

Tools like semantic layers or catalogs can document and enforce shared definitions across teams and systems.

#### Data Mesh

Data mesh distributes data ownership across teams, turning them into both ***producers and consumers*** of high-quality data.

This decentralization improves scale and accountability, as each domain serves its data for others to use. It changes how data is served—teams must prepare, document, and support the data they publish to the mesh.

### Analytics

This is the first use case for data-serving. 😌

#### Business Analytics

***Business analytics*** helps stakeholders make strategic decisions using **historical trends**, **KPIs**, and **dashboards**. 

Data is often served through data warehouses or lakes, using BI tools like [Tableau](https://www.tableau.com/), [Looker](https://cloud.google.com/looker?hl=en), or [Power BI](https://www.microsoft.com/en-us/power-platform/products/power-bi). 

Dashboards, reports, and ad hoc analysis are key outputs, with data engineers enabling access and quality.

#### Operational Analytics

***Operational analytics*** supports **real-time monitoring** and **rapid responses** by processing live data streams.

It powers use cases like fraud detection, system monitoring, and factory floor analytics with low-latency data. This category requires real-time pipelines and databases optimized for concurrency, freshness, and speed. 

Real-time analytics at the factory is a great example here!

#### Embedded Analytics

***Embedded analytics*** integrates data and insights directly into user-facing applications, enabling real-time, data-driven decision-making.

For instance, a smart thermostat app displays live temperature and energy usage, helping users optimize their heating or cooling schedules for efficiency.

Similarly, a third-party e-commerce platform offers sellers real-time dashboards on sales, inventory, and returns—empowering them to react quickly, like launching instant promotions.

Other examples include fitness apps showing health trends and workout suggestions based on user data, SaaS platforms that provide usage and engagement insights to customer success teams, and ride-sharing apps surfacing driver performance and earnings in real time.

In all these cases, analytics isn’t a separate tool—it’s woven into the experience, driving immediate, contextual decisions.

Users expect near-instant data updates and smooth interactivity, which requires low-latency serving systems. Data engineers manage performance, concurrency, and delivery infrastructure behind the scenes.

#### Machine Learning

Really good quote:

> Boundary between ML, data science, data engineering, and ML engineering is increasingly fuzzy, and this boundary varies dramatically between organizations.

Serving for ML means preparing and delivering high-quality data for model training, tuning, and inference. 

Data engineers may handle raw ingestion, feature pipelines, or even [batch scoring](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/batch-scoring-databricks) alongside ML teams.

#### What a Data Engineer Should Know About ML 🤨

- Data engineers don’t need to be ML experts but should understand core concepts like supervised learning and feature engineering.

- They should know when to use batch vs. online learning, how to prepare structured/unstructured data, common ML workflows and difference between classification and regression techniques.

- When to choose the “classical” techniques (logistic regression, tree-based learning, support vector machines) versus deep learning.

This knowledge helps data engineers better support ML pipelines and collaborate effectively with data scientists and ML engineers.

### Ways to Serve Data for Analytics and ML

#### File Exchange

File-based serving is still common—CSV, Excel, JSON—but lacks scalability and consistency. 

Better alternatives include cloud file sharing, object storage, or automated pipelines into data lakes.

It’s often a stopgap or used when consumers lack access to more advanced platforms.

#### Databases

OLAP databases like [Snowflake](https://www.snowflake.com/en/emea/), [BigQuery](https://cloud.google.com/bigquery?hl=en), and [Redshift](https://aws.amazon.com/redshift/) offer structured, high-performance serving for analytics and ML. 

They support schemas, access control, and caching, and allow slicing compute for cost management.

Data engineers manage performance, security, and scaling based on usage and workload.

#### Streaming Systems

Streaming systems enable near real-time serving by continuously processing incoming data. They’re used for operational dashboards, anomaly detection, and time-sensitive applications.

Technologies like [Flink](https://flink.apache.org/), [Kafka](https://kafka.apache.org/), and materialized views help bridge streaming and batch worlds.

#### Query Federation

Query federation lets users query multiple systems (e.g., OLTP, OLAP, APIs) without centralizing the data.

It’s useful for ad hoc analysis and controlled access but requires performance and resource safeguards.

Tools like [Trino](https://trino.io/) and [Starburst](https://www.starburst.io/) make this practical, especially in data mesh environments.

#### Data Sharing

Data sharing provides secure, scalable access to datasets between teams or organizations in the cloud.

It reduces the need for duplicating data and allows for real-time consumption through platforms like [Snowflake](https://www.snowflake.com/en/emea/) or [BigQuery](https://cloud.google.com/bigquery?hl=en).

Access control becomes the main concern, and engineers shift to enabling visibility while managing cost.

#### Semantic and Metrics Layers

Semantic layers define shared metrics and business logic once, enabling reuse across dashboards and queries.

They improve consistency, trust, and speed of development by centralizing definitions. 

Tools like [Looker](https://cloud.google.com/looker?hl=en) and [dbt](https://www.getdbt.com/) exemplify this approach, bridging analysts, engineers, and stakeholders.

#### Serving Data in Notebooks

Notebooks like Jupyter are central to data science work, but local environments often hit memory limits. Data scientists typically connect to data sources programmatically, whether it's an API, a relational database, a cloud data warehouse, or a data lake.

Engineers help scale access via cloud notebooks, distributed engines (e.g., [Dask](https://www.dask.org/), [Spark](https://spark.apache.org/)), or managed services. They also manage permissions, access control, and infrastructure for collaborative, reproducible analysis.

#### Reverse ETL

Reverse ETL pushes processed data from the warehouse back into operational tools like CRMs or ad platforms.

It enables teams to act on insights directly within their workflows, improving impact and usability.

However, it makes feedback loops and must be carefully monitored for accuracy, cost, and unintended consequences.

### Summary

At the final stage of the data engineering lifecycle, serving data ensures insights flow into action.

This involves delivering clean, timely, and trustworthy data to a variety of consumers: analysts generating dashboards and reports, data scientists training models, and even business systems via reverse ETL—where insights are pushed back into operational tools like CRMs or ad platforms. 

Regardless of the use case, trust is foundational: teams must invest in ***data validation***, ***observability***, and ***clear service-level agreements*** (SLAs/SLOs) to maintain reliability. 

The right data definitions and consistent logic—often managed via semantic or metrics layers—ensure that users interpret and act on data the same way across the organization.

Data must be served with the ***user*** and ***use case*** in mind. 

Business analysts rely on OLAP databases and BI tools (like Tableau, Looker, or Power BI) for trend detection and strategic reporting, while operational and embedded analytics require real-time or low-latency systems.

For machine learning, engineers prepare structured or semi-structured data for offline or online model training, often via feature pipelines and batch exports from data warehouses. 

Data scientists may use notebooks like Jupyter, often hitting limits of local memory and scaling into tools like Spark, Ray, or SageMaker. 

Whether serving analytics or ML, delivery options include query engines, object storage, streaming systems, and federated queries—all chosen based on latency, concurrency, and access control needs.

Lastly, reverse ETL has emerged as a key method to close the loop between insights and action. Rather than expecting users to access insights in dashboards or files, reverse ETL pipelines push enriched or modeled data directly into operational tools—like inserting ML-scored leads back into Salesforce.

This approach reduces friction and enables real-time decisioning within the tools teams already use.

However, it also introduces potential feedback loops and risks, such as runaway bid models in ad platforms. Monitoring and safeguards are essential. 

As serving becomes more complex and democratized, concepts like data mesh, where teams produce and consume data products autonomously, shift the mindset from centralized pipelines to federated, domain-driven delivery.

---

