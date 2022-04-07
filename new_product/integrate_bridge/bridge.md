<h1 align="center">API</h1>

The following APIs are provided for developers who need to implement the bridge function. 
If you want to integrate into the Poly bridge, please get in touch with `Poly team` via <a class="fab fa-discord" href= "https://discord.com/invite/y6MuEnq"></a> for more support.

## Poly Bridge APIs:

- Get cross-chain fee.
- Get cross-chain transaction status.
- Get transaction history.
- Get basic token information.
- Get token mapping relationship between different chains.
- Get the expected cross-chain duration.

### Get cross chain fee

This API returns a transaction fee charged on the source chain in a cross-chain transaction.
And if `SwapTokenHash` is specified, the transferable amount will be returned.

API
```
Testnet: https://bridge.poly.network/testnet/v1/getfee
Mainnet: https://bridge.poly.network/v1/getfee
```

Parameter
```
/* @SrcChainId:    source chain id
 * @SwapTokenHash: the hash of the cross-chain token on the source chain
 * @Hash: The token hash used to charge fees
 * @DstChainId: target chain id
*/
```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1/getfee' \
--header 'Content-Type: application/json' \
--data-raw '{
    "SrcChainId": 2,
    "SwapTokenHash": "0d9c8723b343a8368bebe0b5e89273ff8d712e3c",
    "Hash": "0000000000000000000000000000000000000000",
    "DstChainId": 3
}'
```
Example Response
```json
{
  "SrcChainId": 2,
  "Hash": "0000000000000000000000000000000000000000",
  "DstChainId": 3,
  "UsdtAmount": "0.1874142096",
  "TokenAmount": "0.00006131",
  "TokenAmountWithPrecision": "6.1318011405463236515e+13",
  "SwapTokenHash": "0d9c8723b343a8368bebe0b5e89273ff8d712e3c",
  "Balance": "2.9553450781056623353e+41",
  "BalanceWithPrecision": "2.9553450781056623352e+47"
}
```

Here are examples of other dApps using this API:

[Chainswap](https://exchange.chainswap.com/#/bridge) ：Get transaction fee for crossing asset O3 from Ethereum chain to Binance chain
```bash
curl 'https://bridge.poly.network/v1/getfee' \
  --data-raw '{"SrcChainId":2,"SwapTokenHash":"Ee9801669C6138E84bD50dEB500827b776777d28","Hash":"0000000000000000000000000000000000000000","DstChainId":6}'
```
Response
```json
{
  "SrcChainId": 2,
  "Hash": "0000000000000000000000000000000000000000",
  "DstChainId": 6,
  "UsdtAmount": "0.7102667879046",
  "TokenAmount": "0.00021774",
  "TokenAmountWithPrecision": "2.17745709200889804991497e+14",
  "SwapTokenHash": "Ee9801669C6138E84bD50dEB500827b776777d28",
  "Balance": "905253.1100058306801",
  "BalanceWithPrecision": "9.052531100058306801e+23"
}
```

[O3Swap](https://o3swap.com/hub) : Get transaction fee for crossing USDT from Polygon chain to Arbitrum chain
```bash
curl 'https://bridge.poly.network/v1/getfee' \
  --data-raw '{"SrcChainId":17,"Hash":"0000000000000000000000000000000000000000","DstChainId":19}'
```
Response
```json
{
  "SrcChainId": 17,
  "Hash": "0000000000000000000000000000000000000000",
  "DstChainId": 19,
  "UsdtAmount": "6.24023692731448418039399",
  "TokenAmount": "4.15070971",
  "TokenAmountWithPrecision": "4.15070971868175230536829e+18",
  "SwapTokenHash": "",
  "Balance": "0",
  "BalanceWithPrecision": "0"
}
```

### Get the cross chain transaction status
This API returns the details of the assigned hash, and you can view the cross-chain procedure through the `TransactionState` in the response.  

API
```
Testnet: https://bridge.poly.network/testnet/v1//transactionofhash
Mainnet: https://bridge.poly.network/v1/transactionofhash
```

Parameter
```
/*
 * @Hash: the cross chain transaction hash
*/


