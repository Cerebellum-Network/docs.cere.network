# Uploading and downloading files to DDC

In this tutorial, you'll learn how to use a React frontend to interact with the smart contract on the Cere blockchain that coordinates the exchange of storage space for $CERE tokens between users and providers. 

Like other online storage services, users that wish to store and access their data in real-time must pay the entity that does so. In the case of Cere DDC, this is no different. 

In the case of centralized services, funds are used to manage large server farms that are connected to the internet and allow access to your content at all times. 

However, this process is dependent on human input and the perpetuity of your content is ultimately subject to the decisions of a centralized set of individuals.

If we wish to decentralize this process, we must remove all human input from the transaction. This is where the smart contract comes in. In short, the smart contract creates an financial incentive for multiple providers to store and make your data available, while ensuring that even if one provider chooses to stop hosting your content, it will remain available via other providers. To learn more about this smart contract, see section [insert section here]

- What is the mechanism by which a new provider is given a piece of data when one provider stops giving access to it?

- Idea: Because the data is encryptped and providers cannot interpret the data that they are hosting, it becomes impossible for providers to coordinate to take down a piece of data. 


Insert more theory here. 

Interaction with the smart contract requires the Cere software development kit (SDK). Written in both Javascript and Kotlin, this set of tools allow users to upload content to DDC in just a few lines of code, without needing to worry about the details of interacting with our smart contract.

Note: Unlike centralized solutions, the process of storing data with DDC requires coding by design. App developers will use our SDK to build their front-end, and it is up to them to abstract the code behind their own user interfaces. In other words, we provide the SDK for app developers to seamlessly offer a decentralized storage solution to the data stored on their apps. We do so by designing a publicly accessible system that ensures a economic incentive between users and providers at all times. 

# The process of uploading and downloading a file from DDC
The process of uploading and downloading a file from DDC involves 3 steps:
1. The creation of a bucket. Buckets are the basic containers to which you will uploading your data. Buckets are used to organize data and can be configured for location and access control. We'll touch more on access control in section [insert here](google.com).
2. Uploading of a file.
3. Downloading of a file.

- Include some info on upload/download chunks. Not clear from the code how this is achieved.


# Getting set up
## Downloading the Polkadot.js extension
Cere is a substrate-based blockchain. This is where the smart contracts that coordinate the economic incentive between users and providers are hosted. In order to interact with these smart contracts, you'll need a polkadot-based wallet. Before starting, you should have the polkadot.js extension installed in your browser. You can download this extension [here](https://polkadot.js.org/extension).

- Insert steps to create a wallet through using the browser extension. Why do we need this command-line tool to generate the wallet, as opposed to simply using the browser extension?

## Getting Testnet Cere in your wallet
You'll also need some Cere tokens to pay for the uploading and downloading of your content. For this tutorial, we'll use the Cere Testnet so that you do not need to pay for your tokens. But the process is equivalent when using the Cere Mainnet and you'd simply purchase them with a crypto exchange or a fiat onramp.

To get your Testnet tokens, go to https://stats.cere.network/faucet and 
1. Select `testnet` as your network of choice
2. Enter the public key for the wallet you just created. To find your public key, click on the Polkadot.js extension and copy the public key/hex beneath the name of your new wallet.
3. Click on "Send me test CERE tokens".

You should receive your tokens after a minute. You can see the current balance of your wallet using the Cere Testnet explorer at https://explorer.cere.network/ and selecting Cere Testnet at the drop down menu on the top left corner.

## Install Node.js and dependencies
Before starting, you will need Node.js installed on your machine. To install Node.js, follow the steps [here](google.com).

Next, you must install some libraries. In a terminal, execute the following command:
```
npm i @cere-ddc-sdk/smart-contract@0.5.0 @cere-ddc-sdk/core@1.1.0 @cere-ddc-sdk/proto@1.1.0 @cere-ddc-sdk/file-storage@1.1.0 @cere-ddc-sdk/content-addressable-storage@1.1.0
```

# Creating a bucket
The first step to uploading a file to DDC is the creation of a bucket. 
1. Copy the following code into a file. Let's name it `createBucket.js`. Replace the `SEED` varialbe with your private keys and save it.

```javascript
const {connect, accountFromUri, sendTx, registerContract, getContract, CERE, ddcBucket,} = require("@cere-ddc-sdk/smart-contract");
const log = console.log;

// Network settings
const RPC = "wss://rpc.testnet.cere.network:9945";
const CONTRACT_ADDRESS = "5DTZfAcmZctJodfa4W88BW5QXVBxT4v7UEax91HZCArTih6U";
// User settings
const SEED = "0x142d8b22d6aea5a16d2aadeb4db2c4e7fea3cb20f68cc345d928a84ee0d018a7";
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
        registerContract(contractName, chainName, CONTRACT_ADDRESS);
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
    return "DONE";
};


createBucket().then(console.log).catch(console.error);                                                    
```


2. Using a terminal, navigate to the folder containing this file and execute the following command:
```
node createBucket.js
```
Once the script is finished executing, your bucket is created. You can exit the by pressing `Control+C` on your keyboard. You should see an output similar to the following:
```
Connected to blockchain: Cere Testnet
From account 5DA7sweXtrq4hHLanbdzMowsuaTsm8kSKJnNT44t9VLLamJd
Using contract ddc_bucket at 5DAx9cTNXYKbbMTQUWzh1cZ46Mj14pnyKvshkVWm8fkfh36X
Create a bucket…
New BucketId 5
```
Keep track of this `BucketId`, we'll need it in the next step. 

# Uploading and downloading a file to DDC
Now that your bucket has been created, we can upload a file to it and subsequently download it.

1. Copy the following code into a file. Let's name it `uploadDownload.js`. Once again, replace the `SEED` variable with your private keys. You also need to specify the `BUCKET_ID` outputted by the previous script.

```javascript
const { Scheme } = require("@cere-ddc-sdk/core")
const { createReadStream } = require("node:fs")
const { FileStorage, FileStorageConfig } = require("@cere-ddc-sdk/file-storage")

const BUCKET_ID = 8;
const CDN_NODE_URL = "https://node-0.v2.eu.cdn.testnet.cere.network"

const SEED = "0x142d8b22d6aea5a16d2aadeb4db2c4e7fea3cb20f68cc345d928a84ee0d018a7";
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
uploadAndDownload().then(console.log).catch(console.error);
```

2. Now save the file and execute the following command:

```
node uploadDownload.js
```

Your file will be uploaded to the bucket we created above, and subsequently downloaded and printed to your command line.
