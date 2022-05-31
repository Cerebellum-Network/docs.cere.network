# ⏱ Quick Start

Simple example for uploading data from `test.txt` file with some text and download file by chunks with size 3 bytes.

{% tabs %}
{% tab title="Javascript" %}
{% hint style="info" %}
More examples and details in [JS SDK README](https://github.com/Cerebellum-Network/cere-ddc-sdk-js)
{% endhint %}

## Create bucket

### Dependencies

package.json

```json
{
  "@cere-ddc-sdk/smart-contract": "0.5.0"
}
```

### Code

```javascript
const {
    connect,
    accountFromUri,
    sendTx,
    registerContract,
    getContract,
    CERE,
    ddcBucket,
} = require("@cere-ddc-sdk/smart-contract");
const log = console.log;

// Network settings
const RPC = "wss://rpc.testnet.cere.network:9945";
// User settings
const SEED = "0x3be19e0bba3af20bad16298976ec27e25d9330cd997634abb09cb101a0387e8b";
// DDC Settings
const CLUSTER_ID = 0;

const txOptionsPay = {
    value: 10n * CERE, // value can be 0, but substrate blockchain network v2.0.0 has bug, so with low value transaction can be failed
    gasLimit: -1, // Unlimited gas
};

const createBucket = async () => {
    //Connect to network and contract
    let contract;
    let account;
    {
        const {api, chainName, getExplorerUrl} = await connect(RPC);
        log("Connected to blockchain:", chainName);

        account = accountFromUri(SEED);
        log("From account", account.address);

        const contractName = "ddc_bucket";
        contract = getContract(contractName, chainName, api);
        log("Using contract", contractName, "at", contract.address.toString());
    }

    // Create Bucket
    let bucketId;
    {
        log("Create a bucket...");
        const tx = contract.tx.bucketCreate(txOptionsPay, '{"replication": 3}', CLUSTER_ID);
        const result = await sendTx(account, tx);
        bucketId = ddcBucket.findCreatedBucketId(result.contractEvents || []);
        log("New BucketId", bucketId, "\n");
    }
}
```

Output:

```
Connected to blockchain: Cere Testnet
From account 5DA7sweXtrq4hHLanbdzMowsuaTsm8kSKJnNT44t9VLLamJd
Using contract ddc_bucket at 5DAx9cTNXYKbbMTQUWzh1cZ46Mj14pnyKvshkVWm8fkfh36X
Create a bucket…
New BucketId 5
```

## Upload and Download

### Dependencies

package.json

```json
{
  "@cere-ddc-sdk/core": "1.1.0",
  "@cere-ddc-sdk/proto": "1.1.0",
  "@cere-ddc-sdk/file-storage": "1.1.0",
  "@cere-ddc-sdk/content-addressable-storage": "1.1.0"
}
```

### Code

```javascript
import {Scheme} from "@cere-ddc-sdk/core";
import {createReadStream} from 'node:fs';
import {FileStorage, FileStorageConfig} from "@cere-ddc-sdk/file-storage";

const BUCKET_ID = 5n;
const CDN_NODE_URL = "https://node-0.cdn.testnet.cere.network"
const SEED = "0x3be19e0bba3af20bad16298976ec27e25d9330cd997634abb09cb101a0387e8b"

const uploadAndDownload = async () => {
    const scheme = await Scheme.createScheme("sr25519", SEED);
    const testSubject = new FileStorage(scheme, CDN_NODE_URL, new FileStorageConfig(2, 3))

    //Upload file bytes
    const readable = createReadStream("test.txt");
    const uri = await testSubject.upload(BUCKET_ID, readable);
    console.log(uri);

    //Read file bytes
    const readers = testSubject.read(BUCKET_ID, uri.cid).getReader()
    let results
    while (!(results = await readers.read()).done) {
        console.log(new TextDecoder().decode(results.value))
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
{% hint style="info" %}
More examples and details in [Kotlin SDK README](https://github.com/Cerebellum-Network/cere-ddc-sdk-kotlin)
{% endhint %}

## Create bucket

### Dependencies

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

### Code

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

## Upload and Download

### Dependencies

build.gradle.kts

```kts
dependencies {
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:core:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:proto:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:content-addressable-storage:1.0.4.Final")
    api("com.github.Cerebellum-Network.cere-ddc-sdk-kotlin:file-storage:1.0.4.Final")
}
```

### Code

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
