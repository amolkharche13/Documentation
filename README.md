
### **SUSE Observability Kubernetes Deployment: Non-HA vs. HA Profiles**

The SUSE Observability platform supports two deployment profiles: **Non-High Availability (Non-HA)** and **High Availability (HA)**. These profiles determine the level of fault tolerance, scalability, and resource distribution across the nodes. Below is a detailed comparison of the pod configurations for each profile:

To gain a better understanding of each pod function in SUSE Observability, please review the following link: [Overview of Subsystems](https://docs.stackstate.com/self-hosted-setup/install-stackstate/troubleshooting/advanced-troubleshooting#overview-of-subsystems).

#### **Non-High Availability (Non-HA)**
In a Non-HA setup, all services are designed to run as single instances, making the deployment lightweight and suitable for environments where high availability is not a critical requirement. Key characteristics include:

- Single instance of core components such as Elasticsearch, Kafka, Zookeeper, and Clickhouse.
- Minimal redundancy across the system, simplifying resource allocation and management.
- Example of pods in a Non-HA setup:
  - `suse-observability-clickhouse-shard0-0`
  - `suse-observability-elasticsearch-master-0`
  - `suse-observability-kafka-0`
  - `suse-observability-zookeeper-0`
  - `suse-observability-ui-xxx`
<details>
  <summary>Click to expand pods output in Non-HA setup</summary>

  ```bash
NAME                                                              READY   STATUS    RESTARTS        AGE
suse-observability-clickhouse-shard0-0                            2/2     Running   0               2d22h
suse-observability-correlate-d5dc56d8b-5t6lf                      1/1     Running   0               2d18h
suse-observability-e2es-754f9c858-sjfz6                           1/1     Running   0               2d22h
suse-observability-elasticsearch-master-0                         1/1     Running   0               2d22h
suse-observability-hbase-stackgraph-0                             1/1     Running   0               2d22h
suse-observability-hbase-tephra-0                                 1/1     Running   0               2d22h
suse-observability-kafka-0                                        2/2     Running   0               2d22h
suse-observability-kafkaup-operator-kafkaup-7c57c5bdbf-zksfr      1/1     Running   0               2d22h
suse-observability-otel-collector-0                               1/1     Running   0               2d22h
suse-observability-prometheus-elasticsearch-exporter-765cd6w5sl   1/1     Running   0               2d22h
suse-observability-receiver-759754bb48-ttbg7                      1/1     Running   0               2d18h
suse-observability-router-7cbfd778d9-hkckv                        1/1     Running   0               2d22h
suse-observability-server-86f4649d4b-r82tp                        1/1     Running   0               2d18h
suse-observability-ui-56c7df6cd7-qlss2                            2/2     Running   0               2d22h
suse-observability-victoria-metrics-0-0                           1/1     Running   0               2d22h
suse-observability-vmagent-0                                      1/1     Running   0               2d22h
suse-observability-zookeeper-0                                    1/1     Running   0               2d22h

```
</details>

#### **High Availability (HA)**
In an HA deployment, the architecture introduces redundancy and scalability to ensure continuous service availability and handle larger workloads. 
The HA profile includes multiple instances of critical components, enabling the system to withstand node or pod failures. Key features include:

- **Elasticsearch:** Operates with three master nodes (`elasticsearch-master-0`, `elasticsearch-master-1`, `elasticsearch-master-2`) to maintain quorum.
- **Kafka and Zookeeper:** Three replicas each, ensuring high throughput and resiliency.
- **HBase and HDFS:** Includes multiple region servers, data nodes, and master nodes to distribute data and maintain uptime.
- **suse-observability-server-xxxx** will be replaced by `suse-observability-notification-xxx`,`suse-observability-slicing-xxx` and `suse-observability-initializer-xxx` in HA setup.
- ** Other components are in HA, such as:
  - `suse-observability-health-sync`
  - `suse-observability-receiver-logs`, `receiver-process-agent`, and `receiver-base` for better load segregation.

**Example pods in HA:**
- `suse-observability-elasticsearch-master-[0, 1, 2]`
- `suse-observability-kafka-[0, 1, 2]`
- `suse-observability-zookeeper-[0, 1, 2]`:Zookeeper is used for service discovery, orchestration and failover. Zookeeper is deployed using 1 or more pods with the name:
  - `suse-observability-zookeeper-<n>`
- `suse-observability-hbase-hbase-rs-[0, 1, 2]`
- `suse-observability-ui-[multiple instances]`

<details>
  <summary>Click to expand pods output in HA setup</summary>

  ```bash
  NAME                                                              READY   STATUS    RESTARTS      AGE
  suse-observability-agent-checks-agent-95ff66f4c-l974n             1/1     Running   0             10m
  suse-observability-agent-cluster-agent-7b85bc7857-kv5fw           1/1     Running   0             10m
  suse-observability-agent-logs-agent-4ppw7                         1/1     Running   0             10m
  suse-observability-agent-logs-agent-x9jn8                         1/1     Running   0             10m
  suse-observability-agent-node-agent-8nf87                         2/2     Running   0             10m
  suse-observability-agent-node-agent-gvqdp                         2/2     Running   0             10m
  suse-observability-api-7d796dcf9b-qkf9q                           1/1     Running   0             36m
  suse-observability-checks-589488bd64-sfw6j                        1/1     Running   0             15m
  suse-observability-clickhouse-shard0-0                            2/2     Running   0             35m
  suse-observability-correlate-5cbbb98d46-pwdx6                     1/1     Running   0             33m
  suse-observability-correlate-5cbbb98d46-rjfl5                     1/1     Running   0             36m
  suse-observability-correlate-5cbbb98d46-tcpn6                     1/1     Running   0             32m
  suse-observability-e2es-7649f4fdb4-st4rx                          1/1     Running   0             36m
  suse-observability-elasticsearch-master-0                         1/1     Running   0             15m
  suse-observability-elasticsearch-master-1                         1/1     Running   0             15m
  suse-observability-elasticsearch-master-2                         1/1     Running   0             15m
  suse-observability-hbase-hbase-master-0                           1/1     Running   0             34m
  suse-observability-hbase-hbase-master-1                           1/1     Running   0             35m
  suse-observability-hbase-hbase-rs-0                               1/1     Running   0             34m
  suse-observability-hbase-hbase-rs-1                               1/1     Running   0             34m
  suse-observability-hbase-hbase-rs-2                               1/1     Running   0             35m
  suse-observability-hbase-hdfs-dn-0                                1/1     Running   0             32m
  suse-observability-hbase-hdfs-dn-1                                1/1     Running   0             33m
  suse-observability-hbase-hdfs-dn-2                                1/1     Running   0             35m
  suse-observability-hbase-hdfs-nn-0                                1/1     Running   0             34m
  suse-observability-hbase-hdfs-snn-0                               1/1     Running   0             34m
  suse-observability-hbase-tephra-0                                 1/1     Running   0             34m
  suse-observability-hbase-tephra-1                                 1/1     Running   0             35m
  suse-observability-health-sync-8cb88d7d-c62j4                     1/1     Running   0             15m
  suse-observability-initializer-7d487dffb7-l858s                   1/1     Running   0             35m
  suse-observability-kafka-0                                        2/2     Running   0             33m
  suse-observability-kafka-1                                        2/2     Running   0             34m
  suse-observability-kafka-2                                        2/2     Running   0             35m
  suse-observability-kafkaup-operator-kafkaup-7c57c5bdbf-j8hlr      1/1     Running   0             43m
  suse-observability-notification-5d999c4d5b-4j8s5                  1/1     Running   0             35m
  suse-observability-otel-collector-0                               1/1     Running   0             43m
  suse-observability-prometheus-elasticsearch-exporter-54cc6lbhf5   1/1     Running   0             43m
  suse-observability-receiver-base-54b66d775d-q6ftf                 1/1     Running   0             36m
  suse-observability-receiver-logs-6d557fd5f5-bv4tt                 1/1     Running   0             36m
  suse-observability-receiver-process-agent-7d4f8bc7c5-7bc7m        1/1     Running   0             36m
  suse-observability-router-bd8d7b544-68dr8                         1/1     Running   0             43m
  suse-observability-slicing-74bfddd9b9-wnh6q                       1/1     Running   0             35m
  suse-observability-state-5d9bd7f694-ddzzl                         1/1     Running   0             36m
  suse-observability-sync-7fbb946b4b-mddrm                          1/1     Running   0             36m
  suse-observability-ui-7c548d877c-48ndx                            2/2     Running   0             43m
  suse-observability-ui-7c548d877c-7ss4g                            2/2     Running   0             43m
  suse-observability-victoria-metrics-0-0                           1/1     Running   0             35m
  suse-observability-victoria-metrics-1-0                           1/1     Running   0             21m
  suse-observability-vmagent-0                                      1/1     Running   0             22m
  suse-observability-zookeeper-0                                    1/1     Running   0             35m
  suse-observability-zookeeper-1                                    1/1     Running   0             35m
  suse-observability-zookeeper-2                                    1/1     Running   0             35m

```
</details>

#### **Comparison Summary**

| Feature                        | Non-HA Deployment                 | HA Deployment                      |
|--------------------------------|------------------------------------|------------------------------------|
| **Scalability**                | Limited                           | Highly scalable                    |
| **Redundancy**                 | None                              | Multiple replicas for core services |
| **Resilience to Failures**     | Low                               | High                               |
| **Resource Requirements**      | Minimal                           | High                               |
| **Example Pods**               | `kafka-0`, `zookeeper-0`          | `kafka-[0, 1, 2]`, `zookeeper-[0, 1, 2]` |


####  
If you're planning to deploy SUSE Observability in a production environment, the HA setup is strongly recommended. It provides the necessary redundancy and fault tolerance to ensure that your observability platform remains operational even in the face of failures.
