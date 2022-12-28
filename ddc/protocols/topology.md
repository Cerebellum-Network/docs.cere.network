# 🕸 Network Topology

## Exploring the network

The entry point into the DDC network is the main smart contract on the Cere blockchain.
From there, any participant can fully explore the DDC network and discover the location of all nodes and all data.
Notably, the DDC does not use a gossip protocol; instead, all information can be found by following
identifiers and URLs found in several data structures described here.

Starting from scratch, the first object that a client software needs to find is a `Cluster`, which describes a particular service. The full list of Clusters can be queried from the contract. The client must know or choose the right Cluster, identified by a `cluster_id`. This can be one of the clusters recommended by Cere
(the default in SDKs, [testnet](/testnet/ddc-network-testnet.md), [mainnet](/mainnet/ddc-network.md)),
or another as recommended by someone else such as the developers of a given app.

From there, the client can lookup the current state of the Cluster. This includes some configuration about the service, the costs, and the rules of validation. This also keeps track of the nodes currently offering this service. Services are usually partitioned and replicated, and the client can discover which nodes are replicas of which partitions; see [TopologyType](contract-params-schema.md#topologytype).

After choosing a node, the client can read its current URL, connect to it, and use the protocol of the service (e.g., the [CDN API](cdn-api.md)) to upload, search, or download data.

**For details, see the [contract API](smart-contract-api.md) and the related [data structures](contract-params-schema.md).**
