---
description: >-
  These examples demonstrates several common DDC SDK use-cases for NodeJs
  applications.
---

# 👨‍💻 NodeJS

All examples use the account (wallet) established in the [setup.md](../setup.md "mention") guide.

## Create a bucket

This code snippet demonstrates how to create a new public bucket on the DDC Testnet.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { DdcClient, TESTNET } from '@cere-ddc-sdk/ddc-client';

const CERE = 10_000_000_000n;

/**
 * The wallet should have enough CERE to pay for the transaction fees
 */
const user = 'gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory';

/**
 * The DDC cluster where the bucket will be created
 */
const clusterId = '0x825c4b2352850de9986d9d28568db6f0c023a1e3';

/**
 * Create a DDC client instance and connect it to DDC TESTNET
 */
const client = await DdcClient.create(user, TESTNET);

/**
 * Get current deposit of the DDC customer
 */
const deposit = await client.getDeposit();
console.log('Current deposit', deposit / CERE, 'CERE');

/**
 * If the deposit is 0, deposit 5 CERE
 */
if (deposit === 0n) {
  await client.depositBalance(5n * CERE);
  console.log('Deposited 5 CERE');
}

/**
 * Create a public bucket
 */
const bucketId = await client.createBucket(clusterId, {
  isPublic: true,
});

console.log('Public bucket created', bucketId);
console.log('Copy the bucket ID and use it in other examples');

/**
 * Disconnect the client
 */
await client.disconnect();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
A runnable version of this example is available in [the examples directory](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/examples/node).
{% endhint %}

## Store and read a file

This code snippet demonstrates how to upload and download a file from a bucket on the DDC Testnet.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import path from 'path';
import fs from 'fs';
import { fileURLToPath } from 'url';
import { Readable } from 'stream';
import { pipeline } from 'stream/promises';
import { DdcClient, File, TESTNET } from '@cere-ddc-sdk/ddc-client';

/**
 * The wallet should have enough CERE to pay for the transaction fees
 */
const user = 'gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory';

/**
 * The DDC bucket where the file will be stored
 */
const bucketId = 12068n;

/**
 * Detect the current directory
 */
const dir = path.dirname(fileURLToPath(import.meta.url));

/**
 * The file to be stored
 */
const inputFilePath = path.resolve(dir, '2mb.jpg');

/**
 * The path to store the downloaded file
 */
const outputFilePath = path.resolve(dir, '2mb-downloaded.jpg');

/**
 * Create a DDC client instance and connect it to DDC TESTNET
 */
const client = await DdcClient.create(user, TESTNET);

/**
 * Read the file stats
 */
const inputFileStats = fs.statSync(inputFilePath);

/**
 * Create the file read stream
 */
const inputFileStream = fs.createReadStream(inputFilePath);

/**
 * Create a DDC file instance
 */
const ddcFile = new File(inputFileStream, {
  size: inputFileStats.size,
});

/**
 * Store the file into DDC
 */
const fileUri = await client.store(bucketId, ddcFile);
console.log('File stored into bucket', bucketId, 'with CID', fileUri.cid);
console.log('The file can be accessed by this URL', `https://storage.testnet.cere.network/${bucketId}/${fileUri.cid}`);

/**
 * Read the file from DDC
 */
const fileResponse = await client.read(fileUri);
const outputFileStream = fs.createWriteStream(outputFilePath);

/**
 * Stream the content to the destination file
 */
await pipeline(Readable.fromWeb(fileResponse.body), outputFileStream);
console.log('The file downloaded to', path.basename(outputFilePath));

/**
 * Disconnect the client
 */
await client.disconnect();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
A runnable version of this example is available in [the examples directory](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/examples/node).
{% endhint %}

## Upload a website

This code snippet demonstrates how to a static Web Application (eg. website) to DDC Testnet.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import path from 'path';
import fs from 'fs/promises';
import { Stats, createReadStream } from 'fs';
import { fileURLToPath } from 'url';
import { DdcClient, File, Link, DagNode, TESTNET } from '@cere-ddc-sdk/ddc-client';

/**
 * The wallet should have enough CERE to pay for the transaction fees
 */
const user = 'gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory';

/**
 * The DDC bucket where the file will be stored
 */
const bucketId = 12068n;

/**
 * Detect the current directory
 */
const rootDir = path.dirname(fileURLToPath(import.meta.url));

/**
 * The website directory to be uploaded
 */
