---
description: Learn how to get started with DDC using the SDK Client Library.
---

# ⏱ Quickstart

In order to complete this guide using the DDC client library, you must have an account in Cere Blockchain that have enough CERE tokens (amount depends on your needs) and secret phrase (mnemonic) of that account (required to configure a client).

Use the [🔗 Setup](setup.md) guide to create and top-up an account.

### Installing the client library

{% tabs %}
{% tab title="JavaScript" %}
Latest version of SDK can be found on [releases](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/releases) page.

```
@cere-ddc-sdk --save @cere-ddc-sdk/ddc-client@1.3.3
npm install --save-dev typescript
```

_**package.json**_

```json
{
  "name": "developer-guide",
  "version": "1.0.0",
  "description": "An example illustrating usage of the SDK",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "NAME",
  "license": "ISC",
  "dependencies": {
    "@cere-ddc-sdk/ddc-client": "^1.3.3"
  },
  "devDependencies": {
    "typescript": "^4.7.4"
  },
  "type": "module"
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}

\
Latest version of SDK can be found on [releases](https://github.com/Cerebellum-Network/cere-ddc-sdk-kotlin/releases) page.

_**build.gradle.kts**_

```
repositories {
    maven("https://jitpack.io")
    ivy("https://github.com") {
        patternLayout {
            artifact("[organisation]/releases/download/v[revision]/[module]-[revision].jar")
            setM2compatible(true)
        }
        metadataSources { artifact() }
    }
}

