## 3. Designing Good Data Architecture 🎋

What is it? Here is a definition:

- **Data architecture** is the design of systems to support the evolving data needs of an enterprise, achieved by flexible and reversible decisions reached through a careful evaluation of trade-offs.

We can divide Data Architecture into two parts, ***Operational*** and ***Technical***:

***Operational architecture*** involves the practical needs related to people, processes, and technology. For example, it looks at which business activities the data supports, how the company maintains data quality, and how quickly data needs to be available for use after it's created. 

***Technical architecture*** explains the methods for collecting, storing, changing, and delivering data throughout its lifecycle. For example, it might describe how to move 10 TB of data every hour from a source database to a data lake.

In short, operational architecture defines *what needs to be done*, while technical architecture explains *how to do it*.

Effective data architecture meets business needs by using standardized, reusable components while remaining adaptable and balancing necessary compromises. It's also dynamic and continually evolving. It is never truly complete. 

By definition, adaptability and growth are fundamental to the essence and objectives of data architecture.

Next, let's explore the principles that underpin good data architecture.

### Principles of Good Data Architecture

Here are 9 principles to keep in mind.

#### 1: Choose Common Components Wisely

A key responsibility of data engineers is selecting shared components and practices—such as ***object storage***, ***version control systems***, ***observability tools***, ***orchestration platforms***, and ***processing engines***—that are widely usable across the organization.

Effective selection promotes collaboration, breaks down silos, and enhances flexibility by leveraging common knowledge and skills. These shared tools should be accessible to all relevant teams, encouraging the use of existing solutions over creating new ones, while ensuring robust permissions and security to safely share resources.

Cloud platforms are ideal for implementing these components, allowing teams to access a common storage layer with specialized tools for their specific needs.

Balancing organizational-wide requirements with the flexibility for specialized tasks is essential to support various projects and foster collaboration without imposing one-size-fits-all solutions.

Further details are provided in Chapter 4.

#### 2: Plan for Failure

Modern hardware is generally reliable, but failures are inevitable over time. 

To build robust data systems, it's essential to design with potential failures in mind by understanding key concepts such as ***availability*** (the percentage of time a service is operational), ***reliability*** (the likelihood a system performs its intended function), ***recovery time objective*** (the maximum acceptable downtime), and ***recovery point objective*** (the maximum acceptable data loss).

These factors guide engineers in making informed architectural decisions to effectively handle and mitigate failure scenarios, ensuring systems remain resilient and meet business requirements.

#### 3: Architect for Scalability

***Scalability*** in data systems means the ability to automatically increase capacity to handle large data volumes or temporary spikes and decrease it to reduce costs when demand drops. 

Elastic systems adjust dynamically, sometimes even scaling to zero when not needed, as seen in serverless architectures. However, choosing the right scaling strategy is essential to avoid complexity and high costs.

This requires carefully assessing current usage, anticipating future growth, and selecting appropriate database architectures to ensure efficiency and cost-effectiveness as the organization expands.

#### 4: Architecture Is Leadership

Data architects combine strong technical expertise with leadership and mentorship to make technology decisions, promote flexibility and innovation, and guide data engineers in achieving organizational goals.

It really helps to have a growth mindset.

#### 5: Always Be Architecting

Data architects continuously design and adapt architectures in an agile, collaborative way, responding to business and technology changes by planning and prioritizing updates.

> Innovation requires iteration. Mark Papermaster.

#### 6: Build Loosely Coupled Systems

***Loose coupling*** through independent components and APIs allows teams to collaborate efficiently and evolve systems flexibly.

#### 7: Make Reversible Decisions

To stay agile in a rapidly changing data landscape, architects should make reversible decisions that keep architectures simple and adaptable.

> You can read [this shareholder letter](https://www.sec.gov/Archives/edgar/data/1018724/000119312516530910/d168744dex991.htm) from Jeff Bezos on reversible decisions.

#### 8: Prioritize Security

Data engineers must take responsibility for system security by adopting [zero-trust models](https://www.microsoft.com/en-us/security/business/zero-trust) and the [shared responsibility](https://aws.amazon.com/compliance/shared-responsibility-model/) approach, ensuring robust protection in cloud-native environments and preventing breaches through proper configuration and proactive security practices.

#### 9: Embrace FinOps

***FinOps*** is a cloud financial management practice that encourages collaboration between engineering and finance teams to optimize cloud spending through data-driven decisions and continuous cost monitoring.

We should embrace it!

Now that we have a grasp of the fundamental principles of effective data architecture, let's explore the key concepts necessary for designing and building robust data systems in more detail.

### Major Architecture Concepts

To learn more about:

- Domains and Services
- Distributed Systems, Scalability, and Designing for Failure
- Tight Versus Loose Coupling: Tiers, Monoliths, and Microservices
- User Access: Single Versus Multitenant
- Event-Driven Architecture
- Brownfield Versus Greenfield Projects

please read this part. 🥰

Next, we’ll explore different types of architectures.

### Examples and Types of Data Architecture

Here, we can explore some 101 information about:

- ***Data Warehouse*** – Centralized, structured, query-optimized storage.

- ***Data Marts*** – Department-specific subsets of warehouse data.

- ***Data Lake*** – Raw, unstructured data stored at scale.

- ***Data Lakehouses*** – Data lake + warehouse features combined.

- ***The Modern Data Stack*** – Cloud-native, modular data tooling ecosystem.

- ***Lambda Architecture*** – Combines batch and real-time processing.

- ***Kappa Architecture*** – Streaming-only alternative to Lambda.

- ***Unified Batch and Streaming*** – One engine for all data flows.

- ***IoT Architecture*** – Real-time pipelines for connected devices.

- ***Data Mesh*** – Decentralized, domain-owned data architecture.

which is foundational knowledge on which what we'll build after.

### Conclusion

Architectural design involves close collaboration with business teams to weigh different options.

For instance:

- How does choosing a cloud data warehouse compare to implementing a data lake?

- What considerations come into play when selecting between cloud providers? 

- Under what circumstances would a unified processing system like Flink or Beam make sense? 

Gaining a strong grasp of these decision points will equip us to make sound, reasonable choices.

Next, we’ll explore approaches to selecting the right technologies for our data architecture and throughout the data engineering lifecycle. 😍

---

