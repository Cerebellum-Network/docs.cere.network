# 🔗 DDC URL

{% hint style="warning" %}
TODO

Write up from [these requirements](https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#c8397cdafc4d4f5a9ddd1072a87c189e).
{% endhint %}


![Structure of DDC URLs](<../../.gitbook/assets/DDC URL.png>)

## URI - Queries in the DDC Protocol

A URI represents an object or a set of objects in the DDC network.

### File protocol

Represent a file. FILE_PATH resolves to a query of pieces by tag. The piece will be interpreted as a file descriptor, possibly fetching the file content from other pieces.

    ddc:file/BUCKET_NAME/FILE_PATH

### Piece protocol

Represent a piece by bucket ID and piece ID:

    ddc:piece/BUCKET_ID/CID

Represent a piece by piece ID, in any bucket:

    ddc:piece/*/CID

Search a piece by tag and bucket ID:

    ddc:piece/BUCKET_ID/?tag=KEY:VALUE

Or by bucket NAME:

    ddc:piece/BUCKET_NAME/?tag=KEY:VALUE

Select a piece with additional options:

    ddc:piece/BUCKET_NAME/?tag=KEY:VALUE&version:latest


## URL - The gateway from the web to DDC

DDC defines URLs that can be used anywhere where HTTPS is expected, i.e. in web browsers. A web URL is constructed with the URL to a CDN node, followed by the DDC URI to resolve, like so:

    CDN_NODE_URL/DDC_URI

For example:

    https://cdn.cere.network/ddc:file/my_bucket/image.png
    ^                        ^
    The web gateway          The DDC URI

The URL identifies a file, and suggest a CDN node that can retrieve it from the DDC network. Given
a request for this URL, the CDN node will find the data piece(s) that describe the file, and return
the file content.

The client may also use another CDN node of his choice, or it may use the `ddc:file` protocol by itself and
find the file in storage nodes directly.

The URI of an object can always be parsed from a URL by detecting the first occurence of the string `/ddc:`
