# On-chain interaction

`Cube` is compatible with `Ethereum`'s ecosystem, support all `Ethereum`'s `RPC` API and SDK. 

Besides, there are a few Cube specific `RPC` APIs, showing bellow.

## Ethereum's RPC

[RPC Method List](https://eth.wiki/json-rpc/api)

Example:

```
 curl -s -H 'content-type:application/json' -d '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}' http://localhost:8545
```

## Cube Specific RPC

### eth_getBlockPredictStatus

Returns the status of the block obtained according to the [CasperFFG](https://www.adiasg.me/eth2/2020/04/09/casper-ffg-in-eth2-0.html "Casper FFG in Eth2.0") consensus rule

#### Parameters

------------


1. `DATA` - 32 Bytes - Hash of a block.
2. `QUANTITY` - integer of a block number,

```
params: [
   "0x72d17147a11acd2912846f8c5d10d842be45d67563b2fb6660e0272cbe3a664b",
   '0x1b4', // 436
]
```

------------


#### Returns

|  result  | status  |  mark |
| ------------ | ------------ | ------------ |
| 0  | Unknown   |  The status of the current block is unknown |
| 1  | Justified | The current block has reached a preliminary consensus, but there is a certain chance to discard it  |
| 2  | Finalized | The current block has reached a final consensus and will not be forked and discarded  |

------------


#### Example

------------


```shell
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockPredictStatus","params":["0x72d17147a11acd2912846f8c5d10d842be45d67563b2fb6660e0272cbe3a664b", "0x1b4"],"id":1}' localhost:8545
```
```shell
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": 2
}
```

### eth_getLastFinalizedBlockInfo
Query the block info of the latest finalized status

#### Parameters

------------
none

#### Example

------------
```shell
curl -X POST -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"eth_getLastFinalizedBlockInfo\",\"params\":[],\"id\":1}" localhost:8545
```
```shell
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "Number": "0x91b",
    "Hash": "0x80afd1753ed7a5ddc02188e6fe9e8e3fe5c5832f9415527081214ebe240e2e98"
  }
}
```

### eth_getDoubleSignPunishTransactionsByBlockNumber
Returns the penalty transaction of violating multiple signatures in the block queried according to the block number
#### Parameters

------------

1. `QUANTITY` - integer of a block number

```
params: [
   '0x1b4'
]
```

------------


#### Returns
------------
Returns all penalty transactions submitted for violating the multi sign rule in the block.
The submitted penalty transaction body contains two supporting information. The address of to is a special address, which is used to filter transactions

#### Example

------------
```shell
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getDoubleSignPunishTransactionsByBlockNumber","params":["0x192"],"id":1}' localhost:8545
```
```shell
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "blockHash": "0xd6c76c19f9461e1df42b24ad38b7b960aeaecde2339ddd637e1b965d916ce561",
      "blockNumber": "0x192",
      "from": "0x0942737e33b1ad9b028bb4fab46677b1e5371d79",
      "gas": "0x40641b5",
      "gasPrice": "0x0",
      "hash": "0x793e9a7314b8ccdd0fdef1f83153f9904689bd9b46f29dea5c94c6f0b1a97785",
      "input": "0xf9016202f88de4a0b0f01aa0a340f7f8275239c2f1f25bf81f559a75717990725e9eec5bbf11ee0182018ae4a0f2a4a531ac4c906d0cfd221dce758126717d96d01aa11cd4877c30dc779a829782018ba0dd418ea9c5d3c19cb5d233a6180f544227620cbb2d995af4bd74d2957b7032e7a01342371e87a1daf3f34cbf4faf9511820216cf313ca8b50c27f40da30b30d93b80f88de4a03f546da5c906fe37519c0e056f1407d7f72c622c75ebd9f1e26e8a76765348fe820186e4a0f06463b18dd621d4b8c33364acd57d185c1ec67f3ae28db84133d6d7dfcad93d82018ea0dfa9dc35248be47d77840573105e22a44199d60ef898c289e7bbdcf65f963b40a06565df9f0cd4e6009a2e813ada92cd538068a5867a5fa1d81ca0b5dad97fcb868082019094000000000000000000000000000000000000f006940942737e33b1ad9b028bb4fab46677b1e5371d7994540c913ad8b197152eb041bc56f9c6ab314d25ba80",
      "nonce": "0x0",
      "to": "0x00000000000000000000000000000000000fffff",
      "transactionIndex": "0x0",
      "value": "0x0",
      "type": "0x0",
      "v": "0x4a91",
      "r": "0x1f4c0c163ddd523a881bf66f8beba180f23494db42d0249f6a35e90ab72f564f",
      "s": "0x2c683319d430e47f436e0db1588bf592dc7656ab32e8727455e3aa64113e6102"
    }
  ]
}
```
### eth_getDoubleSignPunishTransactionsByBlockHash
Returns the penalty transaction of violating multiple signatures in the block queried by the block hash
#### Parameters

------------

1. `DATA` - 32 Bytes - Hash of a block.

```
params: [
   "0x72d17147a11acd2912846f8c5d10d842be45d67563b2fb6660e0272cbe3a664b"
]
```


#### Returns
------------

Returns all penalty transactions submitted for violating the multi sign rule in the block

#### Example

------------
```shell
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getDoubleSignPunishTransactionsByBlockHash","params":["0x793e9a7314b8ccdd0fdef1f83153f9904689bd9b46f29dea5c94c6f0b1a97785"],"id":1}' localhost:8545
```
```shell
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "blockHash": "0xd6c76c19f9461e1df42b24ad38b7b960aeaecde2339ddd637e1b965d916ce561",
      "blockNumber": "0x192",
      "from": "0x0942737e33b1ad9b028bb4fab46677b1e5371d79",
      "gas": "0x40641b5",
      "gasPrice": "0x0",
      "hash": "0x793e9a7314b8ccdd0fdef1f83153f9904689bd9b46f29dea5c94c6f0b1a97785",
      "input": "0xf9016202f88de4a0b0f01aa0a340f7f8275239c2f1f25bf81f559a75717990725e9eec5bbf11ee0182018ae4a0f2a4a531ac4c906d0cfd221dce758126717d96d01aa11cd4877c30dc779a829782018ba0dd418ea9c5d3c19cb5d233a6180f544227620cbb2d995af4bd74d2957b7032e7a01342371e87a1daf3f34cbf4faf9511820216cf313ca8b50c27f40da30b30d93b80f88de4a03f546da5c906fe37519c0e056f1407d7f72c622c75ebd9f1e26e8a76765348fe820186e4a0f06463b18dd621d4b8c33364acd57d185c1ec67f3ae28db84133d6d7dfcad93d82018ea0dfa9dc35248be47d77840573105e22a44199d60ef898c289e7bbdcf65f963b40a06565df9f0cd4e6009a2e813ada92cd538068a5867a5fa1d81ca0b5dad97fcb868082019094000000000000000000000000000000000000f006940942737e33b1ad9b028bb4fab46677b1e5371d7994540c913ad8b197152eb041bc56f9c6ab314d25ba80",
      "nonce": "0x0",
      "to": "0x00000000000000000000000000000000000fffff",
      "transactionIndex": "0x0",
      "value": "0x0",
      "type": "0x0",
      "v": "0x4a91",
      "r": "0x1f4c0c163ddd523a881bf66f8beba180f23494db42d0249f6a35e90ab72f564f",
      "s": "0x2c683319d430e47f436e0db1588bf592dc7656ab32e8727455e3aa64113e6102"
    }
  ]
}
```
### miner_startAttestation
Start submitting block attestation
By default, it is started at the same time when `miner_start` is executed
#### Parameters

------------
none

#### Example

------------
```shell
curl -X POST -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"miner_startAttestation\",\"params\":[],\"id\":1}" localhost:8545
```

### miner_stopAttestation
Stop submitting block attestation
By default, it is stopped at the same time when `miner_stop` is executed

#### Parameters

------------
none

#### Example

------------
```shell
curl -X POST -H "Content-Type: application/json" --data "{\"jsonrpc\":\"2.0\",\"method\":\"miner_stopAttestation\",\"params\":[],\"id\":1}" localhost:8545
```

## JSON RPC Error Codes

See [json-rpc-error-codes-improvement-proposal](https://eth.wiki/json-rpc/json-rpc-error-codes-improvement-proposal) for details.

## SDK

Use `Ethereum` SDK library such as `web3j`,`web3js`, `ethers.js`, etc for development. 

### web3js simple example

#### 1. Get chain info

```JavaScript
const Web3 = require('web3')

async function getChainId() {
    const web3 = new Web3('https://http-mainnet.cube.network')
    let chainId = await web3.eth.getChainId()
    console.log(`chain id: ${chainId}`)
    return chainId
}
```

#### 2. Generate account

```JavaScript
const Web3Accounts = require('web3-eth-accounts')

let account = new Web3Accounts().create()
//do not do this on prd env
console.log(`account generated. address: ${account.address}, private key: ${account.privateKey}`)
```

#### 3. Build transaction

```JavaScript
const Web3 = require('web3')

async function transfer(fromAccount, to, value){
    const web3 = new Web3('https://http-mainnet.cube.network')
    let chainId = await web3.eth.getChainId()
    let nonce = await web3.eth.getTransactionCount(fromAccount.address)
    let gasPrice = await web3.eth.getGasPrice()

    let unsigned = {
        from: fromAccount.address,
        to,
        value: web3.utils.numberToHex(web3.utils.toWei(value, 'ether')),
        gasPrice,
        nonce,
        chainId,
    }

    unsigned.gas = await web3.eth.estimateGas(unsigned)

    let signed = await fromAccount.signTransaction(unsigned)
    return signed
}
```