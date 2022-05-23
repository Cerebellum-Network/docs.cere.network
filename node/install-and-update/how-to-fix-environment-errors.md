# How to fix environment errors

1. ✘ Please change `chain-data` folder permission to 777.
   * Add permission to the `chain-data` folder by following the instructions in the `Start a Node` section [point #2](https://cere-network.gitbook.io/cere-network/node/install-and-update/start-a-node#start-a-node).
2. ✘ Please install NTP Time Server or reconfigure your NTP Time Server.
   * Install NTP Time Server by following the [instructions](https://cere-network.gitbook.io/cere-network/node/install-and-update/install-and-configure-network-time-protocol-ntp-client).
3. ✘ Please install `lsof` or open your node port `9944`.
   *   Install `lsof` by following the commands:

       ```
       sudo apt-get update -y
       sudo apt-get install -y lsof
       ```
   * Check docker container opened ports by following the command:`docker ps`
   * Recreate the docker container by following the instructions in the `Start a Node` section [point #6](https://cere-network.gitbook.io/cere-network/node/install-and-update/start-a-node#start-a-node).
