<h1 align="center">Build New Bridges</h1>

Besides building a new chain, you may want to join the Poly cross-chain ecosystem by connecting with Poly Bridge or integrating the [APIs](bridge.md) provided by Poly Bridge into your own cross-chain bridge.

If this describes your intention, the following information will be of service to you. 

## 1. Main Functions of Poly Bridge

* Monitor cross-chain event  
During transactions, Poly Bridge monitors the wrapper events emitted by the wrapper contract on the source chain, the lock event (on the source chain) and the unlock event (on the target chain) emitted by CCM contracts.


* Link cross-chain events  
Poly Bridge links the events monitored from the source chain, poly chain and target chain depending on the transaction key, and updates the status of the cross-chain transactions simultaneously.


* Update cross-chain fees  
Poly Bridge will update the transaction fee of the source chain to facilitate the process of initiating a cross-chain transaction for users.


* Update cross-chain token price  
Poly Bridge also update the price of tokens both on the source chain and the target chain from the market, informing users of the exchange rate and accurately calculating the transaction fee.

> [!Note|style:flat|label:Notice]
> For now, Poly Bridge is deployed by Poly team.

## 2. Integrating Poly Bridge APIs
If your goal is to implement your own cross-chain bridge, perhaps you should realize the four main functions mentioned above. For your convenience, Poly Bridge  also provides some API for you to integrate into your own cross-chain bridge.
You can find them [here](bridge.md).
