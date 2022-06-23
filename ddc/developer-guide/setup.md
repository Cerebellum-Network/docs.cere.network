# 🔑 Setup

## Install polkadot{.js} extension

You can download extension [here](https://polkadot.js.org/extension).

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
./ddc-cli generate-keys  -s sr25519
```

Output example:

```shell
Secret phrase: grass smooth rain offer matter senior crucial slim clip news town opera
Seed hex:  0x3be19e0bba3af20bad16298976ec27e25d9330cd997634abb09cb101a0387e8b
```

{% hint style="warning" %}
Please write down your secret phrase and keep it in a safe place. The secret phrase can be used to restore your keys (wallet). Keep it carefully to not lose your assets.
{% endhint %}

Or you can extract seed from existing secret phrase (mnemonic code):

```shell
./ddc-cli extract-seed --secret-phrase 'grass smooth rain offer matter senior crucial slim clip news town opera'
```

## Import account to polkadot{.js} extension

You can import generated account to the extension (if you used `generate-keys` command):

1. Open extension in the browser
2. Click the add icon at the top right
3. Select `Import account from pre-existing seed`
4. Put your secret phrase
5. Click `Next` and follow instructions

## Faucet

To create a bucket, add nodes, create cluster your account should have enough CERE tokens.

Use [faucet](https://stats.cere.network/faucet):

1. choose network `testnet`;
2. enter public key (`5DAx9cTNXYKbbMTQUWzh1cZ46Mj14pnyKvshkVWm8fkfh36X`);
3. click `Send me test CERE tokens`
4. wait 1-3 minutes while tokens will be transferred (can check balance [CERE explorer](https://explorer.cere.network) for `Cere Testnet` network)
