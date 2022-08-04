# Contract ddc_bucket 0.5.1

DDC v2 Smart Contracts -- Orchestrate the network around clusters and buckets


## Functions

### bucket_create (mutable, payable)

    fn bucket_create(
        bucket_params: BucketParams,
        cluster_id: ClusterId,
    ) -> BucketId

 Create a new bucket and return its `bucket_id`.

 The caller will be its first owner and payer of resources.

 `bucket_params` is configuration used by clients and nodes. See the [data structure of BucketParams](https://docs.cere.network/ddc/protocols/contract-params-schema)

 The bucket can be connected to a single cluster (currently). Allocate cluster resources with the function `bucket_alloc_into_cluster`


### bucket_alloc_into_cluster (mutable)

    fn bucket_alloc_into_cluster(
        bucket_id: BucketId,
        resource: Resource,
    )

 Allocate some resources of a cluster to a bucket.

 The amount of resources is given per vnode (total resources will be `resource` times the number of vnodes).


### bucket_settle_payment (mutable)

    fn bucket_settle_payment(
        bucket_id: BucketId,
    )

 Settle the due costs of a bucket from its payer account to the cluster account.


### bucket_change_params (mutable, payable)

    fn bucket_change_params(
        bucket_id: BucketId,
        params: BucketParams,
    )

 Change the `bucket_params`, which is configuration used by clients and nodes.

 See the [data structure of BucketParams](https://docs.cere.network/ddc/protocols/contract-params-schema)


### bucket_get (view)

    fn bucket_get(
        bucket_id: BucketId,
    ) -> Result

 Get the current status of a bucket.


### bucket_list (view)

    fn bucket_list(
        offset: u32,
        limit: u32,
        filter_owner_id: Option,
    )

 Iterate through all buckets.

 The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`.
 The optimal `limit` depends on the size of params.

 The results can be filtered by owner. Note that paging must still be completed fully.


### cluster_create (mutable, payable)

    fn cluster_create(
        _unused: AccountId,
        vnode_count: u32,
        node_ids: Vec,
        cluster_params: ClusterParams,
    ) -> ClusterId

 Create a new cluster and return its `cluster_id`.

 The caller will be its first manager.

 The cluster is split in a number of vnodes. The vnodes are assigned to the given physical nodes in a round-robin. Only nodes of providers that trust the cluster manager can be used (see `node_trust_manager`). The assignment can be changed with the function `cluster_replace_node`.

 `cluster_params` is configuration used by clients and nodes. In particular, this describes the semantics of vnodes. See the [data structure of ClusterParams](https://docs.cere.network/ddc/protocols/contract-params-schema)


### cluster_reserve_resource (mutable)

    fn cluster_reserve_resource(
        cluster_id: ClusterId,
        amount: Resource,
    )

 As manager, reserve more resources for the cluster from the free capacity of nodes.

 The amount of resources is given per vnode (total resources will be `resource` times the number of vnodes).


### cluster_replace_node (mutable)

    fn cluster_replace_node(
        cluster_id: ClusterId,
        vnode_i: VNodeIndex,
        new_node_id: NodeId,
    )

 As manager, re-assign a vnode to another physical node.

 Only nodes of providers that trust the cluster manager can be used (see `node_trust_manager`).


### cluster_distribute_revenues (mutable)

    fn cluster_distribute_revenues(
        cluster_id: ClusterId,
    )

 Trigger the distribution of revenues from the cluster to the providers.


### cluster_change_params (mutable, payable)

    fn cluster_change_params(
        cluster_id: ClusterId,
        params: ClusterParams,
    )

 Change the `cluster_params`, which is configuration used by clients and nodes.

 See the [data structure of ClusterParams](https://docs.cere.network/ddc/protocols/contract-params-schema)


### cluster_get (view)

    fn cluster_get(
        cluster_id: ClusterId,
    ) -> Result

 Get the current status of a cluster.


### cluster_list (view)

    fn cluster_list(
        offset: u32,
        limit: u32,
        filter_manager_id: Option,
    )

 Iterate through all clusters.

 The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`.
 The optimal `limit` depends on the size of params.

 The results can be filtered by manager. Note that paging must still be completed fully.


### node_trust_manager (mutable, payable)

    fn node_trust_manager(
        manager: AccountId,
    )

 As node provider, authorize a cluster manager to use his nodes.


### node_distrust_manager (mutable)

    fn node_distrust_manager(
        manager: AccountId,
    )

 As node provider, revoke the authorization of a cluster manager to use his nodes.


### node_create (mutable, payable)

    fn node_create(
        rent_per_month: Balance,
        node_params: NodeParams,
        capacity: Resource,
    ) -> NodeId

 Create a new node and return its `node_id`.

 The caller will be its owner.

 `node_params` is configuration used by clients and nodes. In particular, this contains the URL to the service. See the [data structure of NodeParams](https://docs.cere.network/ddc/protocols/contract-params-schema)


### node_change_params (mutable, payable)

    fn node_change_params(
        node_id: NodeId,
        params: NodeParams,
    )

 Change the `node_params`, which is configuration used by clients and nodes.

 See the [data structure of NodeParams](https://docs.cere.network/ddc/protocols/contract-params-schema)


### node_get (view)

    fn node_get(
        node_id: NodeId,
    ) -> Result

 Get the current status of a node.


### node_list (view)

    fn node_list(
        offset: u32,
        limit: u32,
        filter_provider_id: Option,
    )

 Iterate through all nodes.

 The algorithm for paging is: start with `offset = 1` and `limit = 20`. The function returns a `(results, max_id)`. Call again with `offset += limit`, until `offset >= max_id`.
 The optimal `limit` depends on the size of params.

 The results can be filtered by owner. Note that paging must still be completed fully.


### account_deposit (mutable, payable)

    fn account_deposit(
    )

 As user, deposit tokens on the account of the caller from the transaction value. This deposit
 can be used to pay for the services to buckets of the account.


### account_get (view)

    fn account_get(
        account_id: AccountId,
    ) -> Result

 Get the current status of an account.


### account_get_usd_per_cere (view)

    fn account_get_usd_per_cere(
    ) -> Balance

 Get the current conversion rate between the native currency and an external currency (USD).


### account_set_usd_per_cere (mutable)

    fn account_set_usd_per_cere(
        usd_per_cere: Balance,
    )

 As price oracle, set the current conversion rate between the native currency and an external currency (USD).


### has_permission (view)

    fn has_permission(
        grantee: AccountId,
        permission: Permission,
    ) -> bool

 Check whether the given account has the given permission currently.


### admin_grant_permission (mutable, payable)

    fn admin_grant_permission(
        grantee: AccountId,
        permission: Permission,
    )

 As admin, grant any permission to any account.


### admin_revoke_permission (mutable)

    fn admin_revoke_permission(
        grantee: AccountId,
        permission: Permission,
    )

 As admin, revoke any permission to any account.


### admin_withdraw (mutable)

    fn admin_withdraw(
        amount: Balance,
    )

 As admin, withdraw the funds held in custody in this contract.

 This is a temporary measure to allow migrating the funds to a new version of the contract.


### admin_set_fee_config (mutable)

    fn admin_set_fee_config(
        config: FeeConfig,
    )

 Set the network and cluster fee configuration.



## Events


### BucketCreated

    event BucketCreated(
        bucket_id: BucketId, (indexed)
        owner_id: AccountId, (indexed)
    )

 A bucket was created. The given account is its first owner and payer of resources.


### BucketAllocated

    event BucketAllocated(
        bucket_id: BucketId, (indexed)
        cluster_id: ClusterId, (indexed)
        resource: Resource, 
    )

 Some amount of resources of a cluster were allocated to a bucket.


### BucketSettlePayment

    event BucketSettlePayment(
        bucket_id: BucketId, (indexed)
        cluster_id: ClusterId, (indexed)
    )

 The due costs of a bucket was settled from the bucket payer to the cluster.


### ClusterCreated

    event ClusterCreated(
        cluster_id: ClusterId, (indexed)
        manager: AccountId, (indexed)
        cluster_params: ClusterParams, 
    )

 A new cluster was created.


### ClusterNodeReplaced

    event ClusterNodeReplaced(
        cluster_id: ClusterId, (indexed)
        node_id: NodeId, (indexed)
        vnode_index: VNodeIndex, 
    )

 A vnode was re-assigned to new node.


### ClusterReserveResource

    event ClusterReserveResource(
        cluster_id: ClusterId, (indexed)
        resource: Resource, 
    )

 Some resources were reserved for the cluster from the nodes.


### ClusterDistributeRevenues

    event ClusterDistributeRevenues(
        cluster_id: ClusterId, (indexed)
        provider_id: AccountId, (indexed)
    )

 The share of revenues of a cluster for a provider was distributed.


### NodeCreated

    event NodeCreated(
        node_id: NodeId, (indexed)
        provider_id: AccountId, (indexed)
        rent_per_month: Balance, 
        node_params: NodeParams, 
    )

 A node was created. The given account is its owner and recipient of revenues.


### Deposit

    event Deposit(
        account_id: AccountId, (indexed)
        value: Balance, 
    )

 Tokens were deposited on an account.


### GrantPermission

    event GrantPermission(
        account_id: AccountId, (indexed)
        permission: Permission, 
    )

 A permission was granted to the account.


### RevokePermission

    event RevokePermission(
        account_id: AccountId, (indexed)
        permission: Permission, 
    )

 A permission was revoked from the account.



## Constructors


### new

    fn new(
    )

Create a new contract.

The caller will be admin of the contract.



## Types


### ::ddc_bucket::ddc_bucket::bucket::entity::Bucket

    struct {
        owner_id: AccountId
        cluster_id: ClusterId
        flow: Flow
        resource_reserved: Resource
    }


### ::ink_env::types::AccountId

    struct {
        : [u8; 32]
    }


### ::ddc_bucket::ddc_bucket::flow::Flow

    struct {
        from: AccountId
        schedule: Schedule
    }


### ::ddc_bucket::ddc_bucket::schedule::Schedule

    struct {
        rate: Balance
        offset: Balance
    }


### ::ddc_bucket::ddc_bucket::cluster::entity::Cluster

    struct {
        manager_id: AccountId
        vnodes: Vec&lt;NodeId&gt;
        resource_per_vnode: Resource
        resource_used: Resource
        revenues: Cash
        total_rent: Balance
    }


### ::ddc_bucket::ddc_bucket::cash::Cash

    struct {
        value: Balance
    }


### ::ddc_bucket::ddc_bucket::node::entity::Node

    struct {
        provider_id: ProviderId
        rent_per_month: Balance
        free_resource: Resource
    }


### ::ink_storage::collections::stash::Header

    struct {
        last_vacant: Index
        len: u32
        len_entries: u32
    }


### ::ink_storage::collections::stash::Entry



### ::ink_storage::collections::stash::VacantEntry

    struct {
        next: Index
        prev: Index
    }


### ::ink_storage::collections::hashmap::ValueEntry

    struct {
        value: V
        key_index: KeyIndex
    }


### ::ddc_bucket::ddc_bucket::account::entity::Account

    struct {
        deposit: Cash
        payable_schedule: Schedule
    }


### ::ink_storage::collections::stash::Entry



### ::ink_storage::collections::hashmap::ValueEntry

    struct {
        value: V
        key_index: KeyIndex
    }


### ::Result



### ::ddc_bucket::ddc_bucket::bucket::entity::BucketStatus

    struct {
        bucket_id: BucketId
        bucket: BucketInStatus
        params: BucketParams
        writer_ids: Vec&lt;AccountId&gt;
        rent_covered_until_ms: u64
    }


### ::ddc_bucket::ddc_bucket::bucket::entity::BucketInStatus

    struct {
        owner_id: AccountId
        cluster_id: ClusterId
        resource_reserved: Resource
    }


### ::ddc_bucket::ddc_bucket::Error



### ::Option



### ::Result



### ::ddc_bucket::ddc_bucket::cluster::entity::ClusterStatus

    struct {
        cluster_id: ClusterId
        cluster: Cluster
        params: Params
    }


### ::Result



### ::ddc_bucket::ddc_bucket::node::entity::NodeStatus

    struct {
        node_id: NodeId
        node: Node
        params: Params
    }


### ::Result



### ::ddc_bucket::ddc_bucket::perm::entity::Permission



### ::ddc_bucket::ddc_bucket::network_fee::FeeConfig

    struct {
        network_fee_bp: BasisPoints
        network_fee_destination: AccountId
        cluster_management_fee_bp: BasisPoints
    }

