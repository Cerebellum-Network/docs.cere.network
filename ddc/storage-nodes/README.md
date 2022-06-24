# 🌐 Storage Service

DDC offers clusters with storage services which persists data safely and allows searching and retrieving data.
A core component of the storage service is the DDC storage node which is responsible for partitions of the data. Safety and scalability is achieved by spreading the data over a number of nodes.

{% hint style="info" %}
**For more details, see the documentation of DDC Storage nodes:**

[**https://cerebellum-network.github.io/ddc-storage-node/**](https://cerebellum-network.github.io/ddc-storage-node/)
{% endhint %}

## Recovery

Every  DDC storage has Merkle DAG which presents tree of stored files. When some node inconsistent, storage node can recover data by using his own Merkle DAG and replica nodes for searching required files and copying.

## Implementation

{% embed url="https://github.com/Cerebellum-Network/ddc-storage-node" %}

## Metrics

Storage node provides metrics for monitoring:

* general HTTP metrics
* stored data in bytes
* stored piece number
