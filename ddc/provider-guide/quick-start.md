# ⏱ Quick Start

## System Requirements

DDC CDN and storage node can run on most Linux, macOS, and Windows systems.

## Register in Smart Contract

Each DDC node should be registered in Smart Contract. Otherwise, the node won’t be able to communicate to other nodes in cluster. Need to create cluster in Smart Contract with required nodes.

## Download docker image

Find the latest version on GitHub:

* [CDN node](https://github.com/Cerebellum-Network/ddc-gateway-node)
* [Storage node](https://github.com/Cerebellum-Network/ddc-storage-node)

{% tabs %}
{% tab title="CDN Node" %}
```shell
docker pull cerebellumnetwork/ddc-cdn-node:latest
```
{% endtab %}

{% tab title="Storage Node" %}
```shell
docker pull cerebellumnetwork/ddc-storage-node:latest
```
{% endtab %}
{% endtabs %}

## Configure

Use next environment variables to configure your node:

<table><thead><tr><th>Name</th><th>Description</th><th data-type="number" data-hidden>asd</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td>HTTP_PORT</td><td>Port of HTTP server</td><td>null</td><td></td><td></td></tr><tr><td>HTTP_ADDRESS</td><td>DNS that client can use to access HTTP API of the node (http://ddc.storage.miner.io)</td><td>null</td><td></td><td></td></tr><tr><td>LOG_LEVEL</td><td>Logging level (debug, info, warn etc.)</td><td>null</td><td></td><td></td></tr><tr><td>LOG_JSON_FORMAT</td><td>Logging in JSON format (true/false)</td><td>null</td><td></td><td></td></tr><tr><td>DEBUG_ENABLED</td><td>Enable debug endpoints for golang app (true/false)</td><td>null</td><td></td><td></td></tr><tr><td>DDC_BUCKET_CONTRACT_API_URL</td><td>Url to blockchain (<code>wss://rpc.testnet.cere.network:9945</code>)</td><td>null</td><td></td><td></td></tr><tr><td>DDC_BUCKET_CONTRACT_ACCOUNT_ID</td><td>Smart contract address (<code>5GqwX528CHg1jAGuRsiwDwBVXruUvnPeLkEcki4YFbigfKsC</code>)</td><td>null</td><td></td><td></td></tr><tr><td>STORAGE</td><td>Node size, only for DDC Storage node</td><td>null</td><td></td><td></td></tr><tr><td>TEST_ENABLED</td><td>Run node in test mode with mocked smart contract</td><td>null</td><td></td><td></td></tr></tbody></table>
