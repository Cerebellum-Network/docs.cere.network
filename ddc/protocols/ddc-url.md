# 🔗 DDC URL

{% hint style="warning" %}
TODO

Write up from [these requirements](https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#c8397cdafc4d4f5a9ddd1072a87c189e).
{% endhint %}


![Structure of DDC URLs](<../../.gitbook/assets/DDC URL.png>)


## URI - Queries in the DDC Protocol

A URI represents an object or a set of objects in the DDC network.


### File Protocol

A `ddc:file` URI represents a file. The most usual form points to a file path inside of a named
bucket:

    ddc:file/BUCKET_NAME/FILE_PATH

But in general, a file URI corresponds to a query of a data piece in DDC Object Storage, using the
`ddc:piece` protocol (described below):

    ddc:file/BUCKET/PIECE

The piece is to be interpreted as a file descriptor, possibly fetching additional
file content from linked pieces.


### Piece Protocol

A `ddc:piece` URI represents a data piece or a set of pieces in DDC Object Storage. Piece URIs take
the following form:

    ddc:piece/BUCKET/PIECE

The notable form below represents an immutable piece addressed by its Content ID (CID), without a
hint for a containing bucket:

    ddc:piece/*/=CID


### Identifying Buckets

BUCKET_ID or BUCKET_NAME resolves to a query in the DDC bucket system to identify the current set
of storage nodes where the files or pieces should be found.

* BUCKET_ID is a numerical, unique, and immutable identifier to a bucket.

* BUCKET_NAME is a mutable alias to a bucket. A bucket name must be claimed and controlled by user
  accounts on the Cere blockchain.

* A star (`*`) indicates that the bucket is unknown, or irrelevant, or that a query spans all
  buckets.


### Identifying Files and Pieces

The PIECE part of a URI identifies a piece of content within a bucket.

* Immutable content is addressed by an equal sign (`=`) followed by a Content Identifier (CID):

        =CID

* Mutable content can be addressed by a slash-based path. Such path is a shorthand for a search on
  the tag `path`.

        NAME or SLASH/SEPARATED/PATH

* Mutable content can be addressed by a search on the tags of pieces:

        ?tag=KEY:VALUE
        ?tag=KEY:VALUE&tag=KEY:VALUE

A query to mutable content represents the set of pieces that match the query. In contexts where a
single piece is requested, the most recent piece should be returned. (TODO: specify the ordering)


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
