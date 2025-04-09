## 8. Queries, Modeling, and Transformation 🪇

Now we'll learn how to make data useful.

### Queries

Queries are at the core of data engineering and data analysis, enabling users to interact with, manipulate, and retrieve data.

Here is an example query:

```sql
SELECT name, age
FROM df
WHERE city = 'LA' AND age > 27;
```

Here is a complete example in Python:

```python
import pandas as pd
import duckdb

# Create a sample DataFrame
data = {
    'id': [1, 2, 3, 4, 5],
    'name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'age': [25, 30, 35, 40, 28],
    'city': ['NY', 'LA', 'NY', 'SF', 'LA']
}

df = pd.DataFrame(data)

# Run SQL query: Get users from LA older than 27
result = duckdb.query("""
    SELECT name, age
    FROM df
    WHERE city = 'LA' AND age > 27
""").to_df()

print(result)
#   name  age
# 0  Bob   30
```

***Structured Query Language*** (SQL) is commonly used for querying tabular and semistructured data.

A query may read data (SELECT), modify it (INSERT, UPDATE, DELETE), or control access (GRANT, REVOKE).

Under the hood, a query goes through parsing, compilation to bytecode, optimization, and execution.

Various query languages (DML, DDL, DCL, TCL) are used to define and manipulate data and database objects, manage access, and control transactions for consistency and reliability.

To improve query performance, data engineers must understand the role of the query optimizer and write efficient queries. Strategies include **optimizing joins**, using **prejoined tables or materialized views**, leveraging **indexes and partitioning**, and **avoiding full table scans**.

Engineers should monitor execution plans, system resource usage, and take advantage of query caching. Managing commits properly and vacuuming dead records are essential to maintain database performance. Understanding the consistency models of databases (e.g., ACID, eventual consistency) ensures reliable query results.

Streaming queries differ from batch queries, requiring real-time strategies such as session, fixed-time, or sliding windows.

Watermarks are used to handle late-arriving data, while triggers enable event-driven processing.

Combining streams with batch data, enriching events, or joining multiple streams adds complexity but unlocks deeper insights.

