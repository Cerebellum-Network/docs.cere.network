# ⏱ Quick Start

{% tabs %}
{% tab title="Javascript" %}
{% hint style="info" %}
More examples and details in [JS SDK README](https://github.com/Cerebellum-Network/cere-ddc-sdk-js)
{% endhint %}

### Dependencies

package.json

```json
{
  "@cere-ddc-sdk/content-addressable-storage": "1.2.0",
  "@cere-ddc-sdk/core": "1.2.0",
  "@cere-ddc-sdk/ddc-client": "1.2.0",
  "@cere-ddc-sdk/file-storage": "1.2.0",
  "@cere-ddc-sdk/key-value-storage": "1.2.0",
  "@cere-ddc-sdk/proto": "1.2.0",
  "@cere-ddc-sdk/smart-contract": "1.2.0",
}
```

### Code

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
    const ddcClientBob = await DdcClient.buildAndConnect(MNEMONIC_BOB, {clusterAddress: cdnUrl, smartContract: DEVNET});
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

Output:

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
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="info" %}
More examples and details in [Kotlin SDK README](https://github.com/Cerebellum-Network/cere-ddc-sdk-kotlin)
{% endhint %}

### Create bucket

#### Dependencies

build.gradle.kts

```kts
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
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:smart-contract:1.0.4.Final")
}
```

#### Code

```kotlin
import network.cere.ddc.contract.BucketContractConfig
import network.cere.ddc.contract.BucketSmartContract
import network.cere.ddc.contract.blockchain.BlockchainConfig
import network.cere.ddc.contract.model.Balance
import java.math.BigDecimal

// Network settings
const val URL = "wss://rpc.testnet.cere.network:9945";
const val CONTRACT_ADDRESS = "5DAx9cTNXYKbbMTQUWzh1cZ46Mj14pnyKvshkVWm8fkfh36X";
// User settings
const val SEED = "0x3be19e0bba3af20bad16298976ec27e25d9330cd997634abb09cb101a0387e8b";
// DDC Settings
const val CLUSTER_ID = 0L;

suspend fun main() {
    val config = BlockchainConfig(URL, CONTRACT_ADDRESS, SEED)
    val contractConfig = BucketContractConfig()

    val contract = BucketSmartContract.buildAndConnect(config, contractConfig)
    println("Connected to blockchain");

    // value can be 0, but substrate blockchain network v2.0.0 has bug, so with low value transaction can be failed
    val balance = Balance(BigDecimal("10"))

    println("Create a bucket...")
    val bucketCreatedEvent = contract.bucketCreate(balance, "{\"replication\": 3}", CLUSTER_ID)
    println("New BucketId ${bucketCreatedEvent.bucketId}")
}
```

Output:

```
Connected to blockchain
Create a bucket...
New BucketId 6
```

### Upload and Download

#### Dependencies

build.gradle.kts

```kts
dependencies {
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:core:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:proto:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:content-addressable-storage:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:file-storage:1.0.4.Final")
}
```

#### Code

```kotlin
import network.cere.ddc.core.signature.Scheme
import network.cere.ddc.storage.config.FileStorageConfig
import java.nio.file.Paths

const val BUCKET_ID = 6L
const val CDN_NODE_URL = "https://node-0.cdn.testnet.cere.network"
const val SEED = "0x3be19e0bba3af20bad16298976ec27e25d9330cd997634abb09cb101a0387e8b"

suspend fun main() {
    val scheme = Scheme.create(Scheme.SR_25519, SEED)
    val fileStorage = FileStorage(scheme, CDN_NODE_URL, FileStorageConfig(parallel = 2, chunkSizeInBytes = 3))

    // Upload file
    val filePath = Paths.get("test.txt")
    val uri = fileStorage.upload(BUCKET_ID, filePath)
    println(uri)

    // Download file
    val newFilePath = Paths.get("test_new.txt")
    fileStorage.download(BUCKET_ID, uri.cid, newFilePath)
}
```
{% endtab %}
{% endtabs %}
