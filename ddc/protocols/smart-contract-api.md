# ⚖ Smart Contract API

DDC v2 Smart Contracts -- Orchestrate the network around clusters and buckets

## Functions

### bucket\_create (mutable, payable)

```
fn bucket_create(
    bucket_params: BucketParams,
    cluster_id: ClusterId,
) -> BucketId
```

Create a new bucket and return its `bucket_id`.

The caller will be its first owner and payer of resources.

`bucket_params` is configuration used by clients and nodes. See the [data structure of BucketParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

The bucket can be connected to a single cluster (currently). Allocate cluster resources with the function `bucket_alloc_into_cluster`

### bucket\_alloc\_into\_cluster (mutable)

```
fn bucket_alloc_into_cluster(
    bucket_id: BucketId,
    resource: Resource,
)
```

Allocate some resources of a cluster to a bucket.

The amount of resources is given per vnode (total resources will be `resource` times the number of vnodes).

### bucket\_settle\_payment (mutable)

```
fn bucket_settle_payment(
    bucket_id: BucketId,
)
```

Settle the due costs of a bucket from its payer account to the cluster account.

### bucket\_change\_params (mutable, payable)

```
fn bucket_change_params(
    bucket_id: BucketId,
    params: BucketParams,
)
```

Change the `bucket_params`, which is configuration used by clients and nodes.

See the [data structure of BucketParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

### bucket\_get (view)

```
fn bucket_get(
    bucket_id: BucketId,
) -> Result
```

Get the current status of a bucket.

### bucket\_list (view)

```
fn bucket_list(
    offset: u32,
    limit: u32,
    filter_owner_id: Option,
)
```

Iterate through all buckets.

The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`. The optimal `limit` depends on the size of params.

The results can be filtered by owner. Note that paging must still be completed fully.

### cluster\_create (mutable, payable)

```
fn cluster_create(
    _unused: AccountId,
    vnode_count: u32,
    node_ids: Vec,
    cluster_params: ClusterParams,
) -> ClusterId
```

Create a new cluster and return its `cluster_id`.

The caller will be its first manager.

The cluster is split in a number of vnodes. The vnodes are assigned to the given physical nodes in a round-robin. Only nodes of providers that trust the cluster manager can be used (see `node_trust_manager`). The assignment can be changed with the function `cluster_replace_node`.

`cluster_params` is configuration used by clients and nodes. In particular, this describes the semantics of vnodes. See the [data structure of ClusterParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

### cluster\_reserve\_resource (mutable)

```
fn cluster_reserve_resource(
    cluster_id: ClusterId,
    amount: Resource,
)
```

As manager, reserve more resources for the cluster from the free capacity of nodes.

The amount of resources is given per vnode (total resources will be `resource` times the number of vnodes).

### cluster\_replace\_node (mutable)

```
fn cluster_replace_node(
    cluster_id: ClusterId,
    vnode_i: VNodeIndex,
    new_node_id: NodeId,
)
```

As manager, re-assign a vnode to another physical node.

Only nodes of providers that trust the cluster manager can be used (see `node_trust_manager`).

### cluster\_distribute\_revenues (mutable)

```
fn cluster_distribute_revenues(
    cluster_id: ClusterId,
)
```

Trigger the distribution of revenues from the cluster to the providers.

### cluster\_change\_params (mutable, payable)

```
fn cluster_change_params(
    cluster_id: ClusterId,
    params: ClusterParams,
)
```

Change the `cluster_params`, which is configuration used by clients and nodes.

See the [data structure of ClusterParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

### cluster\_get (view)

```
fn cluster_get(
    cluster_id: ClusterId,
) -> Result
```

Get the current status of a cluster.

### cluster\_list (view)

```
fn cluster_list(
    offset: u32,
    limit: u32,
    filter_manager_id: Option,
)
```

Iterate through all clusters.

The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`. The optimal `limit` depends on the size of params.

The results can be filtered by manager. Note that paging must still be completed fully.

### node\_trust\_manager (mutable, payable)

```
fn node_trust_manager(
    manager: AccountId,
)
```

As node provider, authorize a cluster manager to use his nodes.

### node\_distrust\_manager (mutable)

```
fn node_distrust_manager(
    manager: AccountId,
)
```

As node provider, revoke the authorization of a cluster manager to use his nodes.

### node\_create (mutable, payable)

```
fn node_create(
    rent_per_month: Balance,
    node_params: NodeParams,
    capacity: Resource,
) -> NodeId
```

Create a new node and return its `node_id`.

The caller will be its owner.

`node_params` is configuration used by clients and nodes. In particular, this contains the URL to the service. See the [data structure of NodeParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

### node\_change\_params (mutable, payable)

```
fn node_change_params(
    node_id: NodeId,
    params: NodeParams,
)
```

Change the `node_params`, which is configuration used by clients and nodes.

See the [data structure of NodeParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

### node\_get (view)

```
fn node_get(
    node_id: NodeId,
) -> Result
```

Get the current status of a node.

### node\_list (view)

```
fn node_list(
    offset: u32,
    limit: u32,
    filter_provider_id: Option,
)
```

Iterate through all nodes.

The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`. The optimal `limit` depends on the size of params.

The results can be filtered by owner. Note that paging must still be completed fully.

### account\_deposit (mutable, payable)

```
fn account_deposit(
)
```

As user, deposit tokens on the account of the caller from the transaction value. This deposit can be used to pay for the services to buckets of the account.

### account\_get (view)

```
fn account_get(
    account_id: AccountId,
) -> Result
```

Get the current status of an account.

### account\_get\_usd\_per\_cere (view)

```
fn account_get_usd_per_cere(
) -> Balance
```

Get the current conversion rate between the native currency and an external currency (USD).

### account\_set\_usd\_per\_cere (mutable)

```
fn account_set_usd_per_cere(
    usd_per_cere: Balance,
)
```

As price oracle, set the current conversion rate between the native currency and an external currency (USD).

### has\_permission (view)

```
fn has_permission(
    grantee: AccountId,
    permission: Permission,
) -> bool
```

Check whether the given account has the given permission currently.

### admin\_grant\_permission (mutable, payable)

```
fn admin_grant_permission(
    grantee: AccountId,
    permission: Permission,
)
```

As admin, grant any permission to any account.

### admin\_revoke\_permission (mutable)

```
fn admin_revoke_permission(
    grantee: AccountId,
    permission: Permission,
)
```

As admin, revoke any permission to any account.

### admin\_withdraw (mutable)

```
fn admin_withdraw(
    amount: Balance,
)
```

As admin, withdraw the funds held in custody in this contract.

This is a temporary measure to allow migrating the funds to a new version of the contract.

### admin\_set\_fee\_config (mutable)

```
fn admin_set_fee_config(
    config: FeeConfig,
)
```

Set the network and cluster fee configuration.

## Events

### BucketCreated

```
event BucketCreated(
    bucket_id: BucketId, (indexed)
    owner_id: AccountId, (indexed)
)
```

A bucket was created. The given account is its first owner and payer of resources.

### BucketAllocated

```
event BucketAllocated(
    bucket_id: BucketId, (indexed)
    cluster_id: ClusterId, (indexed)
    resource: Resource, 
)
```

Some amount of resources of a cluster were allocated to a bucket.

### BucketSettlePayment

```
event BucketSettlePayment(
    bucket_id: BucketId, (indexed)
    cluster_id: ClusterId, (indexed)
)
```

The due costs of a bucket was settled from the bucket payer to the cluster.

### ClusterCreated

```
event ClusterCreated(
    cluster_id: ClusterId, (indexed)
    manager: AccountId, (indexed)
    cluster_params: ClusterParams, 
)
```

A new cluster was created.

### ClusterNodeReplaced

```
event ClusterNodeReplaced(
    cluster_id: ClusterId, (indexed)
    node_id: NodeId, (indexed)
    vnode_index: VNodeIndex, 
)
```

A vnode was re-assigned to new node.

### ClusterReserveResource

```
event ClusterReserveResource(
    cluster_id: ClusterId, (indexed)
    resource: Resource, 
)
```

Some resources were reserved for the cluster from the nodes.

### ClusterDistributeRevenues

```
event ClusterDistributeRevenues(
    cluster_id: ClusterId, (indexed)
    provider_id: AccountId, (indexed)
)
```

The share of revenues of a cluster for a provider was distributed.

### NodeCreated

```
event NodeCreated(
    node_id: NodeId, (indexed)
    provider_id: AccountId, (indexed)
    rent_per_month: Balance, 
    node_params: NodeParams, 
)
```

A node was created. The given account is its owner and recipient of revenues.

### Deposit

```
event Deposit(
    account_id: AccountId, (indexed)
    value: Balance, 
)
```

Tokens were deposited on an account.

### GrantPermission

```
event GrantPermission(
    account_id: AccountId, (indexed)
    permission: Permission, 
)
```

A permission was granted to the account.

### RevokePermission

```
event RevokePermission(
    account_id: AccountId, (indexed)
    permission: Permission, 
)
```

A permission was revoked from the account.

## Constructors

### new

```
fn new(
)
```

Create a new contract.

The caller will be admin of the contract.

## Types

### ::ddc\_bucket::ddc\_bucket::bucket::entity::Bucket

```
struct {
    owner_id: AccountId
    cluster_id: ClusterId
    flow: Flow
    resource_reserved: Resource
}
```

### ::ink\_env::types::AccountId

```
struct {
    : [u8; 32]
}
```

### ::ddc\_bucket::ddc\_bucket::flow::Flow

```
struct {
    from: AccountId
    schedule: Schedule
}
```

### ::ddc\_bucket::ddc\_bucket::schedule::Schedule

```
struct {
    rate: Balance
    offset: Balance
}
```

### ::ddc\_bucket::ddc\_bucket::cluster::entity::Cluster

```
struct {
    manager_id: AccountId
    vnodes: Vec&lt;NodeId&gt;
    resource_per_vnode: Resource
    resource_used: Resource
    revenues: Cash
    total_rent: Balance
}
```

### ::ddc\_bucket::ddc\_bucket::cash::Cash

```
struct {
    value: Balance
}
```

### ::ddc\_bucket::ddc\_bucket::node::entity::Node

```
struct {
    provider_id: ProviderId
    rent_per_month: Balance
    free_resource: Resource
}
```

### ::ink\_storage::collections::stash::Header

```
struct {
    last_vacant: Index
    len: u32
    len_entries: u32
}
```

### ::ink\_storage::collections::stash::Entry

### ::ink\_storage::collections::stash::VacantEntry

```
struct {
    next: Index
    prev: Index
}
```

### ::ink\_storage::collections::hashmap::ValueEntry

```
struct {
    value: V
    key_index: KeyIndex
}
```

### ::ddc\_bucket::ddc\_bucket::account::entity::Account

```
struct {
    deposit: Cash
    payable_schedule: Schedule
}
```

### ::ink\_storage::collections::stash::Entry

### ::ink\_storage::collections::hashmap::ValueEntry

```
struct {
    value: V
    key_index: KeyIndex
}
```

### ::Result

### ::ddc\_bucket::ddc\_bucket::bucket::entity::BucketStatus

```
struct {
    bucket_id: BucketId
    bucket: BucketInStatus
    params: BucketParams
    writer_ids: Vec&lt;AccountId&gt;
    rent_covered_until_ms: u64
}
```

### ::ddc\_bucket::ddc\_bucket::bucket::entity::BucketInStatus

```
struct {
    owner_id: AccountId
    cluster_id: ClusterId
    resource_reserved: Resource
}
```

### ::ddc\_bucket::ddc\_bucket::Error

### ::Option

### ::Result

### ::ddc\_bucket::ddc\_bucket::cluster::entity::ClusterStatus

```
struct {
    cluster_id: ClusterId
    cluster: Cluster
    params: Params
}
```

### ::Result

### ::ddc\_bucket::ddc\_bucket::node::entity::NodeStatus

```
struct {
    node_id: NodeId
    node: Node
    params: Params
}
```

### ::Result

### ::ddc\_bucket::ddc\_bucket::perm::entity::Permission

### ::ddc\_bucket::ddc\_bucket::network\_fee::FeeConfig

```
struct {
    network_fee_bp: BasisPoints
    network_fee_destination: AccountId
    cluster_management_fee_bp: BasisPoints
}
```
