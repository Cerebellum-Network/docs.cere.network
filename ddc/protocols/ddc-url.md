# 🔗 DDC URL

{% hint style="warning" %}
TODO

Write up from [these requirements](https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#c8397cdafc4d4f5a9ddd1072a87c189e).
{% endhint %}


![Structure of DDC URLs](<../../.gitbook/assets/DDC URL.png>)


## URI - Queries in the DDC Protocol

A URI represents an object or a set of objects in the DDC network.


### The `file` Protocol

A `ddc:file` URI represents a file, as known from usual file systems. The simplest form points to a
file by a path inside of a named bucket:

    ddc:file/BUCKET_NAME/FILE_PATH

But in general, a file URI corresponds to a query of a data piece in DDC Object Storage, using the
`ddc:piece` protocol (described below):

    ddc:file/BUCKET/PIECE

The piece is to be interpreted as a file descriptor, which contains metadata and links to other
pieces holding the file content.


### The `piece` Protocol

A piece is a container of data and metadata. It is the smallest indivisible unit stored in DDC
object storage. A piece can contain links to other pieces.

A `ddc:piece` URI represents a data piece or a set of pieces in DDC Object Storage. Piece URIs take
the following form:

    ddc:piece/BUCKET/PIECE


### Identifying Buckets

BUCKET can be a bucket NAME or a bucket ID. It resolves to a query in the DDC bucket system to identify the current set
of storage nodes where the files or pieces should be found.

* BUCKET ID is a numerical, unique, and immutable identifier to a bucket.

* BUCKET NAME is a mutable alias to a bucket. A bucket name must be claimed and controlled by user
  accounts on the Cere blockchain.


### Identifying Files and Pieces

The PIECE part of a URI identifies a piece of content within a bucket.

* Mutable content can be addressed by a name or a slash-based path. Such path is a shorthand for a
  search on the tag `path`.

        NAME or SLASH/SEPARATED/PATH

* Mutable content can be addressed by a search on the tags of pieces:

        ?tag=KEY:VALUE
        ?tag=KEY:VALUE&tag=KEY:VALUE

A query to mutable content represents the set of pieces that match the query. In contexts where a
single piece is requested, the most recent piece should be returned. (TODO: specify the ordering)


### Immutable Data - The `ifile` and `ipiece` protocols

The `ddc:ifile` and `ddc:ipiece` protocols are similar to `ddc:file` and `ddc:piece`, but they represent **immutable data**. The pieces are permanently identified by a Content Identifier (CID). The pieces can be looked up using the content-addressable protocol implemented by CDN and storage nodes. A client given a piece can always verify cryptographically that it matches its URI.

    ddc:ifile/BUCKET/CID
    ddc:ipiece/BUCKET/CID


## URL - The gateway from the web to DDC

DDC defines URLs that can be used anywhere where HTTPS is expected, i.e. in web browsers. A web URL
is constructed with the URL to a CDN node, followed by the DDC URI to resolve, like so:

    CDN_NODE_URL/DDC_URI

For example:

    https://cdn.cere.network/ddc:file/my_bucket/image.png
    ^                        ^
    The web gateway          The DDC URI

The URL identifies a file, and suggest a CDN node that can retrieve it from the DDC network. Given
a request for this URL, the CDN node will find the data piece(s) that describe the file, and return
the file content.

The client may also use another CDN node of his choice, or it may use the `ddc:file` protocol by
itself and find the file in storage nodes directly.

The URI of an object can always be parsed from a URL by detecting the first occurence of the string `/ddc:`
