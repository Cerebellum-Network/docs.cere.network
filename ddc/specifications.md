# 📃 Specifications

## DDC URLs

{% hint style="warning" %}
TODO

Write up from [these requirements](https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#c8397cdafc4d4f5a9ddd1072a87c189e).
{% endhint %}


![Structure of DDC URLs](<../.gitbook/assets/DDC URL.png>)

Represent a piece by bucket ID and piece ID:

    ddc:piece/BUCKET_ID/CID

Represent a piece by piece ID, in any bucket:

    ddc:piece/*/CID

Search a piece by tag and bucket ID:

    ddc:piece/BUCKET_ID/?tag=KEY:VALUE

Or by bucket NAME:

    ddc:piece/@BUCKET_NAME/?tag=KEY:VALUE

Select a piece with additional options:

    ddc:piece/@BUCKET_NAME/?tag=KEY:VALUE&version:latest

Represent a file. FILE_PATH resolves to a query of pieces by tag. The piece will be interpreted as a file descriptor, possibly fetching the file content from other pieces.

    ddc:file/BUCKET_NAME/FILE_PATH

### Web Gateway

DDC defines URLs that can be used anywhere where HTTPS is expected, i.e. in web browsers.

Connect to a given CDN Node and request it to fetch an object by its DDC_URL.

    https://ANY_CDN/DDC_URL

Example:

    https://cdn.cere.network/file/@my_bucket/image.png

Fetch a piece and return the payload.

    https://cdn.cere.network/piece/…


Fetch a piece that describes a file, and return the file content.

    https://cdn.cere.network/file/…




## Network Description

### Discovering nodes

{% hint style="warning" %}
TODO

Explain the discovery of CDN nodes. Clients do that.

&#x20;   Trusted CDN Cluster ID -> CDN Nodes
{% endhint %}

{% hint style="warning" %}
TODO

Explain the discovery of Storage nodes. Normally CDN nodes do that:\
&#x20;   Bucket ID -> Storage Cluster -> Storage Nodes
{% endhint %}

### Discovering data

{% hint style="warning" %}
TODO

Explain the discovery flows:\
&#x20;   Wallet address -> Buckets -> list Pieces, list Files, or search by tag
{% endhint %}

### Bucket Description

### Cluster Description

### Node Description

### Computing Resources

{% hint style="warning" %}
TODO

Specify in what units resources are measured and priced.
{% endhint %}

## CDN Interface

## Storage Interface

## File Storage

{% hint style="warning" %}
TODO

Explain how are files and folders represented over object storage.
{% endhint %}

## Key-Value Storage

{% hint style="warning" %}
TODO

Explain how key-values are represented over object storage.
{% endhint %}

## Data Encryption Format

{% hint style="warning" %}
TODO

Write up from [these requirements](https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#f62263dcb5254e5cac9784d17a9efd55).
{% endhint %}
