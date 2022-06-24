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
npm install --save @cere-ddc-sdk/ddc-client@1.2.6
```

_**package.json**_

```json
{
  "@cere-ddc-sdk/ddc-client": "1.2.6"
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
import {DdcClient} from "@cere-ddc-sdk/ddc-client";

// Cluster address is either string (url of the CDN node to use) or number (id of the CDN cluster to use)
// CDN node addresses and cluster ids can be found here: https://docs.cere.network/testnet/ddc-network-testnet
const options = {clusterAddress: 2n};

// The secret phrase is going to be used to sign requests and encrypt/decrypt (optional) data
// Replace mnemonicGenerate by your secret phrase generated during account setup (see https://docs.cere.network/ddc/developer-guide/setup)
const secretPhrase = mnemonicGenerate();

// Initialise DDC client and connect to blockchain
const ddcClient = DdcClient.buildAndConnect(options, secretPhrase);
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

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const createBucket = async () => {
    // Amount of tokens to deposit to the bucket balance
    const balance = 10n;
    // Bucket parameters
    // 'replication' is a number of copies of each piece. Minimum 1. Maximum 9.
    const parameters = `{"replication": 3}`;
    // Id of the storage cluster on which the bucket should be created
    // Storage custer ids can be found here: https://docs.cere.network/testnet/ddc-network-testnet
    const storageClusterId = 1n;
    
    // Create bucket and return even produced by Smart Contract that contains generated bucketId that should be used later to store and read data  
    const bucketCreatedEvent = await ddcClient.createBucket(balance, parameters, storageClusterId);
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

### Store piece

Piece is the smallest indivisible unit stored in DDC. A piece consists of the data and [tags](../protocols/storage-schema.md) that can be used to search pieces or to store not searchable metadata .&#x20;

Data can be encrypted by client (see [Encryption](quickstart.md#encryption))

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import {Piece} from "@cere-ddc-sdk/ddc-client";
import {Tag, SearchType} from "@cere-ddc-sdk/core";

const storeData = async () => {    
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
    
    // Tags and links are optional
    const piece = new Piece(data, tags, links);
    
    const storeOptions = {
        // True - store encrypted data. False - store unencrypted data.
        encrypt: true
        // If empty or not passed - data will be encrypted by master DEK.
        dekPath: "/documents/personal",
    };
    
    const pieceUri = await ddcClient.store(bucketId, piece, storeOptions);
    console.log("Successfully uploaded unencrypted piece. CID: " + pieceUri.cid);
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="warning" %}
At the moment Kotlin SDK is outdated :cry:
{% endhint %}
{% endtab %}
{% endtabs %}

### Read piece

Encrypted data can be decrypted by client (see [Encryption](quickstart.md#encryption)).

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import {PieceUri} from "@cere-ddc-sdk/content-addressable-storage";

const readData = async () => {
    const readOptions = {
        // True - return decrypted data. False - return data as is.
        decrypt: true
        // If empty or not passed - data will be decrypted by master DEK.
        dekPath: "/documents",
    };
    
    // ID of the bucket from which the piece should be read
    const bucketId = 2n;
    // CID of the piece to read
    const cid = "bafk2bzacecdzr32hb7pq7ksx73hxbn2pgata2anniuwvv5nov67gcznvuou5y";
    const pieceUri = {
        bucketId: bucketId,
        cid: cid
    };
    
    const piece = await ddcClient.read(pieceUri);
    console.log("Successfully read data. CID: " + piece.cid);
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

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const searchPieces = async () => {
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

In order to share data to partner, partner have to share his encryption public key to data owner (generated via his secret phrase).&#x20;

Then partner can read encrypted data having his secret phrase and shared DEK path via DDC Client.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const shareData = async (bucketId: bigint) => {
    // Partner encryption public key
    const partnerPublicKeyHex = "0xkldaf3a8as2109...";
    // DEK path to share access to (hierarchical encryption)
    const dekPath = "/photos";
    
    const edekUri = await ddcClient.shareData(bucketId, "/photos", partnerPublicKeyHex);
    console.log("Successfully shared data (uploaded EDEK). CID: " + edekUri.cid);
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

The DDC Client Library provides encryption/decryption out of the box. \
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