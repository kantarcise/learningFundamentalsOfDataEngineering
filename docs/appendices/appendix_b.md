## Appendix B. Cloud Networking

Data engineers must understand cloud networking basics to design performant and cost-efficient systems.

Cloud networks impact **latency**, *cost* (especially due to data egress fees), and **system architecture**.

### Key Concepts

#### Network Topology & Resource Hierarchy

Public clouds (AWS, GCP, Azure) follow similar structures: zones (smallest unit), regions (group of zones), and in GCP’s case, multiregions (group of regions). Engineers must align data systems with this topology for high performance and resilience.

#### Data Egress Fees

Clouds allow free inbound traffic but charge for outbound traffic, especially across regions or to the internet. This pricing model can create vendor lock-in and affect architecture choices.

Direct connections or CDNs can reduce costs.

#### Zones vs. Regions

- Zones offer low latency and free traffic (within private IPs). Use single-zone deployments for high-throughput clusters when possible.

- Regions consist of independent zones. Running across zones adds resilience but incurs slight latency and cost increases.

- Multiregions (GCP) enable geo-redundant storage with no inter-zone egress fees, simplifying disaster recovery.

####  GCP’s Premium Networking

Google offers premium-tier networking, where inter-region traffic stays on its private network, improving reliability and speed.

#### Direct Connect

Providers like AWS, Azure, and GCP offer direct network connections (e.g., AWS Direct Connect), lowering latency and significantly cutting egress costs—e.g., 9¢/GB to 2¢/GB.

#### CDNs (Content Delivery Networks)

CDNs like Cloudflare and cloud-native options cache data closer to users, improving delivery speed and reducing load on origin servers. However, their availability varies by region and political factors.

#### The Future of Data Egress

Data egress fees restrict cloud portability and multi-cloud adoption.

Competitive pressure and customer demand may push providers to reduce or eliminate egress fees in the near future, just as telecom pricing models evolved.

### Takeaway

Cloud networking shapes system performance, resilience, and cost.

Data engineers must be aware of how their data moves within and across zones, regions, and providers—and should design architectures that balance latency, cost, and reliability while keeping an eye on evolving cloud pricing models.

---

