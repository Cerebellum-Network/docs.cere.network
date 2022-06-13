# 📂 File Storage

{% hint style="info" %} This is the specification of `File Storage v0.1`
{% endhint %}

Files are stored in DDC as a set of data pieces. A file is represented by a piece which is a file descriptor. The file can be referenced by referencing this descriptor piece, e.g., by its CID. The content of the file is stored in chunks in separate data pieces. The chunk pieces are referenced by CID in the descriptor piece in its `links` field.

A file descriptor piece has this structure:

```js
Piece {
    links: [
        // The list of content pieces.
        Link {
            cid: "…",  // The CID of a content piece.
            size: 123, // The size of the chunk in bytes.
        },
        …
    ],
    tags: [
        // Any tag that can help locate this file,
        // or any file metadata.
        …
    ],
}
```

A content piece has this structure:

```js
Piece {
    data: […bytes…], // A chunk of the file content.
    tags: [
        Tag {
            key: "multipart",
            value: "true",
            searchable: "NOT_SEARCHABLE",
        }
    ]
}
```

See the [☁ Storage Schema](storage-schema.md) for more details about pieces, links, and tags.
