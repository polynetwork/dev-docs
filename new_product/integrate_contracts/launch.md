<h1 align="center">Test and Launch</h1>


## 1. Test Your Contracts

There are two steps for you to test your contracts.

> [!Note|style:flat|label:Notice]
> The premise of contract testing is that both the original and target chains you want to test have been connected to the Poly Network. Otherwise, the test cannot be performed.

### Step 1. Preparation

- Deploy your **contract** in the source chain and target chain (For safety reasons, it is recommended to run the test on the TestNet. If you have to run the test on the MainNet, please contact the `Poly team` via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a>).
- Please [submit](https://docs.google.com/forms/d/e/1FAIpQLSeP_4apj9ZYnA8pJFw19YQgoejfUeX1riHWJngIbVldyv7NYA/viewform) the "**method**" (the input parameter of cross-chain function, i.e., the function of your contract called by target chain).
- The `Poly team` will add this "method" to the relayer whitelist to process the transactions automatically. Otherwise, the transaction cannot be processed on the Poly chain.

### Step 2. Test on Poly Network
After the preparation, you can try to call the cross-chain function in your contract, and then query the steps of the cross-chain transaction [here]( https://explorer.poly.network/testnet).
Contract issues can be analyzed according to the step the transaction reached:

- The transaction has been completed on the source chain but has not completed on Poly:
  - If the transaction has been stuck in this step for over 5 minutes, consider whether the input parameter "toChainId" (the input parameter of the cross-chain function, i.e., the target chain ID) is correct.
  - If the "toChainId" is correct, please contact `Poly team` via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a>.

> [!Note|style:flat|label:Notice]
> The "toChainId" refers to the target chain ID registered on Poly, not the network ID.
> For details, see [Chain ID](../../Core_Smart_Contract/Chain_ID/Chain_ID.md).
> Please note that the IDs on the TestNet and the MainNet are different.

- The transaction hasn't been completed on the target chain:
  - Call the API `getmanualtxdata` with the Poly chain hash (the second transaction hash).

  API
    ```
    Testnet: https://bridge.poly.network/testnet/v1/getmanualtxdata
    Mainnet: https://bridge.poly.network/v1/getmanualtxdata
    ```
  Parameter
  ```
  /* @Polyhash: the transaction hash in Poly
   */
  ```
  Example Request
    ```bash
  curl --location --request POST 'https://bridge.poly.network/testnet/v1/getmanualtxdata' \
  --header 'Content-Type: application/json' \
  --data-raw '{
      "polyhash": "",
  }'
  ```

  Example Response
  ```json
  {
    "data": "",
    "dst_ccm": "0x"
  }
  ```
  - Get the response "data" as the input data to send a transaction to "dst_ccm" on the target chain.


> [!Note|style:flat|label:Notice]
> This step is to call the `verifyHeaderAndExecuteTx` function of CCM on the target chain.
> The calling process is encapsulated in "data," so you only need to send a transaction with "data" to CCM on the target chain.


- If you encounter an error submitting this transaction, there is something wrong with your contract on the target chain. Consider doing the following:
  - Making sure the "method" (the function of your contract called by target chain) is allowed to be called by the CCM contract on the target chain.
  - Checking that the txData (one of the input parameters of the cross-chain function) can be parsed correctly by the "method" on the target chain.


- The transaction has been completed on the target chain:
  - Congratulations! You have completed the cross-chain transaction.

> [!Note|style:flat|label:Notice]
> During the test, you will have to send the target transactions manually. For projects, this may not be desirable. If you want to realize the process automatically, a relayer is required. Here is a [relayer solution](../../new_chain/relayer/relayer.md) for you.

## 2. Launch on MainNet
After all tests have been completed on the TestNet; you are ready to launch on the MainNet.
The steps are as follows:

- First, deploy your contract on the MainNet;
- Next, [submit](https://docs.google.com/forms/d/e/1FAIpQLSf7GKqlVkAUbhfs7qFj2wKW_p2qWCJJzBJlEGjNduGfi0wlTQ/viewform) the "method" and the source contract hash you have deployed;
- Then, launch your relayer (See [here](../../new_chain/launch_and_test/launch.md) for steps).

When all is done, you can start your cross-chain journey.