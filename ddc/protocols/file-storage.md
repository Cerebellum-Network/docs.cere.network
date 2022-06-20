# 📂 File Storage

{% hint style="info" %} This is the specification of `File Storage v0.1`
{% endhint %}

Files are stored in DDC as a set of data pieces. A file is represented by a piece which is a file descriptor. The file can be referenced by referencing this descriptor piece, e.g., by its CID or its tags. The content of the file is stored in chunks in separate data pieces. The chunk pieces are referenced by CID in the descriptor piece in its `links` field. The chunk pieces are to be found in the same context as the descriptor piece, i.e., same bucket and same cluster.

A file descriptor piece has this structure:

```js
Piece {
    links: [
        // The list of content pieces, in order.
        Link {
            // The CID of a content piece.
            cid: "bafk2bzacecacrwzaouolb5ldvevszsxisnbokkcxr6byqgsb64djse4uyzvnu",
            // The size of the chunk in bytes (optional).
            size: 303809,
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
}
```

See the [☁ Storage Schema](storage-schema.md) for more details about pieces, links, and tags.
