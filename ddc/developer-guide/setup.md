---
description: Learn how to create an account and deposit tokens
---

# 🔑 Setup

## Install DDC CLI

Download a binary file from [releases](https://github.com/Cerebellum-Network/ddc-cli/releases).

1. Unzip (expand) a compressed item: Double-click the downloaded .zip file
2. Make this file executable

```shell
 chmod +x ddc-cli
```

## Generate keys

If you don’t have a wallet yet, you can use DDC CLI to generate a secret phrase and seed hex:

```shell
./ddc-cli generate-keys -s sr25519
```

Output example:

```shell
Secret phrase: motion gospel order sketch blast rack deer oppose manage burden broccoli foster
Secret seed: 0x46423dd616093f9ea9b5df65994c18c1c8a27a806c68d885d6effd01721d1492
Public key: 0xf2ead4d7a4d8be53ca859bd2cd568c22ffd4bf3a54b4825fb823d7dabeff4e1d
SS58 Address: 5HZDCq4tLYrnjTMpGyMdPsrPxDwAFG8gDR49oW6jL7yTs5Ec
```

{% hint style="warning" %}
Please write down your secret phrase and keep it in a safe place. The secret phrase can be used to restore your keys (wallet). Put it in a safe place so you do not lose your data.
{% endhint %}

Or you can extract keys from the existing secret phrase (mnemonic code):

```shell
./ddc-cli extract-keys --secret-phrase 'hair solve immune apology aerobic broom milk split paper crime maple harvest' --scheme sr25519
```

Output example:

```
Secret phrase: motion gospel order sketch blast rack deer oppose manage burden broccoli foster
Secret seed: 0x46423dd616093f9ea9b5df65994c18c1c8a27a806c68d885d6effd01721d1492
Public key: 0xf2ead4d7a4d8be53ca859bd2cd568c22ffd4bf3a54b4825fb823d7dabeff4e1d
SS58 Address: 5HZDCq4tLYrnjTMpGyMdPsrPxDwAFG8gDR49oW6jL7yTs5Ec
```

## Faucet

Using Faucet API you can receive tokens in test networks (devnet, qanet and testnet).

{% hint style="info" %}
This command transfer 100 CERE tokens to your account.
{% endhint %}

```
./ddc-cli faucet -n testnet -a 5HZDCq4tLYrnjTMpGyMdPsrPxDwAFG8gDR49oW6jL7yTs5Ec
```

Output example:

```
Your transaction hash is 0xdde615ac64db4ab3e08cf4168434762f8febaffa088052e09d4994151145940f
```
