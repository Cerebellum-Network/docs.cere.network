
# Schema of the parameters in the DDC bucket contract

This is the data model used to configure clusters, nodes, and buckets in the [ddc-bucket smart contract](https://github.com/Cerebellum-Network/ddc-bucket-contract).


## Bucket Parameters

Bucket parameters specify:

- How data should be replicated.


## Cluster Parameters

Cluster parameters specify:

- What type of nodes can be included in a cluster.
- How data and requests should be routed to nodes.
- Any additional details given by the cluster manager.


## Node Parameters

Node parameters specify:

- What engine a node is running.
- How to connect to a physical node that provides the service offered in the contract.
- Any additional details given by a provider about his node.


([Source](https://github.com/Cerebellum-Network/ddc-schemas))

## Changelog

### vNext

- Basic bucket parameters with replication factor.
- Basic cluster parameters with engine name, version, and the uniform topology.
- Basic node parameters with the node URL and engine version.


# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [bucket-params.proto](#bucket-params.proto)
    - [BucketParams](#pb.BucketParams)
  
- [cluster-params.proto](#cluster-params.proto)
    - [ClusterParams](#pb.ClusterParams)
  
    - [TopologyType](#pb.TopologyType)
  
- [node-params.proto](#node-params.proto)
    - [NodeParams](#pb.NodeParams)
  
    - [EngineType](#pb.EngineType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="bucket-params.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## bucket-params.proto



<a name="pb.BucketParams"></a>

### BucketParams
The parameters of a bucket.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| replication | [uint32](#uint32) |  | The replication factor desired for this bucket (total number of copies). |
| description | [string](#string) |  | A human-readable description of the bucket. |





 

 

 

 



<a name="cluster-params.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## cluster-params.proto



<a name="pb.ClusterParams"></a>

### ClusterParams
The parameters of a cluster.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| engineType | [EngineType](#pb.EngineType) |  | The type of engine running on nodes of this cluster. |
| engineMinimumVersion | [string](#string) |  | The minimum version of the engine necessary for a node to work correctly in this cluster. |
| topologyType | [TopologyType](#pb.TopologyType) |  | The kind of topology used to organize nodes. |
| maximumReplication | [uint32](#uint32) |  | The maximum replication factor supported on this cluster (total number of copies). |
| description | [string](#string) |  | A human-readable description of the cluster. |





 


<a name="pb.TopologyType"></a>

### TopologyType
A topology specifies how requests should be routed to nodes of a cluster.

A cluster is segmented into a number of vnodes. Each vnode is assigned to a physical node and
there should be multiple vnodes per physical node. The assignment of vnodes is managed in the
`ddc-bucket` smart contract.

Each request is assigned to a list of vnodes based on a routing key.
In a STORAGE cluster, routing keys must be derived from the identifier of data pieces.
In a CDN or otherwise stateless cluster, routing keys may be random, or may implement some
affinity to users or to queries for better performance (caching, etc).

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNIFORM_RING | 0 | The UNIFORM_RING topology defines a ring split into equal segments per vnode. Replication or fallback mechanisms use contiguous vnodes. This is the default topology. |


 

 

 



<a name="node-params.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## node-params.proto



<a name="pb.NodeParams"></a>

### NodeParams
The parameters of a node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| engineType | [EngineType](#pb.EngineType) |  | The type of engine running on the node. |
| engineVersion | [string](#string) |  | The version of the engine running on the node. |
| url | [string](#string) |  | The base URL where the engine API is reachable. |
| description | [string](#string) |  | A human-readable description of the node. |





 


<a name="pb.EngineType"></a>

### EngineType
The list of engine types.

| Name | Number | Description |
| ---- | ------ | ----------- |
| UNKNOWN | 0 | The engine type is missing, this is probably an error. |
| STORAGE | 1 | The engine of storage nodes to store data pieces. |
| CDN | 2 | The engine of CDN nodes to upload and search pieces. |


 

 

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

