# 📂 File System

{% hint style="info" %} This is the specification of `File System v0.1` based on `Storage Schema v0.1.4`
{% endhint %}

Files can be organized in DDC, like in familiar file systems, in a hierarchy of folders and files.
The file path is included in the piece of the file descriptor (see [📂 File Storage](file-storage.md)).

The file can then be accessed by [🔗 `/file/` URLs](ddc-url.md#file-queries). If multiple versions of the file exist, the most recent file will be returned.

Alternatively, all versions of the file can be retrieved with a search query in the [☁ CDN API](cdn-api.md#search-pieces).


```javascript
Piece {
    tags: [
        // An indexed tag specifies the path of this file.
        Tag {
            key: "file-path",
            value: "… the path separated by slashes …",
        },

        // A non-indexed tag specifies the timestamp.
        // The timestamp should be in UNIX milliseconds, encoded as a ProtoBuf VarInt.
        Tag {
            key: "timestamp",
            value: "… the timestamp …",
            searchable: "NOT_SEARCHABLE",
        },
    ],

    // The rest of the data structure about the file content.
    …
}
```
