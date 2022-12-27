# Start a Node

## **Pre Requirements**

1. [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
2. [Install Docker](https://docs.docker.com/get-docker/)
3. [Install Docker Compose 1.25.0+](https://docs.docker.com/compose/install/)
4. [Install & Configure Network Time Protocol (NTP) Client - Linux only](/node/install-and-update/install-and-configure-network-time-protocol-ntp-client.md)

## **Start a Node**

To run a Node on your host, please follow the steps below:

1.  Clone the [repository](https://github.com/Cerebellum-Network/nodes-installation-scripts) and go to its folder:

    ```
     git clone https://github.com/Cerebellum-Network/nodes-installation-scripts.git
     cd nodes-installation-scripts
    ```
2.  Add permissions to the `chain-data` folder:

    ```
     chmod -R 777 ./chain-data
    ```
3. Open the following ports in your firewall so that inbound traffic from the Internet can reach your Node:
   1. Validator Node:
      1. `30333` - Specifies the port that your node will listen for p2p traffic on
   2. Full / Archive Node:
      1. `9933` - Specifies the port that your node will listen for incoming RPC traffic on
      2. `9944` - Specifies the port that your node will listen for incoming WebSocket traffic on
      3. `30333` - Specifies the port that your node will listen for p2p traffic on
4. Select the config file depending on the Network you are going to connect to.
   1. For Testnet `CONFIG_FILE=./configs/.env.testnet`.
   2. For Mainnet `CONFIG_FILE=./configs/.env.mainnet`.
5. Specify `NODE_NAME` parameter in the config file. The value for the `NODE_NAME` will be used to distinguish nodes in the [Telemetry dashboard](https://telemetry.polkadot.io/#list/0x81443836a9a24caaa23f1241897d1235717535711d1d3fe24eae4fdc942c092c).
6. Run the command to start a Node. From this moment your Node will run in the background.
   1.  Validator Node:

       ```
        docker-compose --env-file ${CONFIG_FILE} up -d add_validation_node_custom
       ```
   2.  Full Node:

       ```
        docker-compose --env-file ${CONFIG_FILE} up -d full_node
       ```
   3.  Archive Node:

       ```
        docker-compose --env-file ${CONFIG_FILE} up -d archive_node
       ```
7.  Run the command to confirm the Node has been started successfully.

    ```
     docker-compose logs -f | grep "Local node identity is"
    ```

    &#x20;You should have at least one line in the output. Below is an example:

    ```
     Mar 03 14:42:23.973  INFO Local node identity is: 12D3KooWLafVQ2XGQS8A3rdj9t8rX5UFcVGxtx8a9vgQsYJi7Rdz (legacy representation: 12D3KooWLafVQ2XGQS8A3rdj9t8rX5UFcVGxtx8a9vgQsYJi7Rdz)
    ```

    If you cannot see the output in more than 2 minutes you should check node logs. Use the [instructions](/node/install-and-update/node-logs.md) to know more about logs and their set up.
8. Launch [Cere Explorer ](https://explorer.cere.network/)and [connect to your node](/tools/cere-explorer/how-to-connect-to-your-node-with-cere-explorer.md). You can check the Node's `syncing` status. To do this go to `Network` -> `Explorer` -> `Node info`. It should be equal to `yes` or `no` (synced already).

Congratulations! You've successfully started a Node on your host.

Please, follow the "Become a Validator or Full / Archive Node" instructions to finish setting up.
