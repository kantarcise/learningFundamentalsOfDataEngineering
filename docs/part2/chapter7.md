## 7. Ingestion

***Ingestion*** is the process of moving data from source systems into storage—it's the first step in the data engineering lifecycle after data is generated.

> Quick definition, **data ingestion** is data movement from point A to B, **data integration** combines data from disparate sources into a new dataset. Example of data integration is a CRM system, advertising analytics data, and web analytics to make a user profile, which is saved to our data warehouse.

A **data pipeline** is the full system that moves data through the data engineering lifecycle. Design of data pipelines typically starts at the ingestion stage.

### What to Consider when Building Ingestion? 🤔

Consider these factors when designing your ingestion architecture:

#### Bounded vs. Unbounded

All data is **unbounded until constrained**. Streaming preserves natural flow; batching adds structure.

#### Frequency

Choose between **batch**, **micro-batch**, or **real-time** ingestion. "Real-time" typically means low-latency, near real-time.

#### Synchronous vs. Asynchronous

- **Synchronous**: Tightly coupled; failure in one stage stops all.  
- **Asynchronous**: Loosely coupled; stages operate independently and more resiliently.

#### Serialization & Deserialization

Data must be encoded before transfer and properly decoded at destination. Incompatible formats make data unusable.

#### Throughput & Scalability

