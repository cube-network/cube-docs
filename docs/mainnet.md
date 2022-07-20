# Mainnet Info

## chainid
```
1818
```
## rpc

> There will be a rate limit. Currently is 50r/s per session/IP .

```
https://http-mainnet.cube.network
wss://ws-mainnet.cube.network

https://http-mainnet-sg.cube.network 
wss://ws-mainnet-sg.cube.network

https://http-mainnet-us.cube.network
wss://ws-mainnet-us.cube.network

```

## archive nodes

> There will be a rate limit. Currently is 50r/s per session/IP .
> 
> Currently available namespace: ["eth", "net", "web3"]

```
https://http-mainnet-archive.cube.network
wss://ws-mainnet-archive.cube.network
```

## explorer
```
https://www.cubescan.network
```

## tool contract address
- WCUBE Address：[0x9d3f61338d6eb394e378d28c1fd17d5909ac6591][wcube]
- Multicall Address:  [0x28d2ebdb36369db1c51355cdc0898754d1a1c3c5][multicall]
- Multicall2 Address:  [0x511b6bdf973bccda108059f082807bc5f2ef6b8b][multicall2]
  [wcube]:https://www.cubescan.network/en-us/address/0x9d3f61338d6eb394e378d28c1fd17d5909ac6591
  [multicall]:https://www.cubescan.network/en-us/address/0x28d2ebdb36369db1c51355cdc0898754d1a1c3c5
  [multicall2]:https://www.cubescan.network/en-us/address/0x511b6bdf973bccda108059f082807bc5f2ef6b8b

## ERC-20 CUBE

ERC-20 CUBE address on Ethereum: [0x6df241103F89983b1fB86244C8601E44ea55368C](https://etherscan.io/token/0x6df241103f89983b1fb86244c8601e44ea55368c)

## genesis

**Genesis block hash**: 0x878c0e84bf867ddd5ede830d3b17f853eeb985530e1a74468ce898094427d41f

> `MainnetGenesisHash` in the code: <https://github.com/cube-network/Cube/blob/master/params/config.go#L30>

[View or download mainnet genesis file](/static/mainnet_genesis.json ':ignore')

### How to check/verify the mainnet genesis file

There's an open source code that can be used to check the miannet genesis file, the code is <https://github.com/cube-network/Cube/blob/master/cmd/genesis-check/main.go> .

Do the check as follow:
```
cd cmd/genesis-check
go run main.go <your-downloaded-mainnet-genesisfile.json>
```

The result will be as follow if you are checking the mainnet genesis file:
```
Genesis Hash: 0x878c0e84bf867ddd5ede830d3b17f853eeb985530e1a74468ce898094427d41f
Is Mainnet: true
Is Testnet: false
```

## P2P Nodes

allow P2P port（default `33688`） udp/tcp

> the following nodes are default config for bootstrap node in code https://github.com/cube-network/Cube/blob/master/params/bootnodes.go

```
"enode://38d2c886ed86379a88c58bf024b7575d3b89e9239ba380bc0c49c4d14d24f147b429f89553106351600f35efa835680ff96fc4220c1d6f5e3fb8b109e36f2574@43.133.189.105:33688",
"enode://576751034af9c7fbc958af162318e5d962b962821b55e6533967a29dec83bb2c486baf8808e020ca7e1ffb3658b1669ccfe25513f73058fca93a9296cfa15b7c@43.133.23.39:33688",
"enode://2219a8c4cefc5c31ce4c744de46ca70f436344342327baad82d83fe3d64e679fdd631380e1c7ae1033d12ec9a8a1ae75ecae2e10313b95108ec1791955d5291f@43.128.80.123:33688",
"enode://4e158ecd8cdc6857461f8358768b37bdab94ba934fd3b0982eb1d85c0b3d354bdcd21ed0ca22a58ef56437dc74781f71884af166e0673b97aabad42c0b6e55b8@43.134.69.100:33688",
"enode://ae8f91e34062152d483f45999bee323bde7c230f3a4d79d3fc67cff83027ecaa5cfde5a538e3aacf7266b064825ee90ca9c8ddd6192cdc8d83b8363c7dc82777@50.18.45.58:33688",
"enode://193a049e1f76ef6202a5e1a24b33492fd2f343e3ae527c6582a5afe7e7158502b44ed524939d126b2cc37fa7354dfad7b594591f532755dd16f54155964ee3dc@50.18.102.76:33688",
```
