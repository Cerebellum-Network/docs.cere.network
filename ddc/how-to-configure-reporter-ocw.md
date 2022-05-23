---
description: >-
  This section is supposed to be used to describe the process on how to
  configure reporter (OCW)
---

# How to configure DDC Inspectors

### Prerequisites

* You have installed [subkey](https://github.com/paritytech/substrate/tree/master/bin/utils/subkey) util
* You have a DDC Smart Contract address. In our case: `5F7hDxFwMobL3mofGVUL4QwwZTmygwPE7T1ikPy5YT8Nizf8`
* You are [connected to a Node via Cere Block Viewer](https://cere-network.gitbook.io/cere-network/tools/block-viewer/how-to-connect-to-your-node-with-block-viewer) directly.

### Steps

By default reporter functionality already included in the Validator Node. This section described how to enable and configure the reporter.&#x20;

#### 1. Prepare reporter account

Create a new account using `subkey` util using the command: `subkey generate --scheme sr25519`:

```
~$ subkey generate --scheme sr25519
Secret phrase `ivory alpha language juice pyramid enforce kitchen cake galaxy ankle error aspect` is account:
  Secret seed:      0xfdd8ddf960c0ebf8b6970728780cd8210b0494b50ceb8823025efff519e1f6dd
  Public key (hex): 0xae4ef92049ef164f44b08da3f14e120a79cf6425ca5d8cdca655a4c7d6cf0978
  Account ID:       0xae4ef92049ef164f44b08da3f14e120a79cf6425ca5d8cdca655a4c7d6cf0978
  SS58 Address:     5G1Feba7v4dPBU4rVZxmooKQiHoY9JYibz8JNPMmgFzyPsGK

```

Send some amount of native tokens to this account (in our case -  `5G1Feba7v4dPBU4rVZxmooKQiHoY9JYibz8JNPMmgFzyPsGK`) to be able to pay for transactions.

#### 2. Add a reporter to the Smart Contract

* Open [Block Viewer](https://block-viewer.cere.network/#/explorer)
* Open `Developer` -> `Contracts`
* Select `DDC SC` -> `addInspector`
* Insert `Public key (hex)` to the `inspector` field
* Press `Execute`

#### 3. Configure interval

* Update the `ddc-metrics-offchain-worker::block_interval` in Developer -> RPC -> offchain - localStorageSet. The block number value should be in the Fixed Width Little Endian format. Examples: 5 - 0x05000000, 600 - 0x58020000, 10 - 0x0A000000.

#### 4. Configure the Smart Contract address

* Open [Block Viewer](https://block-viewer.cere.network/) which points to your Validator Node.&#x20;
* Go to `Developer` -> `RPC calls`
* Select `offchain` -> `localStorageSet` and specify the Smart Contract address mentioned in prerequisites section:
  * key: `ddc-metrics-offchain-worker::sc_address`
  * value: `5F7hDxFwMobL3mofGVUL4QwwZTmygwPE7T1ikPy5YT8Nizf8`
* Click `Submit RPC call`

![](<../.gitbook/assets/Screenshot from 2021-06-03 13-25-21.png>)

You are done with the Smart Contract address configuration!

#### 5. Configure your reporter account

* Use the same section (`Developer` -> `RPC calls`)
* Select `author` -> `insertKey`
* Specify params:
  * keyType: `ddc1`
  * suri: \<Secret phrase from the first step> (in our case - `ivory alpha language juice pyramid enforce kitchen cake galaxy ankle error aspect`)
  * publicKey: \<Public key from the first step> (in our case - `0xae4ef92049ef164f44b08da3f14e120a79cf6425ca5d8cdca655a4c7d6cf0978`)
* Click `Submit RPC call`

![](<../.gitbook/assets/Screenshot from 2021-06-03 13-29-03.png>)

You are done with the reporter configuration!

Congrats, you have just configured OCW for your Validator Node! Your reporter will be triggered after the next block generated!
