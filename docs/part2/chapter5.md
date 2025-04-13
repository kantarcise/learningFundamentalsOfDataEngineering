## 5. Data Generation in Source Systems

Before getting the raw data, we must understand where the data exists, how it is generated, and its characteristics.

Let's make sure we get the absolute basics about source systems correctly. 🍓

### Main Ideas on Source Systems

#### Files

A **file** is a sequence of bytes, typically stored on a disk. Applications often write data to files. Files may store local parameters, events, logs, images, and audio.

In addition, files are a universal medium of data exchange. As much as data engineers wish that they could get data programmatically, much of the world still sends and receives files.

#### APIs

**API**'s are a standard data exchange method. 

A simple example would be the "log in with Twitter/Google/GitHub" capability seen on many websites. Rather than entering into users' social media accounts (which would be a severe security risk), applications with this capability use the APIs of these platforms to authenticate the user with each login.

#### Application Databases (OLTP)

**Application databases** store app state with fast, high-volume reads/writes. 

They are ideal for transactional tasks like banking. Commonly low-latency, high-concurrency systems—RDBMS, document, or graph DBs.

More info about **ACID** and **atomic transactions** can be found here.

#### OLAP Systems

Built for large, interactive analytics—relatively slower at single-record lookups. Often used in ML pipelines or reverse ETL.

#### Change Data Capture (CDC)

**CDC**, captures DB changes (insert/update/delete) for real-time sync or streaming. Implementation varies by database type.

Database Logs store operations before execution for recovery. Key for CDC and reliable event streams.

#### Logs

**Logs** are tracked system events for analysis or ML. Common sources: OS, apps, servers, networks. Formats: binary, semi-structured (e.g. JSON), or plain text.

Log Resolution defines how much detail logs capture. Log level controls what gets recorded (e.g., errors only). Logs can be batch or real-time.

#### CRUD

Create, Read, Update, Delete. 

Core data operations in apps and databases. Common in APIs and storage systems.

#### Insert-Only

Instead of updates, new records are inserted with timestamps. This is great for history, but tables grow fast and lookups get costly.

#### Messages and Streams

***Messages*** are single-use signals between systems. Once the message is received, and the action is taken, the message is removed from the message queue. 

***Streams*** are ordered, persistent logs of events for long-term processing. With append only nature, records in a stream are persisted over a
retention window. 

Streaming platforms often handle both.

#### Types of Time

- ***Event Time***: When the event happened.
- ***Ingestion Time***: When it entered your system.
- ***Processing Time***: When it was transformed.

Track all to monitor delays and flow.

### Source Systems Practical Details

Here are some practical knowledge of APIs, databases, and data flow tools is essential but ever-changing—stay current.

#### **Relational Databases (RDBMS)**

Structured, ACID-compliant, great for transactional systems. They have tables, foreign keys, normalization, etc.

Examples would be [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/), [SQL Server](https://www.microsoft.com/en-us/sql-server/), [Oracle DB](https://www.oracle.com/database/) etc.

#### **NoSQL Databases**

Flexible, horizontally scalable databases with different data models.

##### **Key-Value Stores**

Fast read/write using unique keys. Great for caching or real-time event storage.

Examples would be [Redis](https://redis.io/), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) etc.  

##### **Document Stores**

Schema-flexible, store nested JSON-like documents.

Some examples are [MongoDB](https://www.mongodb.com/), [Couchbase](https://www.couchbase.com/), [Firebase Firestore](https://firebase.google.com/products/firestore) etc.

##### **Wide-Column Stores**

High-throughput databases that scale horizontally. They use column families and rows.

Some examples are [Cassandra](https://cassandra.apache.org/), [ScyllaDB](https://www.scylladb.com/), [Google Bigtable](https://cloud.google.com/bigtable).

##### **Graph Databases**  

Store nodes and edges. Ideal for analyzing relationships.

Examples could be given as [Neo4j](https://neo4j.com/), [Amazon Neptune](https://aws.amazon.com/neptune/), [ArangoDB](https://www.arangodb.com/) etc.

##### **Search Databases**

Fast search and text analysis engines. Common in logs and e-commerce.

Popular examples are [Elasticsearch](https://www.elastic.co/elasticsearch/), [Apache Solr](https://solr.apache.org/), [Algolia](https://www.algolia.com/).

##### **Time-Series Databases**

Optimized for time-stamped data: metrics, sensors, logs.

Some examples would be [InfluxDB](https://www.influxdata.com/), [TimescaleDB](https://www.timescale.com/), [Apache Druid](https://druid.apache.org/) etc.

#### **APIs**

Standard for data exchange across systems, especially over HTTP.

##### **REST (Representational State Transfer)**

Stateless API style using HTTP verbs (GET, POST, etc.). Widely adopted but loosely defined—developer experience varies.

An example would be the [GitHub REST API](https://docs.github.com/en/rest).

##### **GraphQL**

Made by Meta. Lets clients request exactly the data they need in one query—more flexible than REST.

Here is a link for the curious: [GitHub GraphQL API](https://docs.github.com/en/graphql).

##### **Webhooks**

Event-based callbacks from source systems to endpoints. Called *reverse APIs* because the server pushes data to the client.

[Stripe Webhooks](https://stripe.com/docs/webhooks) and [Slack Webhooks](https://api.slack.com/messaging/webhooks) are great examples.

##### **RPC / gRPC**

Run remote functions as if local. **gRPC** (by Google) uses Protocol Buffers and HTTP/2 for fast, efficient communication.

Check out [gRPC by Google](https://grpc.io/) for more.

#### And More

There are more details about Data Sharing, Third-Party Data Sources, Message Queues and Event-Streaming Platforms.

### Summary

Now we have a baseline for understanding source systems. The details matter.

We should work closely with app teams to improve data quality, anticipate changes, and build better data products. Collaboration leads to shared success—especially with trends like reverse ETL and event-driven architectures.

Making source teams part of the data journey is also a great idea. 

Next: storing the data.

> One additional note: Ideally our systems should be **idempotent**. An idempotent system produces the same result whether a message is processed once or multiple times—crucial for handling retries safely.

---
