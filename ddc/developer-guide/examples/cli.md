---
description: These examples demonstrates several common DDC CLI use-cases.
---

# 💻 CLI

The example assume that you already installed DDC CLI by following [#install-ddc-cli](../setup.md#install-ddc-cli "mention") instructions.

## Create an account

This example demonstrates how to create a new account (wallet) on DDC Testnet

### Generate a new account

```bash
npx cere-ddc account --random
```

Output

```
New account
  Mnemonic: gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory
  Type: sr25519
  Address: 6PxrvjkVJFrQs6Tqdeov1GcJkS5rpzuM3NYGhX8KQErv49V2
```

### Create a configuration

Create a configuration file (`ddc.config.json`) with the following content

```json
{
  "signer": "gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory"
}
```

### Top-up the account

Top-up the account with CERE tokens either by using [the faucet](https://stats.cere.network/faucet) or by sending some amount of CERE tokens from another account.

```
6PxrvjkVJFrQs6Tqdeov1GcJkS5rpzuM3NYGhX8KQErv49V2
```

### Check the account balance

```bash
npx cere-ddc --config ./ddc.config.json account
```

Output

```
Account information
  Network: testnet
  Type: sr25519
  Address: 6PxrvjkVJFrQs6Tqdeov1GcJkS5rpzuM3NYGhX8KQErv49V2
  Public key: 0x140366f3907d204001192a29c9e38243d344bae9b83726e531e46839ac461214
  Balance: 50
  Deposit: 0
```

### Deposit tokens

Deposit 20 CERE tokens to the account

```bash
npx cere-ddc --config ./ddc.config.json deposit 20
```

Output

```
Deposit completed
  Network: testnet
  Amount: 20
  Total deposit: 20
```

## Create a bucket

Add `clusterId` to the configuration file (`ddc.config.json`)

```json
{
  "signer": "gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory",
  "clusterId": "0x825c4b2352850de9986d9d28568db6f0c023a1e3" // <--
}
```

### Private bucket

```bash
npx cere-ddc --config ./ddc.config.json create-bucket --access private
```

Output

```
Bucket created
  Network: testnet
  Cluster ID: 0x825c4b2352850de9986d9d28568db6f0c023a1e3
  Bucket ID: 27327n
```

### Public bucket

```bash
npx cere-ddc --config ./ddc.config.json create-bucket --access public
```

Output

```
Bucket created
  Network: testnet
  Cluster ID: 0x825c4b2352850de9986d9d28568db6f0c023a1e3
  Bucket ID: 27328n
```

## Upload/download files

Add `bucketId` to the configuration file (`ddc.config.json`)

```json
{
  "signer": "gospel fee escape timber toilet crouch artist catalog salt icon bulb ivory",
  "clusterId": "0x825c4b2352850de9986d9d28568db6f0c023a1e3",
  "bucketId": "27327"
}
```

### Upload a file

```bash
npx cere-ddc --config ./ddc.config.json upload ./2mb.jpg
```

Output

```
File upload completed
  Network: testnet
  Bucket ID: 27327n
  Path: 2mb.jpg
  CID: baebb4ide2ma4fvzr35kkgaqlnh5lgeojwsyhp4n4k7jc2qgsdizcariizm
```

### Download the file

```bash
npx cere-ddc --config ./ddc.config.json download baebb4ide2ma4fvzr35kkgaqlnh5lgeojwsyhp4n4k7jc2qgsdizcariizm ./2mb-downloaded.jpg
```

Output

```
File download completed
  Network: testnet
  Bucket ID: 27327
  CID: baebb4ide2ma4fvzr35kkgaqlnh5lgeojwsyhp4n4k7jc2qgsdizcariizm
  Destination: 2mb-downloaded.jpg
```

***

{% hint style="info" %}
Runnable versions of the examples are available in [the examples directory](https://github.com/Cerebellum-Network/cere-ddc-sdk-js/tree/main/examples/cli).
{% endhint %}
