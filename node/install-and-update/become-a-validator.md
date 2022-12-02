# Become a Validator

From this section you will know how to become a Validator.

1. Create a `Stash` account by following the [instructions](/tools/cere-explorer/how-to-create-an-account-by-using-cere-explorer.md).
2. Create a `Controller` account by following the [instructions](/tools/cere-explorer/how-to-create-an-account-by-using-cere-explorer.md).
3. **Testnets Only!** - **** Fund Stash and Controller accounts with native tokens by using [Cere FriendBot](https://laboratory.cere.network/#/friend-bot) (you can take accounts public keys in the [Account tab in the Cere Explorer ](https://explorer.cere.network/#/accounts)by clicking on the round icon left to the account name).
4. Wait until your local Node will be synced. Syncing should happen automatically after X minutes.
5. Add a Validator via [Cere Explorer](https://explorer.cere.network/) (check [here](/tools/cere-explorer/how-to-connect-to-your-node-with-cere-explorer.md) how to connect to your node).
   1. Generate `Session Key`.
      1. Go to `Developer` → `RPC calls.`
      2. Select `author` → `rotateKeys()` function.
      3. Click `Submit RPC call.`
      4.  Store your `session key` for the later steps (double click on the response below and copy to clipboard). `session key` example can be found below:

          ```
           0x11b76096f732a6d22bcfc0c1b5acfe871d8430e2d44a60765d6bb4f4db989ed354e2a0cc60489c4e5c850a2d83e778f9968e3cd3acba86f24f9ea522c95755101ec09626e5ec94c5a10ca83b4994fe580a6ab9f6f720f97858b905d84d794f072603b677467097c1209ebab19a14d285e6174004e4f1459c4f823bb5c8a05c0c
          ```
   2. Setup Validator
      1. Go to `Network` → `Staking`
      2. Select `Account Actions` tab and click `+ Validator`
      3. Bond your Tokens. Bond a maximum of 95% of your tokens so that you are still able to pay for transaction fees.
      4. Click `next.`
      5. Enter generated `Session Key` then you will have your account in the validators account list.
      6. Set `reward commission percentage` equal to `5`
      7. Press `Bond & Validate`
      8. Press `Sign and Submit`
6. **IGNORE if you completed #5, BETA!** - Add a Validator by using an automated script.
   1.  Replace the [./scripts/validator/.env](https://github.com/Cerebellum-Network/nodes-installation-scripts/blob/master/scripts/validator/.env) with appropriate values.

       | Parameter                     | Description                                                        |
       | ----------------------------- | ------------------------------------------------------------------ |
       | PROVIDER                      | Websocket link for the local node                                  |
       | STASH\_ACCOUNT\_MNEMONIC      | Stash account mnemonic                                             |
       | CONTROLLER\_ACCOUNT\_MNEMONIC | Controller account mnemonic                                        |
       | BOND\_VALUE                   | Staking value (Should be multiplied to 10^15)                      |
       | REWARD\_COMMISSION            | Percentage reward (0-100) that should be applied for the validator |
   2.  Run the script.

       ```
            docker-compose --env-file ./scripts/validator/.env up add_validator
       ```
   3. Once it is finished, you should be able to see `Validator added successfully!` message.
   4. Clean up the values in [./scripts/validator/.env](https://github.com/Cerebellum-Network/nodes-installation-scripts/blob/master/scripts/validator/.env).

Congratulations! Now your Validator is in a waiting state.
