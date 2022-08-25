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


## File Maps

A set of files can be summarized into a file map. This is similar to a tree of directories and files in regular file systems. A map associates relative file paths to the DDC URIs of the target files. The URIs may be absolute (`/ddc/…`), or relative to the context of the map, in which case the URI may be at the minimum the CIDs of the files.

The map format is as follows:

```json
{
    "fileMap": {
        "…FILE PATH…": "…DDC URI…",
    }
}
```

A map can be used to package a web application. This technique is used in particular by the [DDC Web Loader](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/packages/web-loader). Example:

```json
{
    "fileMap": {
        "index.html": "some_cid_123",
        "js/main.js": "some_cide_456",
        "images/logo.png": "some_cide_789",
    }
}
```

A map may be uploaded itself as a DDC file, and its content holds the map. A map file is immutable, but it is possible to upload multiple versions of a map, distinguished by their timestamp.
