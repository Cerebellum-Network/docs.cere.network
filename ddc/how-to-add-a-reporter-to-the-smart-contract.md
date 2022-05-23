---
description: >-
  This section is supposed to be used to describe the process on how to add a
  reporter to the DDC Smart Contract
---

# How to add an Inspector to the Smart Contract

### Prerequisites

* You should be a Smart Contract Admin.
* You should have enough CERE tokens to pay fees.

### Steps

1. Open [Block Viewer](https://block-viewer.cere.network/?rpc=wss%3A%2F%2Frpc.testnet.dev.cere.network%3A9945#/staking): `Developer` -> `Contracts`
2. Find method: `addReporter`
3. Click on `exec`
4. Specify `reporter`

![](<../.gitbook/assets/Screenshot from 2021-06-03 12-48-24.png>)

5\. Click `Execute` then sign the tx and you should be able to see check if reported added using the  `isReporter` function. Select the reporter you just added:

![](<../.gitbook/assets/Screenshot from 2021-06-03 12-50-42.png>)

Congrats! You should be able to see `true` in the response, if reported added successfully.
