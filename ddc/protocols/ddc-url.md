# 🔗 DDC URLs

{% hint style="info" %} This is the specification of `DDC URL v0.2`
{% endhint %}

![Structure of DDC URLs](<../../.gitbook/assets/DDC URL.png>)

## URIs - Identifying objects in DDC

A URI represents an object or a set of objects in the DDC network.

Here is an example URI to a file:

    /ddc/org/my_project/buc/my_files/file/my_picture.jpeg

The meaning of the different parts of such URIs is explained below.


### The `file/` queries

A `file/` URI represents a file, as known from usual file systems. The simplest form points to a
file by a path inside of a named bucket:

    /ddc/buc/BUCKET_ID/file/PATH

But in general, a file URI corresponds to a `piece/` query of a data piece in DDC Object Storage (described below):

    /ddc/buc/BUCKET_ID/piece/PATH

The piece is to be interpreted as a file descriptor, which contains metadata and links to other
pieces holding the file content. See [📂 File Storage](file-storage.md) for more details.


### The `piece/` queries

A piece is a container of data and metadata. It is the smallest indivisible unit stored in DDC
object storage. A piece can contain links to other pieces.

A `piece/` URI represents a data piece or a set of pieces in DDC Object Storage. Piece URIs take
the following form:

    /ddc/buc/BUCKET_ID/piece/PATH


### Identifying files and pieces

The PATH part of a URI identifies a piece of content within a bucket.

Mutable content can be addressed by a filename or a slash-based path:

        file/NAME
        file/SLASH/SEPARATED/PATH

Mutable content can also be addressed by a key/value search on the tags of data pieces. A query by path is actually a shorthand for a search on the tag `file-path`. See the [☁ Storage Schema](storage-schema.md) for details on tags.

        file/?KEY=VALUE (and &…)

A query to mutable content represents the set of pieces that match the query. In contexts where a
single piece is requested, the most recent piece should be returned. (TODO: specify the ordering)


### The `ifile/` and `ipiece/` queries - Immutable data

The `ifile/` and `ipiece/` queries are similar to `file/` and `piece/`, but they represent **immutable data**. The pieces are permanently identified by a Content Identifier (CID). The pieces can be looked up using the content-addressable protocol implemented by CDN and storage nodes. A client given a piece can always verify cryptographically that it matches its URI.

    /ddc/buc/BUCKET_ID/ifile/CID
    /ddc/buc/BUCKET_ID/ipiece/CID


### `buc/` - Identifying buckets by ID

A bucket can be identified by its ID:

    buc/BUCKET_ID/

BUCKET_ID is a numerical, unique, and immutable identifier to a bucket.
A `buc/…/` query can be resolved in the DDC bucket system to identify the current set
of storage nodes responsible for this bucket.


### `org/` - Identifying buckets by name

A bucket can also be identified by a name:

    org/ORGANIZATION_NAME/buc/BUCKET_NAME/

BUCKET_NAME is a mutable alias to a bucket. Bucket names exist within an *organization*, and can be freely chosen by owners of the organization. An organization itself has a unique name (ORGANIZATION_NAME), which must be claimed and controlled by user accounts in the DDC name system, on the Cere blockchain. A `org/…/buc/…/` query can be resolved to find the ID of a bucket.


## Web URLs - The gateway from the web to DDC

DDC defines Web URLs that can be used anywhere where HTTPS is expected, i.e. in web browsers. A Web URL
is constructed with the URL to a CDN node, followed by the DDC URI to resolve, like so:

    https://CDN_NODE/DDC_URI

For example:

    https://cdn.cere.network/ddc/buc/123/file/picture.png
    ^                       ^
    The web gateway         The DDC URI

The Web URL identifies a file, and suggest a CDN node that can retrieve it from the DDC network. Given
a request for this URL, the CDN node will find the data piece(s) that describe the file, and return
the file content.

The client may also use any other CDN node of his choice, or it may use the DDC protocol by
itself and find the file in storage nodes directly.

The DDC URI of an object can always be parsed from a Web URL by detecting the first occurence of the string `/ddc/`.


[requirements]: https://www.notion.so/cere/Architecture-of-DDC-software-2d6824916b394fa0bc20ff176525d0fc#c8397cdafc4d4f5a9ddd1072a87c189e
[original picture]: https://miro.com/app/board/o9J_lsMr5wI=/?moveToWidget=3458764526343104952&cot=14
