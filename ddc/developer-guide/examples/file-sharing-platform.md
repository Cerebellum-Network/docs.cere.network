---
description: Example application that allows users to upload, download and share a files.
---

# 🗄 File sharing platform

### Scenario

There are 2 users Bob and Alice. The scenario flow is next:

* Bob creates a bucket&#x20;
* Bob stores private (encrypted) data
* Bob stores public (unencrypted) data
* Bob shares data to Alice
* Alice reads Bob's public data
* Alice read Bob's private data

{% hint style="warning" %}
Add a link to React demo application GitHub repository
{% endhint %}

### Code example (JavaScript)

```javascript
const {DdcClient, PieceArray} = require("@cere-ddc-sdk/ddc-client");
const {TESTNET} = require("@cere-ddc-sdk/smart-contract");
const {Tag} = require("@cere-ddc-sdk/content-addressable-storage");
const {u8aToHex} = require("@polkadot/util");

const MNEMONIC_BOB = "grass smooth rain offer matter senior crucial slim clip news town opera";
const MNEMONIC_ALICE = "little absent donor alcohol dynamic unit throw laptop boring tissue pen design";
const clusterId = 0n;
const cdnUrl = "https://node-0.v2.us.cdn.testnet.cere.network";

// Secret Bob's Data
const secretData = new Uint8Array([0, 1, 2, 3, 4, 5]);
const topSecretDataTag = new Tag("data-status", "top_secret");

// Public Bob's Data
const publicData = new Uint8Array([10, 11, 12, 13, 14, 15]);
const publicDataTag = new Tag("data-status", "public");

// DEK paths
const mainDekPath = "bob/data/secret";
const subDekPath = "bob/data/secret/some";

const main = async () => {
    console.log("========================= Initialize =========================");
    // Init Bob client
    const ddcClientBob = await DdcClient.buildAndConnect(MNEMONIC_BOB, {clusterAddress: cdnUrl, smartContract: TESTNET});
    // Bob creates bucket in cluster
    const {bucketId} = await ddcClientBob.createBucket(10n, '{"replication": 3}', clusterId);
    console.log(`New Bucket Id: ${bucketId}`);

    console.log("========================= Store =========================");
    // Store encrypted data
    const secretPieceArray = new PieceArray(secretData, [topSecretDataTag]);
    const secretUri = await ddcClientBob.store(bucketId, secretPieceArray, {encrypt: true, dekPath: subDekPath});
    console.log(`Secret URI: ${secretUri}`)

    // Store unencrypted data
    const publicPieceArray = new PieceArray(publicData, [publicDataTag]);
    const publicUri = await ddcClientBob.store(bucketId, publicPieceArray);
    console.log(`Public URI: ${publicUri}`)


    console.log("========================= Read data for Bob =========================");
    // Read secret data without decryption
    console.log("Read encrypted data for Bob:");
    let pieceArray = await ddcClientBob.read(secretUri);
    await readData(pieceArray);
    console.log("=========================");

    // Read secret decrypted data
    console.log("Read decrypted data for Bob:");
    pieceArray = await ddcClientBob.read(secretUri, {dekPath: subDekPath, decrypt: true});
    await readData(pieceArray);
    console.log("=========================");

    // Read public data
    console.log("Read public data for Bob:");
    pieceArray = await ddcClientBob.read(publicUri);
    await readData(pieceArray);
    console.log("=========================");


    console.log("========================= Share data for Alice =========================");
    // Init Alice client
    const ddcClientAlice = await DdcClient.buildAndConnect(MNEMONIC_ALICE, {
        clusterAddress: cdnUrl,
        smartContract: DEVNET
    });
    // Share DEK
    await ddcClientBob.shareData(bucketId, mainDekPath, u8aToHex(ddcClientAlice.boxKeypair.publicKey));


    console.log("========================= Read data for Alice =========================");
    // Read public data
    console.log("Read public data for Alice:");
    pieceArray = await ddcClientAlice.read(publicUri);
    await readData(pieceArray);
    console.log("=========================");

    // Read secret decrypted data
    console.log("Read decrypted data for Alice:");
    pieceArray = await ddcClientAlice.read(secretUri, {dekPath: mainDekPath, decrypt: true});
    await readData(pieceArray);
    console.log("=========================");
    
    console.log("========================= Disconnect =========================");
    await ddcClientBob.disconnect();
    await ddcClientAlice.disconnect();
}

const readData = async (pieceArray) => {
    for await (const data of pieceArray.dataReader()) {
        console.log(`Data Uint8Array: ${data}`)
    }
}

main().then(() => console.log("DONE")).catch(console.error).finally(() => process.exit());
```

#### Output

```
========================= Initialize =========================
New Bucket Id: 1
========================= Store =========================
Secret URI: cns://1/bafk2bzacedvqeirtgwjaa64e2c4rnck3cvycgr43ux5y4lnbn3l7b6byzx6mk
Public URI: cns://1/bafk2bzaceae7zjzybt34brsbmck2uovjg5ixg5weikxbhglq3gzqjst2kyvki
========================= Read data for Bob =========================
Read encrypted data for Bob:
Data Uint8Array: 198,59,143,241,223,237,145,22,182,37,64,78,166,136,188,213,81,142,109,6,180,35
=========================
Read decrypted data for Bob:
Data Uint8Array: 0,1,2,3,4,5
=========================
Read public data for Bob:
Data Uint8Array: 10,11,12,13,14,15
=========================
========================= Share data for Alice =========================
========================= Read data for Alice =========================
Read public data for Alice:
Data Uint8Array: 10,11,12,13,14,15
=========================
Read decrypted data for Alice:
Data Uint8Array: 0,1,2,3,4,5
=========================
========================= Disconnect =========================
DONE
```
