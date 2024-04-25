---
description: Learn how to get started with the DDC using the SDK Client library.
---

# ⏱️ Quickstart

In order to complete this guide using the [DDC Client library](https://www.npmjs.com/package/@cere-ddc-sdk/ddc-client), you should have an account in Cere blockchain that has enough CERE tokens (amount depends on your needs) and the secret phrase (mnemonic) of that account (required to configure the client).

Use the [🔗 Setup](setup.md) guide to create and top-up an account.

## Installation

{% tabs %}
{% tab title="Node JS" %}
Latest version of SDK can be found on [releases](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/releases) page.

Using `NPM`:

```json
npm install @cere-ddc-sdk/ddc-client --save
```

Using `yarn`:

```
yarn add @cere-ddc-sdk/ddc-client
```
{% endtab %}
{% endtabs %}

## Usage

A quick guide of how to upload a file to DDC Testnet using the `DdcClient` API.

### Create a `DdcClient` instance

To initialize the client we will use the account (wallet) created in [setup.md](setup.md "mention") guide

{% tabs %}
{% tab title="TypeScript" %}
```javascript
import { DdcClient, TESTNET } from '@cere-ddc-sdk/ddc-client';

const seedPhrase = 'gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory';
const ddcClient = await DdcClient.create(seedPhrase, TESTNET);
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The account used to create the instance should have positive balance and DDC deposit
{% endhint %}

### Create a bucket

{% tabs %}
{% tab title="TypeScript" %}
Private bucket (by default)

```typescript
const bucketId = await client.createBucket(clusterId);
```

or a public one

```typescript
const bucketId = await client.createBucket(clusterId, { isPublic: true });
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Creating a bucket results in a transaction on the Cere blockchain, and thus a portion of your wallet balance will be used to pay the associated fees.
{% endhint %}

### Upload a file

To optimize memory consumption, we will read the file from the file system using NodeJS readable stream.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import * as fs from 'fs';
import { File } from '@cere-ddc-sdk/ddc-client';

const filePath = './my-picture.jpg';
const fileStats = fs.statSync(filePath);
const fileStream = fs.createReadStream(filePath);
const file = new File(fileStream, { size: fileStats.size });

const { cid: fileCid } = await ddcClient.store(bucketId, file);

console.log('The uploaded file CID', fileCid)
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
As a result, you will receive the unique Content Identifier (CID) of the uploaded file. This CID will be used later to access the file on the DDC.
{% endhint %}

### Share the file

DDC offers Web API access to files, allowing them to be shared via URLs.

{% tabs %}
{% tab title="TypeScript" %}
If the file's bucket is private

```typescript
import { AuthTokenOperation } from '@cere-ddc-sdk/ddc-client';

const accessToken = await ddcClient.grantAccess({
  bucketId,
  pieceCid: fileCid,
  operations: [AuthTokenOperation.GET],
});

const fileUrl = `https://cdn.testnet.cere.network/${bucketId}/${fileCid}?token=${accessToken}`;

console.log('The file URL', fileUrl);
```

or public

```typescript
const fileUrl = `https://cdn.testnet.cere.network/${bucketId}/${fileCid}`;

console.log('The file URL', fileUrl);
```
{% endtab %}
{% endtabs %}

It is also possible to download the file by its CID using the SDK Client

{% tabs %}
{% tab title="TypeScript" %}
To optimize memory consumption, we will store the file to the file system using NodeJS writable stream.

```typescript
import * as fs from 'fs';
import { pipeline } from 'stream/promises';
import { Readable } from 'stream';
import { FileUri } from '@cere-ddc-sdk/ddc-client';

const outputFilePath = './my-downloaded-picture.jpg';

const uri = new FileUri(bucketId, fileCid);
const fileResponse = await ddcClient.read(uri, { accessToken });
const outputFileStream = fs.createWriteStream(outputFilePath);

/**
 * Stream the content from DDC to the destination file
 */
await pipeline(Readable.fromWeb(fileResponse.body), outputFileStream);

console.log('The file downloaded to', outputFilePath);
```
{% endtab %}
{% endtabs %}

***

For more information about what this package provides, see [API reference](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/blob/main/packages/ddc-client/docs/README.md)
