# Storage Schema

This is the data model for the API of DDC Storage.


([Source](https://github.com/Cerebellum-Network/ddc-schemas))


## Changelog

### vNext
- Tag values are bytes instead of strings.

### v0.1.3

- Piece and query model.
- As implemented by the [Go SDK v0.1.3](https://github.com/Cerebellum-Network/cere-ddc-sdk-go/releases/tag/v0.1.3)
- Inline documentation.


# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [link.proto](#link.proto)
  - [Link](#pb.Link)

- [piece.proto](#piece.proto)
  - [Piece](#pb.Piece)

- [query.proto](#query.proto)
  - [Query](#pb.Query)

- [search_result.proto](#search_result.proto)
  - [SearchResult](#pb.SearchResult)
  - [SearchedPiece](#pb.SearchedPiece)

- [signature.proto](#signature.proto)
  - [Signature](#pb.Signature)

- [signed_piece.proto](#signed_piece.proto)
  - [SignedPiece](#pb.SignedPiece)

- [tag.proto](#tag.proto)
  - [Tag](#pb.Tag)

- [Scalar Value Types](#scalar-value-types)



<a name="link.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## link.proto



<a name="pb.Link"></a>

### Link
A link is a pointer from one piece to another.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cid | [string](#string) |  | The CID of the linked piece. |
| size | [uint64](#uint64) |  | The size of the payload of the linked piece. |
| name | [string](#string) | optional | The name of the linked piece, in the context of the linking piece. |















<a name="piece.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## piece.proto



<a name="pb.Piece"></a>

### Piece
A piece is a container of data and metadata.
It is the smallest indivisible unit stored in DDC object storage.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| data | [bytes](#bytes) |  | The opaque payload carried by the piece. |
| bucketId | [uint32](#uint32) |  | The ID of a bucket that contains the piece. |
| tags | [Tag](#pb.Tag) | repeated | A list of tags with which the piece may be searched. There can be multiple tags with the same key. |
| links | [Link](#pb.Link) | repeated | A list of links to other pieces. If this piece is interpreted as a file, the linked pieces make up the file content. |















<a name="query.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## query.proto



<a name="pb.Query"></a>

### Query
A query represents a search of pieces by tags.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| bucketId | [uint32](#uint32) |  | The ID of the bucket to search. |
| tags | [Tag](#pb.Tag) | repeated | A list of tags to match against the tags of stored pieces. There can be multiple tags with the same key. |
| loadData | [bool](#bool) |  | Return search result with piece data |















<a name="search_result.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## search_result.proto



<a name="pb.SearchResult"></a>

### SearchResult
A search result contains the pieces found from a search.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| searchedPieces | [SearchedPiece](#pb.SearchedPiece) | repeated | The list of pieces found in storage. |






<a name="pb.SearchedPiece"></a>

### SearchedPiece
A searched piece found in storage.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| signedPiece | [SignedPiece](#pb.SignedPiece) |  | Found signed piece. |
| cid | [string](#string) |  | CID of the found piece. |















<a name="signature.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## signature.proto



<a name="pb.Signature"></a>

### Signature
A signature and details to help verify it.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [string](#string) |  | A cryptographic signature. |
| signer | [string](#string) |  | The public key of the signer. |
| scheme | [string](#string) |  | The name of the signature scheme (sr25519, secp256k1, ed25519). |
| multiHashType | [uint64](#uint64) |  | The ID of the hashing algorithm as per multiformats/multicodec. Default: 0, meaning blake2b-256. |















<a name="signed_piece.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## signed_piece.proto



<a name="pb.SignedPiece"></a>

### SignedPiece
A piece signed by an account.
This can be used to verify the intent of the account holder to upload the piece.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| piece | [Piece](#pb.Piece) |  | A piece. |
| signature | [Signature](#pb.Signature) |  | A signature of the piece by the keypair of the uploader. |















<a name="tag.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## tag.proto



<a name="pb.Tag"></a>

### Tag
A tag is an attribute attached to pieces.
Tags can be used to search or filter pieces in storage.

Specific tags are used to implement different higher protocols, such as a file system.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  | The key of the tag. |
| value | [bytes](#bytes) |  | The value of the tag for this key. |















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