```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1//transactionofhash' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Hash": "0b2c42b35e811b973c1d9258214cc68e63059271256bfb89239198cd1a7bda98"
}'
```
Example Response
```json
{
    "Hash": "4496cdb2905c148c06039a83d4130f04ddae498aa1065b06430ccd2766b08f78",
    "User": "547f1e7f552ec9701a8a4b4c30c6bbf3fff513cf",
    "SrcChainId": 7,
    "BlockHeight": 12538043,
    "Time": 1644508161,
    "DstChainId": 6,
    "FeeAmount": "2.59436134",
    "TransferAmount": "21926.387813549774936158",
    "DstUser": "bc7128144f31da5a1eff7c79e86d6c0b01af3de6",
    "ServerId": 0,
    "State": 0,
    "Token": {
        "Hash": "25d2e80cb6b86881fd7e07dd263fb79f4abe033c",
        "ChainId": 7,
        "Name": "MDX",
        "Property": 1,
        "TokenBasicName": "MDX",
        "Precision": 18,
        "AvailableAmount": "21578714598525328008311694",
        "TokenBasic": {
            "Name": "MDX",
            "Precision": 18,
            "Price": "0.28673118",
            "Ind": 1,
            "Time": 1644508861,
            "Property": 1,
            "Meta": "https://app.ont.io/poly/mdx.svg"
        }
    },
    "FeeToken": {
        "Hash": "25d2e80cb6b86881fd7e07dd263fb79f4abe033c",
        "ChainId": 7,
        "Name": "MDX",
        "Property": 1,
        "TokenBasicName": "MDX",
        "Precision": 18,
        "AvailableAmount": "21578714598525328008311694"
    },
    "TransactionState": [
        {
            "Hash": "4496cdb2905c148c06039a83d4130f04ddae498aa1065b06430ccd2766b08f78",
            "ChainId": 7,
            "Blocks": 21,
            "NeedBlocks": 21,
            "Time": 1644508161
        },
        {
            "Hash": "e93416eac8f52b9e1d7523ebecceb901b6cddb08d6c0e851903e52094f92cb77",
            "ChainId": 0,
            "Blocks": 1,
            "NeedBlocks": 1,
            "Time": 1644508259
        },
        {
            "Hash": "8d9cfefed98e22015f6baab9ab5abb9b33e8ca2b70003a774f12a90a8bb65273",
            "ChainId": 6,
            "Blocks": 1,
            "NeedBlocks": 1,
            "Time": 1644508270
        }
    ]
}
```
### Get transaction history

This API returns the cross-chain history of the assigned address.

API
```
Testnet: https://bridge.poly.network/testnet/v1/transactionsofaddress
Mainnet: https://bridge.poly.network/v1/transactionsofaddress
```

Parameter
```
/* @Addresses: wallet address
 * @PageNo: start page
 * @PageSize: size of displays per page
*/
```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1/transactionsofaddress' \
--header 'Content-Type: application/json' \
--data-raw '{
    "Addresses": [
        "93aea6537acd6c10ed76ea8eb367c2ae4cfa2282"
    ],
    "PageNo": 0,
    "PageSize": 10
}'
```
Example Response
```json
{
  "PageSize": 1,
  "PageNo": 0,
  "TotalPage": 1,
  "TotalCount": 1,
  "Transactions": [
    {
      "Hash": "0b2c42b35e811b973c1d9258214cc68e63059271256bfb89239198cd1a7bda98",
      "User": "aecb8dff86f198ad3a812a00035438e553b230bd",
      "SrcChainId": 88,
      "BlockHeight": 1163947,
      "Time": 1644997356,
      "DstChainId": 7,
      "FeeAmount": "0.00023409",
      "TransferAmount": "1.23",
      "DstUser": "93aea6537acd6c10ed76ea8eb367c2ae4cfa2282",
      "ServerId": 0,
      "State": 0,
      "Token": {
        "Hash": "d2a4cff31913016155e38e474a2c06d08be276cf",
        "ChainId": 88,
        "Name": "GAS",
        "Property": 1,
        "TokenBasicName": "GAS",
        "Precision": 8,
        "AvailableAmount": "39455668193977",
        "TokenBasic": {
          "Name": "GAS",
          "Precision": 8,
          "Price": "5.50686187",
          "Ind": 1,
          "Time": 1645095305,
          "Property": 1,
          "Meta": ""
        }
      },
      "FeeToken": {
        "Hash": "d2a4cff31913016155e38e474a2c06d08be276cf",
        "ChainId": 88,
        "Name": "GAS",
        "Property": 1,
        "TokenBasicName": "GAS",
        "Precision": 8,
        "AvailableAmount": "39455668193977"
      },
      "TransactionState": [
        {
          "Hash": "0b2c42b35e811b973c1d9258214cc68e63059271256bfb89239198cd1a7bda98",
          "ChainId": 88,
          "Blocks": 1,
          "NeedBlocks": 1,
          "Time": 1644997356
        },
        {
          "Hash": "331287ddc15029faa160dcf3846a57471d21b7b817fabbcd87e3fe4b80d89130",
          "ChainId": 0,
          "Blocks": 1,
          "NeedBlocks": 1,
          "Time": 1644997377
        },
        {
          "Hash": "42e45d1bdddc9bd3b63979676fd99f928fa741ea28a806cf385c001ac36ba101",
          "ChainId": 7,
          "Blocks": 1,
          "NeedBlocks": 1,
          "Time": 1644997384
        }
      ]
    }
  ]
}
```
### Get token basic information

This API returns the basic information of the token, including price, precision, meta data and the token information mapped on each chain.

API
```
Testnet: https://bridge.poly.network/testnet/v1/tokenbasics
Mainnet: https://bridge.poly.network/v1/tokenbasics
```

Parameter
```
none
```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1/tokenbasics'
```
Example Response
```json
{
  "TotalCount": 2,
  "TokenBasics": [
    {
      "Name": "8PAY",
      "Precision": 18,
      "Price": "1",
      "Ind": 1,
      "Time": 0,
      "Property": 1,
      "Meta": "https://app.ont.io/poly/8pay.jpg",
      "Tokens": [
        {
          "Hash": "48abb7954b5bc46be41cc444e731f9d03aa33feb",
          "ChainId": 79,
          "Name": "8pay",
          "Property": 1,
          "TokenBasicName": "8PAY",
          "Precision": 18,
          "AvailableAmount": "28254867780000000000"
        },
        {
          "Hash": "aa00fbcface6ff4913cc8322b12f63e2ad45d448",
          "ChainId": 2,
          "Name": "8pay",
          "Property": 1,
          "TokenBasicName": "8PAY",
          "Precision": 18,
          "AvailableAmount": "71745132220000000000"
        }
      ]
    }
  ]
}
```