const websiteDir = path.resolve(rootDir, 'website');

/**
 * The website CNS name
 */
const websiteCnsName = 'website-example';

/**
 * Create a DDC client instance and connect it to DDC TESTNET
 */
const client = await DdcClient.create(user, TESTNET);

/**
 * Uploads a file into DDC and returns a DAG link to it
 */
async function uploadFile(filePath: string, { size }: Stats) {
  const fileStream = createReadStream(filePath);
  const file = new File(fileStream, { size });

  const { cid } = await client.store(bucketId, file);

  console.log('File:', path.relative(rootDir, filePath), 'stored into bucket', bucketId, 'with CID', cid);

  return new Link(cid, size, path.relative(websiteDir, filePath));
}

/**
 * Traverse the website directory and upload all files into DDC. Returns a list of DAG links to the files.
 */
async function uploadDir(dir = websiteDir) {
  const links: Promise<Link>[] = [];
  const files = await fs.readdir(dir);

  for (const fileName of files) {
    const filePath = path.join(dir, fileName);
    const stats = await fs.stat(filePath);
    const subLinks = stats.isDirectory() ? await uploadDir(filePath) : [uploadFile(filePath, stats)];

    links.push(...subLinks);
  }

  return links;
}

console.log('Uploading website from', websiteDir, 'to bucket', bucketId);
const fileLinks = await Promise.all(await uploadDir(websiteDir));

/**
 * Create and store a DAG node with the list of links to the website files
 */
const websiteNode = new DagNode('v1.0.0', fileLinks);
const websiteNodeUri = await client.store(bucketId, websiteNode, {
  name: websiteCnsName,
});

console.log('Website uploaded into bucket', bucketId, 'with CID', websiteNodeUri.cid);
console.log(
  'The website can be accessed by this URL',
  `https://cdn.testnet.cere.network/${bucketId}/${websiteCnsName}`,
);

/**
 * Disconnect the client
 */
await client.disconnect();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
A runnable version of this example is available in [the examples directory](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/examples/node).
{% endhint %}

## Stream events

This code snippet demonstrates how to use a Direct Acyclic Graph (DAG) to create an events timeline stored on the DDC Testnet.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { DdcClient, Link, DagNode, TESTNET, DagNodeUri } from '@cere-ddc-sdk/ddc-client';

/**
 * The wallet should have enough CERE to pay for the transaction fees
 */
const user = 'gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory';

/**
 * The DDC bucket where the file will be stored
 */
const bucketId = 12068n;

/**
 * The website CNS name
 */
const streamCnsName = 'events-stream-example';

/**
 * Create a DDC client instance and connect it to DDC TESTNET
 */
const client = await DdcClient.create(user, { ...TESTNET, logLevel: 'silent' });

/**
 * Get the previous event root attached to the stream CNS record
 */
const prevEvent = await client.read(new DagNodeUri(bucketId, streamCnsName)).catch(() => null);

/*
 * Create a new root event with a link to the previous event root (if exists)
 */
const rootEvent = new DagNode(
  JSON.stringify({
    type: 'example-run',
    time: new Date().toISOString(),
  }),
  prevEvent ? [new Link(prevEvent.cid, prevEvent.size)] : [],
);

/**
 * Store the new root event to the DDC and update the stream CNS record
 */
const rootEventUri = await client.store(bucketId, rootEvent, {
  name: streamCnsName,
});

console.log('Event stream stored into bucket', bucketId, 'with CID', rootEventUri.cid);

/**
 * Recursevly read the last 5 events from the DDC starting from the root event (the latest one)
 */
async function readEventNodes(rootNodeUri: DagNodeUri, depth = 5): Promise<DagNode[]> {
  const rootNode = await client.read(rootNodeUri);
  const [prevNodeLink] = rootNode.links;

  if (!prevNodeLink || depth === 1) {
    return [rootNode];
  }

  const prevNodeUri = new DagNodeUri(bucketId, prevNodeLink.cid);
  const prevNodes = await readEventNodes(prevNodeUri, depth - 1);

  return [rootNode, ...prevNodes];
}

const allNodes = await readEventNodes(rootEventUri);

console.log(
  'All events in the stream:',
  allNodes.map((event) => JSON.parse(event.data.toString())),
);

/**
 * Disconnect the client
 */
await client.disconnect();
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
A runnable version of this example is available in [the examples directory](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/examples/node).
{% endhint %}
