# ☁ Storage Schema

## Storage Schema

This is the data model for the API of the DDC Storage.

([Source](https://github.com/Cerebellum-Network/ddc-schemas))

### Changelog

#### vNext

#### v0.1.4

* `SignedPiece.piece` is embedded as a serialized message.
  * No changes in the wire format.
  * This facilitates forward-compatibility and signature verification.
* Improved security and performance of signed upload requests.
  * The signature and the signer address do not need to be encoded in hexadecimal.
  * The signed message explains what is signed and is human-readable.
  * The signed message includes a timestamp.
* Added test vectors from the JS SDK.

#### v0.1.3

* \[Breaking] Return piece CIDs in `SearchResult`.
* Added the option `Query.skipData` to fetch piece metadata without payload data in search results.
* Added the option `Tag.searchable` to specify whether or not a tag should be indexed for fast search.
* `Tag.key` and `Tag.value` may contain bytes instead of only string.
* Added a table of known tags.

#### v0.1.2

* Piece and query model.
* As implemented by the [Go SDK v0.1.2](https://github.com/Cerebellum-Network/cere-ddc-sdk-go/releases/tag/v0.1.2)
* Inline documentation.

## Protocol Documentation

### Table of Contents

* [link.proto](storage-schema.md#link.proto)
  * [Link](storage-schema.md#pb.Link)
* [piece.proto](storage-schema.md#piece.proto)
  * [Piece](storage-schema.md#pb.Piece)
* [query.proto](storage-schema.md#query.proto)
  * [Query](storage-schema.md#pb.Query)
* [search\_result.proto](storage-schema.md#search\_result.proto)
  * [SearchResult](storage-schema.md#pb.SearchResult)
  * [SearchedPiece](storage-schema.md#pb.SearchedPiece)
* [signature.proto](storage-schema.md#signature.proto)
  * [Signature](storage-schema.md#pb.Signature)
* [signed\_piece.proto](storage-schema.md#signed\_piece.proto)
  * [SignedPiece](storage-schema.md#pb.SignedPiece)
* [tag.proto](storage-schema.md#tag.proto)
  * [Tag](storage-schema.md#pb.Tag)
  * [SearchType](storage-schema.md#pb.SearchType)
* [Scalar Value Types](storage-schema.md#scalar-value-types)

[Top](storage-schema.md#top)

### link.proto

#### Link

A link is a pointer from one piece to another.

| Field | Type                               | Label    | Description                                                        |
| ----- | ---------------------------------- | -------- | ------------------------------------------------------------------ |
| cid   | [string](storage-schema.md#string) |          | The CID of the linked piece.                                       |
| size  | [uint64](storage-schema.md#uint64) |          | The size of the payload of the linked piece.                       |
| name  | [string](storage-schema.md#string) | optional | The name of the linked piece, in the context of the linking piece. |

[Top](storage-schema.md#top)

### piece.proto

#### Piece

A piece is a container of data and metadata. It is the smallest indivisible unit stored in the DDC object storage.

| Field    | Type                               | Label    | Description                                                                                                          |
| -------- | ---------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------- |
| data     | [bytes](storage-schema.md#bytes)   |          | The opaque payload carried by the piece.                                                                             |
| bucketId | [uint32](storage-schema.md#uint32) |          | The ID of a bucket that contains the piece.                                                                          |
| tags     | [Tag](storage-schema.md#pb.Tag)    | repeated | A list of tags with which the piece may be searched. There can be multiple tags with the same key.                   |
| links    | [Link](storage-schema.md#pb.Link)  | repeated | A list of links to other pieces. If this piece is interpreted as a file, the linked pieces make up the file content. |

[Top](storage-schema.md#top)

### query.proto

#### Query

A query represents a search of pieces by tags.

| Field    | Type                               | Label    | Description                                                                                              |
| -------- | ---------------------------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| bucketId | [uint32](storage-schema.md#uint32) |          | The ID of the bucket to search.                                                                          |
| tags     | [Tag](storage-schema.md#pb.Tag)    | repeated | A list of tags to match against the tags of stored pieces. There can be multiple tags with the same key. |
| skipData | [bool](storage-schema.md#bool)     |          | Skip piece data in search result.                                                                        |

[Top](storage-schema.md#top)

### search\_result.proto

#### SearchResult

A search result contains the pieces found from a search.

| Field          | Type                                                | Label    | Description                          |
| -------------- | --------------------------------------------------- | -------- | ------------------------------------ |
| searchedPieces | [SearchedPiece](storage-schema.md#pb.SearchedPiece) | repeated | The list of pieces found in storage. |

#### SearchedPiece

A searched piece found in storage.

| Field       | Type                                            | Label | Description             |
| ----------- | ----------------------------------------------- | ----- | ----------------------- |
| signedPiece | [SignedPiece](storage-schema.md#pb.SignedPiece) |       | Found signed piece.     |
| cid         | [string](storage-schema.md#string)              |       | CID of the found piece. |

[Top](storage-schema.md#top)

### signature.proto

#### Signature

A signature and details to help verify it.

**Generation**

* Compute a CID from the `piece` bytes using details from `signature`:
  * [CIDv1](https://github.com/multiformats/cid).
  * The hash function should be blake2b-256 and `multiHashType` should be empty.
  * Content type codec `0x55`
  * Base encoded in Base32 with the prefix `b`
  * Example: bafk2bzacea73ycjnxe2qov7cvnhx52lzfp6nf5jcblnfus6gqreh6ygganbws
* Store the public key of the signer in `publicKey` in binary encoding.
* Store the current time in `timestamp`
  * In JavaScript: `timestamp = +new Date()`
* Format the current time in ISO 8601 `YYYY-MM-DDTHH:mm:ss.sssZ`
* In JavaScript: `timeText = new Date(timestamp).toISOString()`
* In Go format: `2006-01-02T15:04:05.000Z`
* The signed message to store a piece is:
  * `<Bytes>DDC store ${CID} at ${timeText}</Bytes>`
  * Note: the `<Bytes>` part is enforced by the Polkadot.js browser extension.
  * Example: `<Bytes>DDC store bafk2bzacea73ycjnxe2qov7cvnhx52lzfp6nf5jcblnfus6gqreh6ygganbws at 2022-06-27T07:33:44.607Z</Bytes>`
* The signing scheme should be sr25519, and `scheme` should be empty.
  * If this not supported by a signer, then `scheme` should be "ed25519".
* Sign and store the signature in `sig` in binary encoding.

**Verification**

* Recompute the signed message using the details in `signature`.
* Verify `sig` given the scheme, the message, and the public key.

**Legacy signatures before v0.1.4**

If `timestamp == 0`, assume an older version:

* Decode `value` and `signer` from hexadecimal with or without `0x`.
* Then the signed message is either `${CID}` or `<Bytes>${CID}</Bytes>`.

| Field         | Type                               | Label | Description                                                                                                                     |
| ------------- | ---------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------- |
| value         | [bytes](storage-schema.md#bytes)   |       | The cryptographic signature in binary encoding as per the scheme.                                                               |
| signer        | [bytes](storage-schema.md#bytes)   |       | The public key of the signer in binary encoding as per the scheme.                                                              |
| scheme        | [string](storage-schema.md#string) |       | The name of the signature scheme (sr25519, secp256k1, ed25519). Default and recommended value: "" or "sr25519".                 |
| multiHashType | [uint64](storage-schema.md#uint64) |       | The ID of the hashing algorithm as per multiformats/multihash. Default and recommended value: 0 or 0xb220, meaning blake2b-256. |
| timestamp     | [uint64](storage-schema.md#uint64) |       | The timestamp in UNIX milliseconds.                                                                                             |

[Top](storage-schema.md#top)

### signed\_piece.proto

#### SignedPiece

A piece signed by an account. This can be used to verify the intent of the account holder to upload the piece.

| Field     | Type                                        | Label | Description                                              |
| --------- | ------------------------------------------- | ----- | -------------------------------------------------------- |
| piece     | [bytes](storage-schema.md#bytes)            |       | A Piece message serialized in protobuf.                  |
| signature | [Signature](storage-schema.md#pb.Signature) |       | A signature of the piece by the keypair of the uploader. |

[Top](storage-schema.md#top)

### tag.proto

#### Tag

A tag is a `key/value` attribute attached to a piece.

Tags can be used to search or filter pieces in storage. If search is not needed, disable it using the `searchable` field.

Specific tags are used to implement different higher protocols, such as a file system. Each key should start with a prefix indicating which protocol or application relies on it. Below is a non-exhaustive table of known tag keys:

| Tag Key      | Description                                                                                                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------- |
| content-type | The MIME type of the payload of a piece or file. This is returned by the CDN web interface as HTTP Content-Type. |
| file-\*      | Tags to implement a filesystem over object storage. Example: `file-path`                                         |
| kv-\*        | Tags to implement a key/value store over object storage.                                                         |
| enc-\*       | Tags to organize data encryption and data sharing keys.                                                          |

Below is the structure of a tag:

| Field      | Type                                          | Label | Description                                                                          |
| ---------- | --------------------------------------------- | ----- | ------------------------------------------------------------------------------------ |
| key        | [bytes](storage-schema.md#bytes)              |       | The key of the tag. It is usually a UTF-8 string, but it may be any data.            |
| value      | [bytes](storage-schema.md#bytes)              |       | The value of the tag for this key. The value should be interpreted based on the key. |
| searchable | [SearchType](storage-schema.md#pb.SearchType) |       | Whether this tag is searchable or not. Yes by default.                               |

#### SearchType

How can this tag be searched.

| Name            | Number | Description                                                                                                                                                 |
| --------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RANGE           | 0      | RANGE tags should be indexed by nodes into their database to allow efficient queries on a key for an exact value or a range of values. This is the default. |
| NOT\_SEARCHABLE | 1      | NOT\_SEARCHABLE tags should not be indexed. This option should be set when possible to save node space and speed.                                           |

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
