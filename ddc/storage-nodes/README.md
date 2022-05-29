# 🌐 Storage Nodes

DDC Storage Node is a core component of DDC that is responsible for storing and retrieving the data. It’s written in Go and consists of a large set of components and modules, which are shown in the high level architecture diagram below.

DDC nodes operations are coordinated and rewarded through smart contracts running on the Cere test/main blockchains.

{% hint style="info" %}
**For more details, see the documentation of DDC Storage nodes:**

****[**https://cerebellum-network.github.io/ddc-storage-node/**](https://cerebellum-network.github.io/ddc-storage-node/)
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
