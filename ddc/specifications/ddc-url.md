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

DDC defines URLs that can be used anywhere where HTTPS is expected, i.e. in web browsers.

The URI of an object can be parsed from a URL by detecting the first occurence of the string `/ddc:`

The client can connect to a given CDN node and request it to fetch an object by its DDC URI. The client can also decide to use another CDN node of his choice.

    https://ANY-CDN-NODE/DDC-URI

Example: fetch a piece that describes a file, and return the file content.

    https://cdn.cere.network/ddc:file/my_bucket/image.png

            The web gateway / The DDC query

Fetch a piece and return the payload.

    https://cdn.cere.network/ddc:piece/BUCKET_ID/PIECE_ID
