
**SUSE Observability Kubernetes Deployment: Non-HA vs. HA Profiles**

The SUSE Observability platform supports two deployment profiles: **Non-High Availability (Non-HA)** and **High Availability (HA)**. These profiles determine the level of fault tolerance, scalability, and resource distribution across the platform components. Below is a detailed comparison of the pod configurations for each profile:

### **Non-High Availability (Non-HA)**
In a Non-HA setup, all services are designed to run as single instances, making the deployment lightweight and suitable for environments where high availability is not a critical requirement. Key characteristics include:

- Single instance of core components such as Elasticsearch, Kafka, Zookeeper, and Clickhouse.
- Minimal redundancy across the system, simplifying resource allocation and management.
- Example of pods in a Non-HA setup:
  - `suse-observability-clickhouse-shard0-0`
  - `suse-observability-elasticsearch-master-0`
  - `suse-observability-kafka-0`
  - `suse-observability-zookeeper-0`
  - `suse-observability-ui-56c7df6cd7-qlss2`

### **High Availability (HA)**
In an HA deployment, the architecture introduces redundancy and scalability to ensure continuous service availability and handle larger workloads. The HA profile includes multiple instances of critical components, enabling the system to withstand node or pod failures. Key features include:

- **Elasticsearch:** Operates with three master nodes (`elasticsearch-master-0`, `elasticsearch-master-1`, `elasticsearch-master-2`) to maintain quorum.
- **Kafka and Zookeeper:** Three replicas each, ensuring high throughput and resiliency.
- **HBase and HDFS:** Includes multiple region servers, data nodes, and master nodes to distribute data and maintain uptime.
- **suse-observability-server-xxxx** will be replaced by suse-observability-notification-xxx,suse-observability-slicing-xxx and suse-observability-initializer-xxx.
- ** Other components are in HA, such as:
  - `suse-observability-health-sync`
  - `suse-observability-receiver-logs`, `receiver-process-agent`, and `receiver-base` for better load segregation.

**Example pods in HA:**
- `suse-observability-elasticsearch-master-[0, 1, 2]`
- `suse-observability-kafka-[0, 1, 2]`
- `suse-observability-zookeeper-[0, 1, 2]`
- `suse-observability-hbase-hbase-rs-[0, 1, 2]`
- `suse-observability-ui-[multiple instances]`

### **Comparison Summary**

| Feature                        | Non-HA Deployment                 | HA Deployment                      |
|--------------------------------|------------------------------------|------------------------------------|
| **Scalability**                | Limited                           | Highly scalable                    |
| **Redundancy**                 | None                              | Multiple replicas for core services |
| **Resilience to Failures**     | Low                               | High                               |
| **Resource Requirements**      | Minimal                           | High                               |
| **Example Pods**               | `kafka-0`, `zookeeper-0`          | `kafka-[0, 1, 2]`, `zookeeper-[0, 1, 2]` |

#### If you're planning to deploy SUSE Observability in a production environment, the HA setup is strongly recommended. It provides the necessary redundancy and fault tolerance to ensure that your observability platform remains operational even in the face of failures.
