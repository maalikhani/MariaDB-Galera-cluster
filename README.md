# MariaDB-Galera-cluster
A High-Availability MySQL/MariaDB Galera Cluster designed for resilient, multi-master replication.
This lab demonstrates a 3-node cluster with automatic node joining, synchronous replication, and failover handling.

⚡ Features

Multi-Master HA: All nodes can serve read/write traffic.

Synchronous replication: Ensures data consistency across nodes.

Automatic node recovery: Failed nodes rejoin the cluster automatically.

Load balancing: HAProxy handles query distribution and failover.

Ready-to-run lab setup: Example configuration for 3 nodes.

🏗 Updated Architecture
         ```text
           +---------------------+
           |   HAProxy / ProxySQL|
           | Load Balancer / LB  |
           | 192.168.220.150     |
           +----------+----------+
                      |
     +----------------+----------------+
     |                |                |
     v                v                v
+--------+       +--------+       +--------+
| db1    |       | db2    |       | db3    |
|192.168 |       |192.168 |       |192.168 |
|220.143 |       |220.144 |       |220.145 |
+--------+       +--------+       +--------+
       \           |           /
        \ Synchronous Replication /


Each node is equal in the cluster; there is no single point of failure for writes.

📂 Repository Structure
galera-cluster-lab/
├─ galera/
│  ├─ node1.cnf        # Node-specific configuration
│  ├─ node2.cnf
│  └─ node3.cnf
├─ haproxy/
│  └─ haproxy.cfg      # Load balancer configuration
├─ docs/
│  └─ setup-guide.md   # Step-by-step deployment instructions
└─ README.md           # This file

⚙ Requirements

Linux (tested on Rocky 9 / Ubuntu 22.04)

MySQL ≥ 8.0 or MariaDB ≥ 10.6

Galera ≥ 4.0

HAProxy ≥ 2.4

📝 Best Practices

Maintain quorum: At least 2 out of 3 nodes must be online for writes.

Use stable hardware; avoid ephemeral or unstable VMs in production.

Tune key parameters:

innodb_buffer_pool_size → memory allocation

wsrep_provider_options → replication tuning

innodb_flush_log_at_trx_commit → balance durability vs performance

Monitor cluster health using Prometheus + Grafana.

Enable automated backups and regular sst/ist testing.

Consider separating read and write workloads via HAProxy for performance optimization.

⚡ Notes

Galera supports true multi-master replication, but network partitioning can cause write conflicts.

HAProxy ensures that traffic is routed only to healthy nodes, minimizing downtime.

Node recovery is automatic, but for production, always validate SST/IST configurations before scaling.

