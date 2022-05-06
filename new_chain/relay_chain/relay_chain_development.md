<h1 align="center">Develop for Poly Chain</h1>

This step shows you how Poly Chain verifies cross-chain transactions from your chain. 
Verification by synchronizing block headers is the most common method.
The tutorial below will describe how to implement this method in detail.

> [!Note|style:flat|label:Notice]
> If your chain **doesn't** support the method of verification by synchronizing block headers (i.e., your chain fails to meet the prerequisites listed),
> here are also other solutions,
> and please contact `Poly team` via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a> for more support.

## 1. Prerequisites

There are three prerequisites to verify by synchronizing block headers:

- Your chain must support light client verification, like Simple Payment Verification (SPV) protocol in Bitcoin or Light Ethereum Subprotocol (LES) in Ethereum, which means the block header must contain the following information：

  - Hash of the previous block header.
  - Hash of the Merkle state root.
  - A way to prove the unique legitimacy of a block.
  
- Make sure the block header struct in your chain can be parsed and verified correctly.

- Make sure the Merkle state root in your chain can be parsed and verified correctly.


## 2. Development Specifications

There are four steps to be implemented for the verification method. 

### Step1. Adding a router for your chain

When importing your chain to Poly Chain, you should firstly add a router for your chain in the [file](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/utils/params.go#L43). 

### Step2 Implementing the interface of block header synchronization

- First, the corresponding handler should be added to the [file](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/header_sync/entrance.go#L62).
- Second, implement the method for the interface of HeaderSyncHandler, as shown follows:

```go
type HeaderSyncHandler interface {
	SyncGenesisHeader(service *native.NativeService) error
	SyncBlockHeader(service *native.NativeService) error
	SyncCrossChainMsg(service *native.NativeService) error
}
```

| Method                | Description                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **SyncGenesisHeader** | This method is used to store the initial block header so that the subsequent block headers of blocks containing cross-chain events can be verified and synchronized. This method will only be called for **once** in initializing the new chain. Please refer to the [code](https://github.com/polynetwork/poly/blob/master/native/service/header_sync/eth/header_sync.go#L61) for more details.  |
| **SyncBlockHeader**   | This method is used to synchronize block headers consistently from the new chain to Poly Chain. Please refer to the [code](https://github.com/polynetwork/poly/blob/master/native/service/header_sync/eth/header_sync.go#L99) for more details.                                                                                                                                                   |
| **SyncCrossChainMsg** | This method is used to fetch cross-chain messages. Please refer to the ontology [code](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/header_sync/ont/header_sync.go#L106) or eth [code](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/header_sync/eth/header_sync.go#L216) for more details. |


### Step3. Implementing the interface of cross-chain management

- First, the corresponding handler should be added to the [file](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/cross_chain_manager/entrance.go#L61).
- Second, implement the method for the interface of ChainHandler, as shown follows:

```go
type ChainHandler interface {
	MakeDepositProposal(service *native.NativeService) (*MakeTxParam, error)
}
```

| Method                  | Description                                                                                                                                                                                                                                                                       |
|-------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **MakeDepositProposal** | This method is used to verify the legitimacy of cross-chain transactions and store legitimate transactions to Poly Chain. Please refer to the [code](https://github.com/polynetwork/poly/blob/master/native/service/cross_chain_manager/eth/eth_handler.go#L34) for more details. |

The returned struct [**MakeTxParam**](https://github.com/polynetwork/poly/blob/1e629995982e61930e962e84606998f15233c534/native/service/cross_chain_manager/common/param.go#L148) is used for subsequent notification that the transaction has been verified. 

```go
type MakeTxParam struct {
	TxHash              []byte
	CrossChainID        []byte
	FromContractAddress []byte
	ToChainID           uint64
	ToContractAddress   []byte
	Method              string
	Args                []byte
}
```

In the example of the function **MakeDepositProposal**, two functions (**verifyFromTx** and **verifyMerkleProof**) play an important role in cross-chain verification.

| Method           | Description                                                  |
| ---------------- | :----------------------------------------------------------- |
| **verifyFromTx** | This method is used to get the synchronized block header from the database stored in Poly Chain, and then to call the function **verifyMerkleProof** to verify the legitimacy of cross-chain transactions, and decode the [Extra data](https://github.com/polynetwork/poly/blob/ea51f848c3b4284c046ca9bab527869846679640/native/service/cross_chain_manager/common/param.go#L68) to construct MakeTxParam. Please refer to the [code](https://github.com/polynetwork/poly/blob/4323af5cfcd2a3277653d5bdc4db015cd9755fee/native/service/cross_chain_manager/eth/utils.go#L41) for more details. |

```go
/*  
 *  @param native       Native Service that carries values of information of cross-chain events     
 *  @param proof        The proof submitted by the current cross-chain transaction      
 *  @param extra        The cross-chain message which is used to construct MakeTxParam 
 *  @param fromChainID  Source chain id
 *  @param height       The block height corresponding to the current transaction event
 *  @param sideChain    Source chain information that contains the ccm contract address
*/
func verifyFromEthTx(native *native.NativeService, proof, extra []byte, fromChainID uint64, height uint32, sideChain *cmanager.SideChain) (*scom.MakeTxParam, error) 
```

| Method                | Description                                                                                                                                                                                                                                                                                                            |
|-----------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **verifyMerkleProof** | This method is used to verify whether the Merkle proof submitted by the transaction is consistent with the block header stored in Poly. Please refer to the [code](https://github.com/polynetwork/poly/blob/4323af5cfcd2a3277653d5bdc4db015cd9755fee/native/service/cross_chain_manager/eth/utils.go#L88) for details. |

```go
/*  
 *  @param ethProof      The proof submitted by the current cross-chain transaction 
 *  @param blockData     The block header stored in poly chain corresponding to the current transaction event      
 *  @param contractAddr  The ccm contract address
*/
func VerifyMerkleProof(ethProof *ETHProof, blockData *eth.Header, contractAddr []byte) ([]byte, error) 
```

### Step4. Developing for register tool

The final step to import your chain is to register your chain in Poly Chain and synchronize the Poly chain genesis header to your chain. 
There are two methods required, as shown follows:

| Method                    | Description                                                  |
| ------------------------- | :----------------------------------------------------------- |
| **RegisterSideChain**     | This method is used to register your chain in Poly Chain. Please refer to the [code](https://github.com/polynetwork/poly-io-test/blob/035b7fadee297e6e1b5a0b3dcde80f22442d8fb1/cmd/tools/run.go#L1765) for more details. |
| **SyncPolyGenesisHeader** | This method is used to achieve two-way synchronization of genesis block headers between Poly Chain and your chain. Please refer to the [code](https://github.com/polynetwork/poly-io-test/blob/master/cmd/tools/run.go#L607) for more details. |

Now, the development for Poly Chain is completed.