Design to handle spikes and backlogs. Use buffering and managed services (e.g., [Kafka](https://kafka.apache.org/), [Kinesis](https://aws.amazon.com/kinesis/)) for elasticity.

#### ⏳ Reliability & Durability

Ensure uptime and no data loss through redundancy and failover. Balance cost vs. risk—design for resilience within reason.

#### 🗃 Payload

Let's understand data characteristics:

- **Kind**: tabular, image, video, text
- **Shape**: dimensions like rows/columns or RGB pixels
- **Size**: total bytes; may need to chunk or compress
- **Schema**: field types and structure
- **Metadata**: context and descriptors for your data

#### Push vs. Pull vs. Poll

- **Push**: Source sends data to the pipeline
- **Pull**: System fetches from the source
- **Poll**: Regular checks for updates, then pulls

And here are some additional insight.

#### 🔄 Streaming + Batch Coexist

Even with real-time ingestion, batch steps are common (e.g., model training, reports). Expect a hybrid approach.

#### 🧱 Schema Awareness

Schemas change—new columns, types, or renames can silently break pipelines. Use **schema registries** to version and manage schemas reliably.

#### 🗂️ Metadata Mattera

Without rich metadata, raw data can become a data swamp. Proper tagging and descriptions are critical for usability.

### Batch Ingestion

If we went with batch way, here are some things to keep in mind. Batch ingestion moves data in bulk, usually based on a time interval or data size. It’s widely used in traditional ETL and for transferring data from stream processors to long-term storage like data lakes.

#### Snapshot vs. Differential Extraction

- **Snapshot**: Captures the full state each time. Simple but storage-heavy.
- **Differential**: Ingests only changes since the last read. More efficient.

#### File-Based Export and Ingestion

- Source systems export data as files (e.g., CSV, Parquet), then push to the target.
- Avoids direct access to sensitive databases.
- Exchange methods: **S3**, **SFTP**, **EDI**, **SCP**.

#### ETL vs. ELT

We defined ETL before. In ELT the definition is as follows:

- **Extract**: Pull data from various source systems (e.g., databases, APIs, logs).
- **Load**: Move the raw data directly into a central data store (like a cloud data warehouse or data lake).
- **Transform**: Perform data cleaning, enrichment, and modeling after the data is loaded—within the storage system itself.

Choose based on system capabilities and transformation complexity.

#### 📥 Inserts, Updates, and Batch Size

- Avoid many small inserts—use bulk operations for better performance.
- Some systems (like [Druid](https://druid.apache.org/), [Pinot]()) handle fast inserts well.
- Columnar databases (e.g., [BigQuery](https://cloud.google.com/bigquery?hl=en)) prefer larger batch loads over frequent single-row inserts.
- Understand how your target system handles updates and file creation.

#### 🔄 Data Migration

Large migrations (TBs+) involve moving full tables or entire systems.

Key challenges are:

- Schema compatibility (e.g., SQL Server → Snowflake)

- Moving pipeline connections to the new environment

Use staging via object storage and test sample loads before full migration. Also consider migration tools instead of writing from scratch.

### 📨 Message and Stream Ingestion

Event-based ingestion is common in modern architectures. This section covers best practices and challenges to watch for when working with streaming data.

#### 🧬 Schema Evolution

Schema changes (added fields, type changes) can break pipelines. 

Here is what you can do:

- Use **schema registries** to version schemas.
- Set up **dead-letter queues** for unprocessable events.
- **Communicate** with upstream teams about upcoming changes.

#### 🕓 Late-Arriving Data

- Some events arrive later than expected due to network delays or offline devices.
- Always **distinguish event time from ingestion time**.
- Define **cutoff rules** for how long you will accept late data.

#### 🔁 Ordering & Duplicate Delivery

- Distributed systems can cause **out-of-order** and **duplicate** messages.
- Most systems (e.g., [Kafka](https://kafka.apache.org/)) guarantee **at-least-once** delivery.
- Build systems that can handle duplicates gracefully (e.g., idempotent processing). 🎉

#### ⏪ Replay

Replay lets you reprocess historical events within a time range.

- Platforms like **[Kafka](https://kafka.apache.org/)**, **Kinesis**, and **Pub/Sub** support this.
- This is really useful for recovery, debugging, or rebuilding pipelines.

#### ⏳ Time to Live (TTL)

TTL defines how long events are retained before being discarded. It's the maximum message retention time, which is helpful to reduce backpressure.

Short TTLs can cause data loss; long TTLs can create backlogs.

Examples:

- **[Pub/Sub](https://cloud.google.com/pubsub/docs/overview)**: up to 7 days
- **[Kinesis](https://aws.amazon.com/kinesis/)**: up to 365 days
- **[Kafka](https://kafka.apache.org/)**: configurable, even indefinite with object storage

#### 📏 Message Size

Be mindful of max size limits:

- **[Kinesis](https://aws.amazon.com/kinesis/)**: 1 MB
- **[Kafka](https://kafka.apache.org/)**: configurable (default 1 MB, up to 20+ MB)

#### 🧯 Error Handling & Dead-Letter Queues

Invalid or oversized messages should be routed to a **dead-letter queue**.

- This prevents bad events from blocking pipeline processing.
- Allows investigation and optional reprocessing after fixes.

#### 🔄 Consumer Models: Pull vs. Push

Pull is default for data engineering; push is used for specialized needs.

- **Pull**: Consumers fetch data from a topic ([Kafka](https://kafka.apache.org/), [Kinesis](https://aws.amazon.com/kinesis/)).
- **Push**: Stream pushes data to a listener ([Pub/Sub](https://cloud.google.com/pubsub/docs/overview), [RabbitMQ](https://www.rabbitmq.com/)).

#### 🌍 Ingestion Location & Latency

- Ingesting data **close to where it's generated** reduces latency and bandwidth cost.
- Multiregional setups improve resilience but increase complexity and egress cost.
- Balance **latency, cost, and performance** carefully.

### 📥 Ways to Ingest Data

There are many ways to ingest data—each with its own trade-offs depending on the source system, use case, and infrastructure setup.

#### 🧩 Direct Database Connections (JDBC/ODBC)

**JDBC/ODBC** are standard interfaces for pulling data directly from databases.

JDBC is Java-native and widely portable; ODBC is system-specific. These connections can be parallelized for performance but are row-based and struggle with nested/columnar data.

Many modern systems now favor **native file export** (e.g., Parquet) or REST APIs instead.

#### 🔄 Change Data Capture (CDC)

Here are some quick definitions on CDC:

- **Batch CDC**: Queries updated records since the last read using timestamps.
- **Continuous CDC**: Uses database logs to stream all changes in near real-time.
- CDC supports real-time replication and analytics but must be carefully managed to avoid overloading the source.
- **Synchronous replication** keeps replicas tightly synced; **asynchronous CDC** offers more flexibility.

#### 🌐 APIs

APIs are a common ingestion method from external systems.

- Use **client libraries**, **connector platforms**, or **data sharing** features to avoid building everything from scratch.
- For unsupported APIs, follow best practices for custom connectors and use orchestration tools for reliability.

#### 📨 Message Queues & Event Streams

Use systems like **[Kafka](https://kafka.apache.org/), [Kinesis](https://aws.amazon.com/kinesis/)**, or **[Pub/Sub](https://cloud.google.com/pubsub/docs/overview)** to ingest real-time event data.

- ***Queues*** are transient (message disappears after read); ***streams*** persist and support replays, joins, and aggregations.

Design for **low latency**, **high throughput**, and consider **autoscaling** or managed services to reduce ops burden.

#### 🔌 Managed Data Connectors

Services like **[Fivetran](https://www.fivetran.com/), [Airbyte](https://airbyte.com/)**, and **[Stitch](https://www.stitchdata.com/)** provide plug-and-play connectors.

- These services manage syncs, retries, schema detection, and alerting.
- This makes them ideal for reducing boilerplate and saving engineering time.

#### 🪣 Object Storage

Object storage (e.g., **[S3](https://aws.amazon.com/s3/), [GCS](https://cloud.google.com/storage?hl=en), [Azure Blob](https://azure.microsoft.com/en-us/products/storage/blobs)**) is great for moving files between teams and systems.

Use signed URLs for temporary access and treat object stores as secure staging zones for data.

#### 💾 EDI (Electronic Data Interchange)

This is a legacy format still common in business environments.

- Automate handling via email ingestion, file drops, or intermediate object storage when modern APIs aren’t available.

#### 📤 File Exports from Databases

Large exports put load on source systems—use **read replicas** or **key-based partitioning** for efficiency.

Modern cloud data warehouses support **direct export to object storage** in formats like Parquet or ORC.

#### 🧾 File Formats & Considerations

Avoid CSV when possible due to its lack of schema, support for nested data, and error-prone behavior.

Prefer **Parquet, ORC, Avro, Arrow, JSON**—which support schema and complex structures.

#### 💻 Shell, SSH, SCP, and SFTP

Shell scripts and CLI tools still play a big role in scripting ingestion pipelines.

- Use **SSH tunneling** for secure access to remote databases.
- **SFTP/SCP** are often used for legacy integrations or working with partner systems.

#### 📡 Webhooks

**Webhooks** are "Reverse API" where the data provider pushes data to your service.

- Typically used for event-based data (e.g., Stripe, GitHub).
- Build reliable ingestion using **serverless functions**, queues, and **stream processors**.

#### 🌐 Web Interfaces & Scraping

- Some SaaS tools still require **manual download** via a browser.
- **Web scraping** can be used for unstructured data extraction, but comes with ethical, legal, and operational challenges.

#### 🚚 Transfer Appliances

- For large-scale migrations (100 TB+), physical **data transfer appliances** like [AWS Snowball](https://aws.amazon.com/tr/snowball/) are used.
- Faster and cheaper than network transfer for petabyte-scale data moves.

#### 🤝 Data Sharing

Platforms like **Snowflake, [BigQuery](https://cloud.google.com/bigquery?hl=en), Redshift**, and **[S3](https://aws.amazon.com/s3/)** allow **read-only data sharing**.

- This is useful for integrating third-party or vendor-provided datasets into your analytics without owning the storage.

### Summary 🥳

Here are some quotes from the book.

> Moving data introduces security vulnerabilities because you have to transfer data between locations. Data that needs to move within your VPC should use secure endpoints and never leave the confines of the VPC.

> Do not collect the data you don't need. Data cannot leak if it is never collected.

My summary is down below:

Ingestion is the stage in the data engineering lifecycle where raw data is moved from source systems into storage or processing systems. 

Data can be ingested in batch or streaming modes, depending on the use case. ***Batch ingestion*** processes large chunks of data at set intervals or based on file size, making it ideal for daily reports or large-scale migrations. ***Streaming ingestion***, on the other hand, continuously processes data as it arrives, making it suitable for real-time applications like IoT, event tracking, or transaction streams. 

Designing ingestion systems involves careful consideration of factors like bounded vs. unbounded data, frequency, serialization, throughput, reliability, and the push/pull method of data retrieval.

Ingestion isn’t just about moving data—it’s about understanding the shape, schema, and sensitivity of that data to ensure it's usable downstream.

As Data Engineers we must track metadata, consider ingestion frequency vs. transformation frequency, and apply best practices for security, compliance, and cost. 

We should also stay flexible: even legacy methods like EDI or manual downloads may still be part of real-world workflows. 

The key is to choose ingestion patterns that match the needs of the business while staying robust, scalable, and future-proof.

---