# 🕸 Topology Schema

## Schema of the parameters in the DDC bucket contract

This is the data model used to configure Clusters, Nodes, and Buckets in the [ddc-bucket smart contract](https://github.com/Cerebellum-Network/ddc-bucket-contract).

### Bucket Parameters

Bucket parameters specify:

* How data should be replicated.

### Cluster Parameters

Cluster parameters specify:

* What type of nodes can be included in a Cluster.
* How data and requests should be routed to Nodes.
* Any additional details given by the Cluster manager.

### Node Parameters

Node parameters specify:

* What engine a Node is running.
* How to connect to a physical Node that provides the service offered in the contract.
* Any additional details given by a Provider about their node.

([Source](https://github.com/Cerebellum-Network/ddc-schemas))

### Changelog

#### vNext

#### v0.1.3

* Basic Bucket parameters with replication factor.
* Basic Cluster parameters with engine name, version, and the uniform topology.
* Basic Node parameters with the Node URL and engine version.

## Protocol Documentation

### Table of Contents

* [bucket-params.proto](contract-params-schema.md#bucket-params.proto)
  * [BucketParams](contract-params-schema.md#pb.BucketParams)
* [cluster-params.proto](contract-params-schema.md#cluster-params.proto)
  * [ClusterParams](contract-params-schema.md#pb.ClusterParams)
  * [TopologyType](contract-params-schema.md#pb.TopologyType)
* [node-params.proto](contract-params-schema.md#node-params.proto)
  * [NodeParams](contract-params-schema.md#pb.NodeParams)
  * [EngineType](contract-params-schema.md#pb.EngineType)
* [Scalar Value Types](contract-params-schema.md#scalar-value-types)

[Top](contract-params-schema.md#top)

### bucket-params.proto

#### BucketParams

The parameters of a bucket.

| Field       | Type                                       | Label | Description                                                              |
| ----------- | ------------------------------------------ | ----- | ------------------------------------------------------------------------ |
| replication | [uint32](contract-params-schema.md#uint32) |       | The replication factor desired for this bucket (total number of copies). |
| description | [string](contract-params-schema.md#string) |       | A human-readable description of the bucket.                              |

[Top](contract-params-schema.md#top)

### cluster-params.proto

#### ClusterParams

The parameters of a Cluster.

| Field                | Type                                                      | Label | Description                                                                               |
| -------------------- | --------------------------------------------------------- | ----- | ----------------------------------------------------------------------------------------- |
| engineType           | [EngineType](contract-params-schema.md#pb.EngineType)     |       | The type of engine running on nodes of this cluster.                                      |
| engineMinimumVersion | [string](contract-params-schema.md#string)                |       | The minimum version of the engine necessary for a node to work correctly in this cluster. |
| topologyType         | [TopologyType](contract-params-schema.md#pb.TopologyType) |       | The kind of topology used to organize nodes.                                              |
| maximumReplication   | [uint32](contract-params-schema.md#uint32)                |       | The maximum replication factor supported on this cluster (total number of copies).        |
| description          | [string](contract-params-schema.md#string)                |       | A human-readable description of the cluster.                                              |

#### TopologyType

A topology specifies how requests should be routed to Nodes of a Cluster.

A Cluster is segmented into a number of vnodes. Each vnode is assigned to a physical Node and there should be multiple vnodes per physical Node. The assignment of vnodes is managed in the `ddc-bucket` smart contract.

Each request is assigned to a list of vnodes based on a routing key. In a STORAGE Cluster, routing keys must be derived from the identifier of data pieces. In a CDN or otherwise stateless Cluster, routing keys may be random, or may implement some affinity to users or to queries for better performance (caching, etc).

| Name          | Number | Description                                                                                                                                                            |
| ------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UNIFORM\_RING | 0      | The UNIFORM\_RING topology defines a ring split into equal segments per vnode. Replication or fallback mechanisms use contiguous vnodes. This is the default topology. |

[Top](contract-params-schema.md#top)

### node-params.proto

#### NodeParams

The parameters of a node.

| Field         | Type                                                  | Label | Description                                     |
| ------------- | ----------------------------------------------------- | ----- | ----------------------------------------------- |
| engineType    | [EngineType](contract-params-schema.md#pb.EngineType) |       | The type of engine running on the node.         |
| engineVersion | [string](contract-params-schema.md#string)            |       | The version of the engine running on the node.  |
| url           | [string](contract-params-schema.md#string)            |       | The base URL where the engine API is reachable. |
| description   | [string](contract-params-schema.md#string)            |       | A human-readable description of the node.       |

#### EngineType

The list of engine types.

| Name    | Number | Description                                            |
| ------- | ------ | ------------------------------------------------------ |
| UNKNOWN | 0      | The engine type is missing, this is probably an error. |
| STORAGE | 1      | The engine of storage Nodes to store data pieces.      |
| CDN     | 2      | The engine of CDN Nodes to upload and search pieces.   |

### Scalar Value Types

| .proto Type | Notes                                                                                                                                           | C++    | Java       | Python      | Go      | C#         | PHP            | Ruby                           |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ----------- | ------- | ---------- | -------------- | ------------------------------ |
| double      |                                                                                                                                                 | double | double     | float       | float64 | double     | float          | Float                          |
| float       |                                                                                                                                                 | float  | float      | float       | float32 | float      | float          | Float                          |
| int32       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| int64       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| uint32      | Uses variable-length encoding.                                                                                                                  | uint32 | int        | int/long    | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| uint64      | Uses variable-length encoding.                                                                                                                  | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum or Fixnum (as required) |
| sint32      | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s.                            | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| sint64      | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s.                            | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| fixed32     | Always four bytes. More efficient than uint32 if values are often greater than 2^28.                                                            | uint32 | int        | int         | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| fixed64     | Always eight bytes. More efficient than uint64 if values are often greater than 2^56.                                                           | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum                         |
| sfixed32    | Always four bytes.                                                                                                                              | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| sfixed64    | Always eight bytes.                                                                                                                             | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| bool        |                                                                                                                                                 | bool   | boolean    | boolean     | bool    | bool       | boolean        | TrueClass/FalseClass           |
| string      | A string must always contain UTF-8 encoded or 7-bit ASCII text.                                                                                 | string | String     | str/unicode | string  | string     | string         | String (UTF-8)                 |
| bytes       | May contain any arbitrary sequence of bytes.                                                                                                    | string | ByteString | str         | \[]byte | ByteString | string         | String (ASCII-8BIT)            |
