---
description: Example application that allows users to upload, download and share a files.
---

# 🗄 File sharing platform

Use the [🔗 Setup](../setup.md) guide to create and top-up an account.

### Scenario

There are 2 users: Bob and Alice. The scenario flow:

* Bob creates a bucket
* Bob stores private (encrypted) data
* Bob stores public (unencrypted) data
* Bob shares data to Alice
* Alice reads Bob's public data
* Alice reads Bob's private data

{% hint style="warning" %}
Add a link to React demo application GitHub repository
{% endhint %}

### Code example (JavaScript)

<pre class="language-javascript"><code class="lang-javascript"><strong>
</strong>import {DdcClient, Piece} from "@cere-ddc-sdk/ddc-client";
import {TESTNET} from "@cere-ddc-sdk/smart-contract";
import {Tag} from "@cere-ddc-sdk/content-addressable-storage";
import {u8aToHex} from "@polkadot/util";

//Replace MNEMONIC_BOB phrase with one you have containing enough balance
const MNEMONIC_BOB = "carpet purchase luggage away oak use inflict cheese buddy thrive moon scout";
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
    const ddcClientBob = await DdcClient.buildAndConnect({clusterAddress: cdnUrl, smartContract: TESTNET}, MNEMONIC_BOB);
    // Bob creates bucket in cluster
    const {bucketId} = await ddcClientBob.createBucket(10n, 1n,clusterId,{"replication": 3});
    console.log(`New Bucket Id: ${bucketId}`);

    console.log("========================= Store =========================");
    // Store encrypted data
    const secretPiece = new Piece(secretData, [topSecretDataTag]);
    const secretUri = await ddcClientBob.store(bucketId, secretPiece, {encrypt: true, dekPath: subDekPath});
    console.log(`Secret URI: ${secretUri}`)

    // Store unencrypted data
    const publicPiece = new Piece(publicData, [publicDataTag]);
    const publicUri = await ddcClientBob.store(bucketId, publicPiece);
    console.log(`Public URI: ${publicUri}`)


    console.log("========================= Read data for Bob =========================");
    // Read secret data without decryption
    console.log("Read encrypted data for Bob:");
    let piece = await ddcClientBob.read(secretUri);
    await readData(piece);
    console.log("=========================");

    // Read secret decrypted data
    console.log("Read decrypted data for Bob:");
    piece = await ddcClientBob.read(secretUri, {dekPath: subDekPath, decrypt: true});
    await readData(piece);
    console.log("=========================");

    // Read public data
    console.log("Read public data for Bob:");
    piece = await ddcClientBob.read(publicUri);
    await readData(piece);
    console.log("=========================");


    console.log("========================= Share data for Alice =========================");
    // Init Alice client
    const ddcClientAlice = await DdcClient.buildAndConnect({clusterAddress: cdnUrl, smartContract: TESTNET}, MNEMONIC_ALICE );
    // Share DEK
    await ddcClientBob.shareData(bucketId, mainDekPath, u8aToHex(ddcClientAlice.boxKeypair.publicKey));


    console.log("========================= Read data for Alice =========================");
    // Read public data
    console.log("Read public data for Alice:");
    piece = await ddcClientAlice.read(publicUri);
    await readData(piece);
    console.log("=========================");

    // Read secret decrypted data
    console.log("Read decrypted data for Alice:");
    piece = await ddcClientAlice.read(secretUri, {dekPath: mainDekPath, decrypt: true});
    await readData(piece);
    console.log("=========================");

    console.log("========================= Disconnect =========================");
    await ddcClientBob.disconnect();
    await ddcClientAlice.disconnect();
}

const readData = async (piece) => {
    for await (const data of piece.data) {
        console.log(`Data Uint8Array: ${data}`)
    }
}

main().then(() => console.log("DONE")).catch(console.error).finally(() => process.exit())</code></pre>
