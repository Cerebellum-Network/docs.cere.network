# ☁ CDN API

The CDN API has two parts:

* The web gateway uses plain HTTP.

  **See the specification of [🔗 DDC URLs](ddc-url.md).**

* Other operations use protobuf messages over HTTP.

  **See the model of messages in [☁ Storage Schema](storage-schema.md).**


### Web Gateway

Resolve a query using a `/ddc/` URI and return the content of a file or piece.

#### Request
```http
GET /ddc/…/file/…
GET /ddc/…/ifile/…
GET /ddc/…/piece/…
GET /ddc/…/ipiece/…
```

#### Response
For `file/` and `ifile/` queries, return the file content and a Content-Type header.

For `piece/` and `ipiece/` queries, return the piece data structure encoded in binary ProtoBuf, or in JSON, or only the payload (`piece.data`), depending on options (TODO: specify the options).


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


## Metrics

CDN nodes provide the following metrics for monitoring:

* general HTTP metrics
* number piece viewed

{% hint style="warning" %} TODO: document the API of metrics
{% endhint %}
