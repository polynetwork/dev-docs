<h1 align="center">Import Token</h1>

To import tokens, there are four steps to follow:


### Step1. Asset deployment
- Deploy mapping contracts of the corresponding asset on the target chain using the [template](https://github.com/polynetwork/eth-contracts/blob/master/contracts/core/assets/erc20_template/ERC20Template.sol).
- Ensure the total amount of contracts stay consistent with the amount on the source chain.

### Step2. Assets transfer
- Transfer the initialized assets to the target chain [proxy contract](../../Core_Smart_Contract/Contract/LockProxy.md).

### Step3. Information submission
- [Submit](https://docs.google.com/forms/d/e/1FAIpQLScEdSVFT_OogZj9Yq3YG4IKKKrvdcjB3hMrG5udgGIzxvFZBw/viewform) your token information to the Poly Network team.
- Contact Poly Network via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a>

### Step4. Test
- Wait until the operators successfully add your contract address to the whitelist.
- Click [here](https://bridge.poly.network/testnet) and follow the [User Manual](../../Core_Smart_Contract/User_Manuals/Token_Transaction.md) to test the token cross-chain transfer.