# ☁ CDN API

## HTTP API

### Web Gateway

Resolve a query using the `ddc:file` protocol and return the content of a file.

See the specification of [🔗 DDC URLs](ddc-url.md) for the format of the queries.

{% hint style="warning" %} This feature is not yet implemented. {% endhint %}

#### Request
```http
GET /ddc:file/{{file_query}}
```

#### Response
* Status code: 200
* Body: the content of the file
* Headers: Content-Type, Content-Disposition

### Download Piece

#### Request

```http
GET /api/rest/pieces/{{cid}}?bucketId={{bucket_id}}
```

#### Response

* Status code: 200
* Body: _SignedPiece_ model

### Upload piece

#### Request

```http
PUT /api/rest/pieces

{{SignedPiece_model}}
```

#### Response

* Status code: 201
* Body: _Signature_ model

### Search pieces

#### Request

```http
GET /api/rest/pieces

{{Query_model}}
```

#### Response

* Status code: 200
* Bode: _SearchResult_ model


## Data Model

CDN Node uses **protobuf** serialization for communicating and requests.

### Signed Piece

Request model for storing piece

```protobuf
message SignedPiece {
  Piece     piece = 1; // type for storing in DDC 
  Signature signature = 2; // type with signature and parameters
}
```

### Signature

Body for verification request

```protobuf
message Signature {
  string value = 1; // signature value
  string signer = 2; // crypto public key of signing user
  string scheme = 3; // algorithm name for signing Piece hash (sr25519, secp256k1, ed25519)
  uint64 multiHashType = 4; // hash algorithm ID from multicodec table, where 0 is default (blake2b-256)
}
```

### Piece

Body stored in DDC or ahead piece for stored file as many pieces

```protobuf
message Piece {
  bytes        data = 1; // data of piece
  uint64       bucketId = 2; // bucket where piece have to be stored
  repeated Tag tags = 3; // piece tags for search
  repeated Link links = 4; // links to other pieces of file (require when we store files as many pieces)
}
```

### Tag

Key-value object for search

```protobuf
message Tag {
  string key = 1;
  string value = 2;
}
```

### Link

Piece metadata for stored file as many pieces

```protobuf
message Link {
  string cid = 1; // cid of piece
  uint64 size = 2; // size of piece in bytes
  string name = 3; // name of piece (optional)
}
```

### Query

Request body for search pieces by tags

```protobuf
message Query {
  uint64       bucketId = 1; // bucket ID where search pieces
  repeated Tag tags = 2; // tags to search for
}
```

### Search Result

Response body for search pieces

```protobuf
message SearchResult {
  repeated SignedPiece signedPieces = 1; // signed pieces by DDC node
}
```


## Metrics

CDN nodes provide the following metrics for monitoring:

* general HTTP metrics
* number piece viewed

{% hint style="warning" %} TODO: document the API of metrics
{% endhint %}