### Get token mapping relationship between different chains

This API returns mapping relations between source chain and target chain of assigned token.

API
```
Testnet: https://bridge.poly.network/testnet/v1/tokenmap
Mainnet：https://bridge.poly.network/v1/tokenmap
```

Parameter
```
/* @ChainId: source chain id
 * @Hash: source token hash
*/
```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1/tokenmap' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ChainId": 79,
    "Hash": "48abb7954b5bc46be41cc444e731f9d03aa33feb"
}'
```
Example Response
```json
{
  "TotalCount": 1,
  "TokenMaps": [
    {
      "SrcTokenHash": "48abb7954b5bc46be41cc444e731f9d03aa33feb",
      "SrcToken": {
        "Hash": "48abb7954b5bc46be41cc444e731f9d03aa33feb",
        "ChainId": 79,
        "Name": "8pay",
        "Property": 1,
        "TokenBasicName": "8PAY",
        "Precision": 18,
        "AvailableAmount": "28254867780000000000"
      },
      "DstTokenHash": "aa00fbcface6ff4913cc8322b12f63e2ad45d448",
      "DstToken": {
        "Hash": "aa00fbcface6ff4913cc8322b12f63e2ad45d448",
        "ChainId": 2,
        "Name": "8pay",
        "Property": 1,
        "TokenBasicName": "8PAY",
        "Precision": 18,
        "AvailableAmount": "71745132220000000000"
      },
      "Property": 1
    }
  ]
}
```


### Get the expected cross-chain duration

This API returns the expected elapsed time for token to transfer from source chain to target chain.


API
```
Testnet: https://bridge.poly.network/testnet/v1/expecttime
Mainnet：https://bridge.poly.network/v1/expecttime
```

Parameter
```
/* @SrcChainId: source chain id
 * @DstChainId: target token hash
*/
```
Example Request
```bash
curl --location --request POST 'https://bridge.poly.network/testnet/v1/expecttime' \
--header 'Content-Type: application/json' \
--data-raw '{
    "SrcChainId": 79,
    "DstChainId": 2
}'
```
Example Response
```json
{
  "TotalCount": 1,
  "TokenMaps": [
    {
      "SrcTokenHash": "48abb7954b5bc46be41cc444e731f9d03aa33feb",
      "SrcToken": {
        "Hash": "48abb7954b5bc46be41cc444e731f9d03aa33feb",
        "ChainId": 79,
        "Name": "8pay",
        "Property": 1,
        "TokenBasicName": "8PAY",
        "Precision": 18,
        "AvailableAmount": "28254867780000000000"
      },
      "DstTokenHash": "aa00fbcface6ff4913cc8322b12f63e2ad45d448",
      "DstToken": {
        "Hash": "aa00fbcface6ff4913cc8322b12f63e2ad45d448",
        "ChainId": 2,
        "Name": "8pay",
        "Property": 1,
        "TokenBasicName": "8PAY",
        "Precision": 18,
        "AvailableAmount": "71745132220000000000"
      },
      "Property": 1
    }
  ]
}
```