dependencies {
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:ddc-client:1.0.5.Final")
}
```
{% endtab %}
{% endtabs %}

### Setup client

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import {mnemonicGenerate} from "@polkadot/util-crypto";
import {DdcClient, File, Piece, Tag, SearchType, DdcUri, IFILE, IPIECE} from "@cere-ddc-sdk/ddc-client";

const setupClient = async () => {
    // Cluster address is either string (url of the CDN node to use) or number (id of the CDN cluster to use)
    // CDN node addresses and cluster ids can be found here: https://docs.cere.network/testnet/ddc-network-testnet
    const options = {clusterAddress: 2n};

    // The secret phrase is going to be used to sign requests and encrypt/decrypt (optional) data
    // Replace mnemonicGenerate by your secret phrase generated during account setup (see https://docs.cere.network/ddc/developer-guide/setup)
    const secretPhrase = mnemonicGenerate();

    // Initialise DDC client and connect to blockchain
    const ddcClient = await DdcClient.buildAndConnect(options, secretPhrase);
    console.log("DDC Client successfully connected.");

    return ddcClient;
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Create bucket

Bucket concept overview can be found on [Concepts](../concepts.md) page.&#x20;

DDC client setup explained in [Setup client](quickstart.md#setup-client) section.

{% hint style="info" %}
On bucket creation, you pay tokens for the bucket reservation. Please, be careful and don't create unnecessary buckets that can be unused or forgotten.&#x20;
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const createBucket = async () => {
    const ddcClient = await setupClient();

    // Amount of tokens to deposit to the bucket balance
    const balance = 10n;
    // Bucket size in GB
    const size = 5n;
    // Bucket parameters
    const parameters = {
        // Number of copies of each piece. Minimum 1. Maximum 9. Temporary limited to 3. Default 1.
        replication: 3,
    };
    // Id of the storage cluster on which the bucket should be created
    // Storage custer ids can be found here: https://docs.cere.network/testnet/ddc-network-testnet
    const storageClusterId = 1n;

    // Create bucket and return even produced by Smart Contract that contains generated bucketId that should be used later to store and read data
    const bucketCreatedEvent = await ddcClient.createBucket(balance, size, storageClusterId, parameters);
    console.log("Successfully created bucket. Id: " + bucketCreatedEvent.bucketId);
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Store data

Data can be encrypted by client (see [Encryption](quickstart.md#encryption)).

DDC client setup explained in [Setup client](quickstart.md#setup-client) section.

{% hint style="info" %}
Only bucket creator can store data to the bucket. Otherwise, storage returns 403 status code.
{% endhint %}

#### Piece

Piece is the smallest indivisible unit stored in DDC. A piece consists of the data and [tags](../protocols/storage-schema.md) that can be used to search pieces or to store not searchable metadata.&#x20;

{% hint style="info" %}
Max size of the piece is 100 MB. To upload bigger unit of data, see [File](quickstart.md#file) section.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const storePiece = async () => {
    const ddcClient = await setupClient();

    // ID of the bucket in which the piece should be stored
    const bucketId = 2n;

    // Data can be encrypted or not (depends on store options 'encrypt' flag)
    const data = new TextEncoder().encode("Hello world!");
    // Tags can be used to store metadata or to search pieces
    const tags = [
        // Tags are searchable by default. In this example piece can be found by `usedId`
        new Tag("userId", "5868302"),
        // To store metadata, not searchable tags  can be used (they are filtered out during piece indexing)
        new Tag("type", "photo", SearchType.NOT_SEARCHABLE)
    ];

    // Tags are optional
    // Data supported types: Uint8Array
    const piece = new Piece(data, tags);

    const storeOptions = {
        // True - store encrypted data. False - store unencrypted data.
        encrypt: true,
        // If empty or not passed - data will be encrypted by master DEK.
        dekPath: "/documents/personal",
    };

    const ddcUri = await ddcClient.store(bucketId, piece, storeOptions);
    console.log("Successfully uploaded piece. DDC URI: " + ddcUri.toString());
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

#### File

File is a bigger unit of data that is presented as a set of pieces. File pieces are distributed across set of nodes (depends on the cluster size and number of pieces).  See [File Storage](../protocols/file-storage.md) section.

{% hint style="info" %}
There is no hard limit on the max size of the file.
{% endhint %}

On the code level storing of the File is almost the same as storing of the Piece, the difference is in supported data types (depends on the language you use) and internal logic (splitting of the file into pieces).&#x20;

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const storeFile = async () => {
    const ddcClient = await setupClient();

    // ID of the bucket in which the piece should be stored
    const bucketId = 2n;

    // Data can be encrypted or not (depends on store options 'encrypt' flag)
    const data = new TextEncoder().encode("Large string 1 GB in size");
    // Tags can be used to store metadata or to search pieces
    const tags = [
        // Tags are searchable by default. In this example piece can be found by `usedId`
        new Tag("userId", "5868302"),
        // To store metadata, not searchable tags  can be used (they are filtered out during piece indexing)
        new Tag("type", "photo", SearchType.NOT_SEARCHABLE)
    ];

    // Tags are optional
    // Data supported types: ReadableStream<Uint8Array> | string | Uint8Array
    const file = new File(data, tags);

    const storeOptions = {
        // True - store encrypted data. False - store unencrypted data.
        encrypt: true,
        // If empty or not passed - data will be encrypted by master DEK.
        dekPath: "/documents/personal",
    };

    const ddcUri = await ddcClient.store(bucketId, file, storeOptions);
    console.log("Successfully uploaded file. DDC URI: " + ddcUri.toString());
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Read data

Encrypted data can be decrypted by client (see [Encryption](quickstart.md#encryption)).

DDC client setup explained in [Setup client](quickstart.md#setup-client) section.

#### Piece

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const readPiece = async () => {
    const ddcClient = await setupClient();

    const readOptions = {
        // True - return decrypted data. False - return data as is.
        decrypt: true,
        // If empty or not passed - data will be decrypted by master DEK.
        dekPath: "/documents",
    };

    // ID of the bucket from which the piece should be read
    const bucketId = 2n;
    // CID of the piece to read
    const cid = "bafk2bzacecdzr32hb7pq7ksx73hxbn2pgata2anniuwvv5nov67gcznvuou5y";
    const ddcUri = DdcUri.parse(bucketId, cid, IPIECE);

    // DdcUri that have IPIECE protocol returns Piece
    const piece = await ddcClient.read(ddcUri, readOptions);
    console.log("Successfully read piece. CID: " + piece.cid);
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

#### File

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const readFile = async () => {
    const ddcClient = await setupClient();

    const readOptions = {
        // True - return decrypted data. False - return data as is.
        decrypt: true,
        // If empty or not passed - data will be decrypted by master DEK.
        dekPath: "/documents",
    };

    // ID of the bucket from which the file should be read
    const bucketId = 2n;
    // CID of the file to read
    const cid = "bafk2bzacecdzr32hb7pq7ksx73hxbn2pgata2anniuwvv5nov67gcznvuou5y";
    const ddcUri = DdcUri.build(bucketId, cid, IFILE)

    // DdcUri that have IFILE protocol returns File
    const file = await ddcClient.read(ddcUri, readOptions);
    console.log("Successfully read file. CID: " + file.headCid);
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Search pieces

Search is based on [tags](../protocols/storage-schema.md) that are combined using logical 'or' operator (e.g. tagA = N or tagB = M). \
\
Search operation is distributed and result is collected from the nodes of the storage cluster where bucket is stored (because bucket is distributed across all cluster nodes).

DDC client setup explained in [Setup client](quickstart.md#setup-client) section.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const searchPieces = async () => {
     const ddcClient = await setupClient();
     
    // ID of the bucket where to search pieces
    const bucketId = 2n;
    
    // Tags to search pieces
    const tags = [
        new Tag("userId", "5868302"),
        new Tag("userId", "5868303"),
    ];
    // True - return only meatadata (CID and tags). False - return both data and metadata
    const skipData = true;
    const query = {
        bucketId: bucketId,
        tags: tags,
        skipData: skipData
    };
    
    const pieces = await ddcClient.search(query);
    console.log("Successfully searched pieces. CIDS: " + pieces.map(e => e.cid));
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Share data

Data sharing is implemented via DEK re-encryption (see [Encryption](quickstart.md#encryption)).

In order to share data, partner have to share his encryption public key to data owner (generated via his secret phrase).&#x20;

Then partner can read encrypted data having his secret phrase and shared DEK path via DDC Client.

DDC client setup explained in [Setup client](quickstart.md#setup-client) section.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const shareData = async () => {
    const ddcClient = await setupClient();

    // ID of the bucket where to share data
    const bucketId = 2n;

    // Partner encryption public key
    const partnerPublicKeyHex = "0x173319894bc4b7fa6167bd93cbe974f520ec43ef450fb6c6746ecc4c6a86b7de";
    // DEK path to share access to (hierarchical encryption)
    const dekPath = "/documents";

    const ddcUri = await ddcClient.shareData(bucketId, dekPath, partnerPublicKeyHex);
    console.log("Successfully shared data (uploaded EDEK). DDC URI: " + ddcUri.toString());
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Encryption

#### Overview

The DDC Client Library provides encryption/decryption out of the box (using [NaCl](https://nacl.cr.yp.to/)). \
\
Secret phrase passed in DDC Client on setup is used to generate (via blake2b-256) master `DEK`.

`DEK path` per piece can be passed inside `storeOptions` to `store` function. Then DEK path is split by '/' into parts and each part (from left to right) is used to generate DEK based on previous value (master DEK is an initial value).

_Example_

Secret phrase = `super secret phrase`

Master DEK = blake2b-256(`super secret phrase`)

Piece DEK path = `/documents/personal`

Piece DEK = blake2b-256(`personal` + blake2b-256(`documents` + blake2b-256(`super secret phrase`)))

_Explanation_

The recursively (in reverse order) generated DEK allows to implement hierarchical encryption where data encrypted by `/documents/personal`  can be decrypted knowing one of the values:

* blake2b-256(`super secret phrase`)
* blake2b-256(`documents` + blake2b-256(`super secret phrase`))

_Use case example_

Directory based access (file sharing platform)&#x20;

#### Terms

**`blake2b-256`** is a one of the fastest cryptographic hash functions

**`DEK`** is a data encryption key used to encrypt the data (master DEK generated based on secret phrase).

**`EDEK`** is an encrypted data encryption key that is stored in DDC and used to share data (re-encrypt DEK per partner).

**`DEK path`** is a hierarchical encryption path (e.g. /photos/friends) that is used to generate `DEK` recursively (uses master DEK as base) and share data on any level. Similar to directory based access.
