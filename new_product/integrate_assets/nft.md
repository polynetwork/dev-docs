<h1 align="center">Import NFT</h1>

The source NFT contract must support `ERC721`, `ERC721Metadata` and `ERC721Enumerable` interfaces with the [ERC-721 standard](https://eips.ethereum.org/EIPS/eip-721).

Here are the three steps to follow to import an NFT. 
### Step1. Development and deployment
- Develop a contract following the NFT [template](https://github.com/polynetwork/nft-contracts/tree/main/contracts/erc721_template) provided by Poly Network.
- This contract implements a minting interface using the Poly Network standard:
  `function mintWithURI(address to, uint256 tokenId, string memory uri) external`
- This method will be called in the [NFTLockProxy](../../Core_Smart_Contract/Contract/NFTLockProxy.md) contract of Poly Network.

> [!Note|style:flat|label:Notice]
>If you want to deploy the NFT contract on the target chain by yourself, please implement the interface below and ensure that the method name, parameters, and order are strictly consistent.

- If your contract does not implement `_safeMint`, `_setTokenURI`, etc., you should implement them first. If you have already implemented these methods, you can directly copy the following code:
  
  ```solidity 
  function mintWithURI(address to, uint256 tokenId, string memory uri) external {
     require(!_exists(tokenId), "token id already exist");
    _safeMint(to, tokenId);
    _setTokenURI(tokenId, uri);
  }
  ```
  
### Step2. Information submission
- [Submit](https://docs.google.com/forms/d/e/1FAIpQLSfH1VcCAmdtjcQeCFjSXvYw7QS9MtsegzktyNmSvIPZzleSYg/viewform) your NFT information to the Poly Network team.
- Contact Poly Network via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a>

### Step3. Test 
- Wait until the operators successfully add your contract address to the whitelist.
- Click [here](https://bridge.poly.network/nft) and follow the [User Manual](../../Core_Smart_Contract/User_Manuals/NFT_Transaction.md) to test the NFT cross-chain transfer.
