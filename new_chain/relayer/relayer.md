<h1 align="center">Develop for Relayer</h1>

## 1. Development Specifications

This step shows how to develop a relayer for your chain or project. The relayer plays a role in delivering messages between different chains, which is part of the cross-chain ecosystem.

There are two required components: **Chain Listener** and **Chain Submitter**.

### 1.1 Chain Listener
The chain listener fetches data from the source chain and Poly Chain, including the block header, cross-chain events emitted from CCM, and Merkle proofs used to verify the cross-chain messages in Poly Chain.
The listed interface is necessary.

```go
type IChainListener interface {
  // Initialize with config
  Init(*config.ListenerConfig, *poly.SDK) error
  // Blocks to confirm
  Defer() int
  // New block check interval
  ListenCheck() time.Duration
  // Chain Id
  ChainId() uint64
  // Optional: Fetch block header and header hash, used to submit to Poly chain for verifications
  Header(height uint64) (header []byte, hash []byte, err error)
  // Optional: Last header sync state in Poly chain.
  LastHeaderSync(uint64, uint64) (uint64, error)
  // Scan cross chain transactions included in the block
  Scan(uint64) ([]*msg.Tx, error)
  // Compose cross chain message before submit to Poly chain
  Compose(*msg.Tx) error
  // Current chain height
  LatestHeight() (uint64, error)
}
```

### 1.2 Chain Submitter
Chain Submitter delivers messages to the target chain, including **validator changes** of Poly Chain to CCD and **cross-chain messages**.
Additionally, Chain Submitter checks whether the transaction has been verified within the CCD.
The listed interface is necessary.

```go
type IChainSubmitter interface {
  // Initialize with config
  Init(*config.SubmitterConfig) error
  // Submit message/transaction to the chain
  Submit(msg.Message) error
  // Start the thread
  Start(context.Context, *sync.WaitGroup, bus.TxBus, bus.DelayedTxBus, msg.PolyComposer) error  
  // Process the cross chain message from Poly chain
  ProcessTx(*msg.Tx, msg.PolyComposer) error
}
```
##2. Develop Steps on Poly-Relayer.

Poly-Relayer is a relayer project maintained by Poly Network.
If you choose to develop based on Poly-Relayer, please follow the steps listed below.

### Step 1. Prerequisites
The [Poly-Relayer](https://github.com/polynetwork/poly-relayer) project is based on the [bridge-common](https://github.com/polynetwork/bridge-common) library. So you need to:
- Add chain ID in the `bridge-common` project [here](https://github.com/polynetwork/bridge-common/tree/main/base).
- Add chain client SDK [here](https://github.com/polynetwork/bridge-common/treemain/chains) for common usage.
- Add chain wallet [here](https://github.com/polynetwork/bridge-common/tree/main/wallet) for common usage.

### Step 2. Interface implementation
Implement interface `IChainListener` and `IChainSubmitter` for the new chain.

### Step 3. Registration
Register `ChainListener` and `ChainSubmitter` in [selectors](https://github.com/polynetwork/poly-relayer/blob/main/relayer/relayer.go#L73) located in the `relayer.go` file.

##3. Preparation for Launch

The configurations are a requirement for launching the relayer:

- Make sure the necessary configuration is specified in `config.json,` including CCM contract, CCD contract, and other details for the chain.
- You can see a [sample](https://github.com/polynetwork/poly-relayer/blob/main/config.sample.json) here.

> [!Note|style:flat|label:Notice]
>
> - If you develop the relayer for the chain integrated with Poly Network, you can directly fetch [CCD](../../Core_Smart_Contract/Contract/CCD.md) and [CCM](../../Core_Smart_Contract/Contract/CCM.md) contracts.
> - If you develop the relayer for a new chain, please complete the contract yourself.
> - Check Poly public [nodes](../../Core_Smart_Contract/Nodes/Nodes.md) in here.

- Specify roles to enable in `roles.json` and see a [sample](https://github.com/polynetwork/poly-relayer/blob/main/roles.sample.json) here.

| Roles       | Quantity Demand                | Description                                                                                    |
|-------------|--------------------------------|------------------------------------------------------------------------------------------------|
| HeaderSync  | One or multiple for each chain | Submitting chain headers to Poly Chain                                                         |
| TxListen    | Only one for each chain        | Observing cross-chain transactions from the source chain and pushing them to the message queue |
| TxCommit    | One or multiple for each chain | Consuming the message queue and submitting the cross-chain transactions to Poly Chain.         |
| PolyListen  | Only one for Poly Chain        | Observing cross-chain transactions from Poly Chain and pushing them to the message queue.      |
| PolyCommit  | One or multiple for Poly Chain | Consuming the message queue and submitting the cross-chain transaction to the target chain.    |


You are now ready for the relayer, please refer to the chapter [Test and Launch](../../new_chain/launch_and_test/launch.md) for launching details.
