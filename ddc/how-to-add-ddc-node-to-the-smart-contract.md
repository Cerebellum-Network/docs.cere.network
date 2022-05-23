---
description: >-
  This section is supposed to be used to describe the process on how to add a
  new node to the DDC Smart Contract
---

# How to add DDC Node to the Smart Contract

### Prerequisites

* You should be a Smart Contract Admin.
* You should have enough CERE tokens to pay fees.

### Steps

1. Open [Block Viewer](https://block-viewer.cere.network/?rpc=wss%3A%2F%2Frpc.testnet.dev.cere.network%3A9945#/staking): `Developer` -> `Contracts`
2. Find method: `addDdcNode`
3. Click on `exec`
4. Specify parameters:
   1. `p2p_id` found in the logs of the DDN.
   2. `p2p_addr` similar to p2p\_id but also includes an IP address.
   3. `url` where the HTTP API of the node is reachable.
   4. `permissions` 1 for trusted nodes (e.g., run by Cere), 0 for untrusted nodes.

![](<../.gitbook/assets/Screenshot from 2021-06-03 12-39-01.png>)

5\. Click `Execute` then sign the tx and you should be able to see the newly added node in the `getAllDdcNodes` function:

![](<../.gitbook/assets/Screenshot from 2021-06-03 12-40-32.png>)

Congrats! You have just added a new DDC Node to the contract.