Technologies like [Kafka](https://kafka.apache.org/), [Flink](https://flink.apache.org/), and [Spark](https://spark.apache.org/) are essential for such patterns. Modern architectures like Kappa treat streaming logs as first-class data stores, enabling analytics on both recent and historical data with minimal latency.

### Data Modeling

**Data modeling** is a foundational practice in data engineering that ensures data structures ***reflect business needs and logic***. A data model shows how data relates to real world.

Despite its long-standing history, it has often been overlooked, especially with the rise of big data and NoSQL systems. 

Today, there's a renewed focus on data modeling as companies recognize the importance of structured data for quality, governance, and decision-making. 

A good data model aligns with business outcomes, supports consistent definitions (like what qualifies as a "customer"), and provides a scalable framework for analytics. 

Modeling typically progresses from ***conceptual*** (business rules), to ***logical*** (types and keys), to ***physical*** (actual database schemas), and always considers the grain (resolution which data is stored and queried) of data.

A normalized model avoids redundancy and maintains data integrity. The first three normal forms (1NF, 2NF, 3NF) establish increasingly strict rules for structuring tables. While normalization reduces duplication, denormalization—often found in analytical or OLAP systems—can improve performance. 

Three dominant batch modeling strategies are **Inmon** (centralized, normalized warehouse with downstream marts), **Kimball** (dimensional model with fact/dimension tables in star schemas), and **Data Vault** (insert-only, agile, source-aligned modeling using hubs, links, and satellites). 

Wide, denormalized tables are gaining popularity in the cloud era due to flexible schemas and cheap storage, especially in columnar databases.

Additionally, streaming data modeling presents new challenges. Traditional batch paradigms don’t easily apply due to continuous schema changes and unbounded nature. 

So flexibility is key: assume the source defines business logic, expect schema drift, and store both recent and historical data together. 

Automation and dynamic analytics on streaming data are emerging trends. While no universal approach has yet emerged, models like the Data Vault show promise in adapting to streaming workflows. 

The future may involve unified layers that combine metrics, semantics, pipelines, and real-time source-connected analytics, reducing the batch-vs-stream divide.

### Transformation

Transformations enhance and persist data for downstream use. 

Unlike queries, which retrieve data, transformations are about shaping and saving data—often as part of a pipeline. This reduces cost, increases performance, and enables reuse.

#### Batch Transformations

Batch transformations process data in chunks on a schedule (e.g., hourly, daily) and support reports, analytics, and ML models.

**Distributed Joins**

- **Broadcast Join**: Small table is sent to each node to join with partitions of a large table. Great for performance.
- **Shuffle Hash Join**: When both tables are large. Data is redistributed based on join keys and then joined. More resource-intensive.

**ETL vs. ELT**

- **ETL**: Extract → Transform → Load. Originated when resources were limited.
- **ELT**: Extract → Load → Transform. Modern systems (data lakes/lakehouses) delay transformations.

Choose based on context—no need to stick to one approach for the entire org.

**SQL vs. Code-Based Tools**

- SQL is declarative and often sufficient for complex workflows using views or orchestration tools.
- Use Spark/PySpark when SQL becomes unreadable or hard to maintain, or for advanced operations (e.g., stemming).

Avoid excessive use of Python UDFs; they slow performance in Spark. Prefer native Scala/Java implementations when needed.

**Update Patterns**

- **Truncate & Reload**: Clears and regenerates entire table.
- **Insert Only**: Appends data, useful for tracking history.
- **Delete**: Can be soft (mark as deleted) or hard (remove permanently).
- **Upsert/Merge**: Updates or inserts depending on whether a match is found. Costly in columnar systems, so optimize merge frequency.

**Schema Updates**

- Columnar systems make adding/removing fields easier.
- JSON fields support evolving schemas.
- Plan and manage schema updates to avoid breaking transformations.

**Data Wrangling**

- Cleans and transforms messy, malformed input data.
- Wrangling tools offer visual, code-generating interfaces—especially useful with semi-structured or complex text data.

**Example ([Spark](https://spark.apache.org/))**

- Ingest 3 JSON APIs → Join and transform in Spark → Output to Delta Lake in S3.
- Use DAGs for orchestration (e.g., Spark DAG inside [Airflow](https://airflow.apache.org/) DAG).

**Business Logic & Derived Data**

- Encode nuanced calculations (e.g., profits with/without marketing costs).
- Use metrics layers to centralize logic and avoid duplication across scripts.

**MapReduce**

- Influential, now outdated. Relied on disk for all intermediate steps.
- [Spark](https://spark.apache.org/) and modern tools keep data in memory for better performance.

#### Materialized Views, Federation, and Query Virtualization

Here are some oneliners.

- **Views**: Reusable query logic, used for security, deduplication, and common access patterns.
- **Materialized Views**: Precompute and persist results for better performance.
- **Composable Materialized Views**: Layers of views (e.g., Databricks live tables).

- **Federated Queries**: Query external data sources (e.g., S3, MySQL) from within a data warehouse. Enables hybrid data lake/warehouse patterns.
- **Data Virtualization**: Process data without storing it. Used in engines like Trino and Presto. Push computation to source systems to minimize load. Avoid virtualizing production databases for analytics.

#### Streaming Transformations and Processing

**Streaming Transformations vs. Queries**

- Queries present live views.
- Transformations enrich or shape streams for downstream use.

**Streaming DAGs**

- DAGs for streaming (e.g., Pulsar) simplify processing by unifying flows in code.

**Micro-Batch vs. True Streaming**

- **Micro-Batch**: Small intervals (seconds/minutes). Works well for most business needs (e.g., Black Friday metrics).
- **True Streaming**: Processes every event as it arrives. Ideal for ultra-low latency (e.g., DDoS detection).

Choose based on latency requirements, team expertise, and real-world testing.

### Summary

Modern data systems revolve around three tightly interwoven pillars: ***queries***, ***data modeling***, and ***transformations***.

At the surface, SQL queries let us retrieve, filter, and analyze data in declarative ways, whether for dashboards or ad-hoc investigations.

But queries alone are not enough—they assume data is structured and meaningful. Techniques like joins (e.g., combining customer orders and product data), window functions, and streaming queries (e.g., computing moving averages in real time) depend on underlying data that’s clean, normalized, and aligned with business logic. Without good structure, queries become brittle, hard to reuse, and difficult to scale.

That structure comes from data modeling—the process of organizing raw data into logical layers that reflect the organization’s goals.

Whether it’s Inmon’s normalized warehouse-first approach, Kimball’s dimensional star schemas, or the flexibility of a Data Vault, modeling helps define relationships, enforce consistency, and preserve meaning over time. 

Modeling even applies to stream data, albeit in more relaxed forms, where business definitions may shift dynamically, and flexibility (e.g., using JSON columns or CDC feeds) becomes more important than strict schema enforcement. Poorly modeled data often leads to **data swamps**, reporting confusion, and **redundant pipelines**—while good models lead to faster insights and cleaner transformations downstream.

Finally, transformations take center stage in turning data into its most useful, consumable form. This includes batch pipelines (e.g., ETL/ELT jobs using Spark or SQL), real-time stream enrichments, and creating derived data that reflects business logic like profit metrics.

Tools like materialized views, [Airflow](https://airflow.apache.org/) DAGs, and orchestration frameworks help simplify these complex workflows and reduce redundant processing.

As data engineers, we’re often tasked with choosing between performance and flexibility—using insert-only patterns, upserts, or schema evolution strategies that balance cost and query speed.

Whether we persist transformed data in a wide denormalized table, or virtualize it across systems via tools like Trino, our transformations are what elevate raw data into decision-ready information.

---
