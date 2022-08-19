# 📂 File Storage

{% hint style="info" %} This is the specification of `File Storage v0.1` based on `Storage Schema v0.1.4`
{% endhint %}

Files are stored in DDC as a set of data pieces. A file is represented by a piece which is a file descriptor. The file can be referenced by referencing this descriptor piece, e.g., by its CID or its tags.

The content of the file is stored in chunks in the descriptor piece and/or in separate data pieces.
The first chunk may be stored inline in the descriptor piece, or not.
Additional chunks, if any, may be stored in data pieces that must be uploaded before the descriptor piece. The data pieces are referenced by their CID in the `links` field of the descriptor piece. The data pieces are to be found in the same context as the descriptor piece, i.e., in the same bucket and cluster, although usually not on the same node.

The details of content chunking is the responsibility of the uploader.
The inline chunk should be empty or small to allow fast access to the descriptor.
Considering that a piece is the smallest unit that can be retrieved at once, the size and number of chunks should be chosen to optimize between parallelism, smoothness of streaming, memory usage, and overhead per chunk. Here are some possible use-cases:
* **Small files:** inline chunk only; this is the most efficient in space and speed.
* **Big files:** all content is in linked data pieces. The file descriptor is the smallest possible.
* **Streamable files**: access to the start of the content as early as possible. The inline chunk may be used to detect the type, or metadata, or preview of the content (video thumbnail), or other application-specific filters. Other chunks should be small enough to allow smooth seeking in media players.

A file descriptor piece has this structure:

```javascript
Piece {
    // The first chunk of the file content. Optional.
    data: […bytes…],

    // A list of pieces with additional chunks, in order. Optional.
    links: [
        Link {
            // The CID of a content piece.
            cid: "bafk2bzacecacrwzaouolb5ldvevszsxisnbokkcxr6byqgsb64djse4uyzvnu",
            // The size of the chunk in bytes (`Piece.data`). Optional.
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

```javascript
Piece {
    data: […bytes…], // A chunk of the file content.
}
```

See the [☁ Storage Schema](storage-schema.md) for more details about pieces, links, and tags.
