# Genesis file
Both the mainnet and testnet genesis information of `Cube` chain have been hardcoded in blockchain, and the corresponding genesis files are listed below for verification.

## Glossary 

Genesis file defines all the information needed to create a genesis block. It is organized in json format. It contains the following 4 sections:
- chain config
- block header related config
- accounts and system contacts config
- validator info

Beside, the system contracts are under the directory `contracts/builtin/` in the `Cube` repo.

### Chain config

The name of chain config item is `config`,including `chainId`, hard fork info, consensus engine config. Demo config is showed as following:

```json
{
    "config": {
        "chainId": 1819,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "eip155Block": 0,
        "eip158Block": 0,
        "byzantiumBlock": 0,
        "constantinopleBlock": 0,
        "petersburgBlock": 0,
        "istanbulBlock": 0,
        "berlinBlock": 0,
        "londonBlock": 0,
        "chaos": {
            "period": 3,
            "epoch": 200,
            "attestationDelay": 2,
            "rule": 1
        }
    }
}
```
Specifically, `chainId` is blockchain identifier which was defined in EIP-155 to avoid replay attack. Mainnet is 1818, and testnet 1819. Besides, users can customize it when setting up a private chain.

`homesteadBlock` ~ `muirGlacierBlock` are the hard fork info inherited from ethereum, which should be all zero

`chaos` is the consensus engine config of Cube. In detail, `period` is the block interval (whose unit is second) as well as `epoch` is the number of blocks which can be produced in an epoch. `attestationDelay` is the number of blocks, before which we can do consensus. `rule` indicates the rewards plan. Usually `rule` is only set in Testnet and should be omitted otherwise.

One thing should be mentioned is that the chain config will not be included in the genesis block although it will be persisted on disk. Therefore it could be changed afterwards.

### Block header related config

The info of block header is in the top level of the config, Demo config is showed as following:

```json
{
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "number": "0x0",
    "timestamp": "0x624e601f",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "difficulty": "0x1",
    "gasLimit": "0x2625a00",
    "gasUsed": "0x0",
    "nonce": "0x0",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "extraData": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
}
```

As showing above, all the items are basic info in block header. 

Among all these items, `nonce` and `mixHash` are something used in PoW, which is deprecated in Cube.

the meaning of other config items are listed as follows: 
- `parentHash`: hash of parent block
- `number`: block numner
- `timestamp`: timestamp of block prodution
- `coinbase`: the address of block producer
- `difficulty`: difficulty in consensus
- `gasLimit`: max gas can be used
- `gasUsed`: gas actually used
- `extraData`: extra data in header

Specifically, `extraData` contains 3 parts: vanity(32 bytes), validator address list, signature (65 bytes). Validator address list will be added into extra data automatically by Cube, so we only need to set vanity and signature, which should be all zero in common.

### Accounts and system contacts config

All of the configs are located in `alloc`. It contains the token allocation of external owned accounts as well as system contracts initializations, including Staking, CommunityPool, BonusPool and GenesisLock

Each system contract has been assigned fixed addresses preliminarily
- Staking Contract:       0x000000000000000000000000000000000000F000
- CommunityPool Contract: 0x000000000000000000000000000000000000F001
- BonusPool Contract:     0x000000000000000000000000000000000000F002
- GenesisLock Contract:   0x000000000000000000000000000000000000F003

#### External owned accounts allocation

Demo config is showed as following:

```json
{
    "alloc": {
        "352BbF453fFdcba6b126a73eD684260D7968dDc8": {
        "balance": "50000000000000000000"
        }
    }
}
```

`352BbF453fFdcba6b126a73eD684260D7968dDc8` is account address, `banlance` is the amount of token allocation in Wei

#### Staking contract config

Staking contract is used to manage validators as well as staked assets. Demo config is showed as following:

```json
{
    "alloc": {
        "000000000000000000000000000000000000F000": {
            "balance": "0x0",
            "init": {
                "admin": "0xd0db65Fc3fa41001C70E8E6F31F837E3010c6F68",
                "firstLockPeriod": "63072000",
                "releasePeriod": "2592000",
                "releaseCnt": "48",
                "ruEpoch": "28800"
            },
            "code": "0x608060405260043610620003e35760003560e01c80637bd8d00c1162..."
        }
    }
}
```

`000000000000000000000000000000000000F000` is the address that Cube assignes to Staking contract. This address is fixed and thus cannot be customized.

`init` is the arguments for contract initalization, which can be interpreted as following: 
- `admin`: contract admin
- `firstLockPeriod`: first lock period in seconds
- `releasePeriod`: the time of one release period in seconds
- `releaseCnt`: the total count of release period
- `ruEpoch`: the number of blocks to update block rewards

`code` is the deployed bytecode of the contract

#### CommunityPool contract config

CommunityPool controls the gas rewards for excellent DAPPs. Demo config is showed as following:

```json
{
    "alloc": {
        "000000000000000000000000000000000000F001": {
            "balance": "0x0",
            "init": {
                "admin": "0xd0db65Fc3fa41001C70E8E6F31F837E3010c6F68"
            },
            "code": "0x6080604052600436106100745760003560e01c80638f283970116100..."
        }
    }
}
```

`000000000000000000000000000000000000F001` is the address that Cube assignes to CommunityPool contract. This address is fixed and thus cannot be customized.

`init.admin` is the address admin

`balance` is 0

`code` is the deployed bytecode of the contract

#### BonusPool contract config

BonusPool is used to manage bonus for staking. Demo config is showed as following:

```json
{
    "alloc": {
        "000000000000000000000000000000000000F002": {
            "balance": "592000000000000000000",
            "code": "0x6080604052600436106100555760003560e01c8063158ef93e146100..."
        }
    }
}
```

`000000000000000000000000000000000000F002` is the address that Cube assignes to BonusPool contract. This address is fixed and thus cannot be customized.

`balance` is equal to the total bonus of staking

`code` is the deployed bytecode of the contract

#### GenesisLock contract config

GenesisLock is used to manage token locking and release. Demo config is showed as following:

```json
{
    "alloc": {
        "000000000000000000000000000000000000F003": {
            "balance": "0x0",
            "init": {
                "periodTime": "2592000",
                "lockedAccounts": [
                    {
                        "userAddress": "0x2FA024cA813449D315d71D49BdDF7c175C036729",
                        "typeId": "1",
                        "lockedAmount": "1000000000000000000000",
                        "lockedTime": "0",
                        "periodAmount": "48"
                    },
                    {
                        "userAddress": "0x3F2327847cF1a9C74a835fe1A2DCbbE2FdAa9626",
                        "typeId": "2",
                        "lockedAmount": "2000000000000000000000",
                        "lockedTime": "31104000",
                        "periodAmount": "24"
                    }
                ]
            },
            "code":"0x608060405234801561001057600080fd5b50600436106100cf576000..."
        }
    }
}
```

`000000000000000000000000000000000000F003` is the address that Cube assignes to GenesisLock contract. This address is fixed and thus cannot be customized.

`periodTime` is the release period in seconds

`init.lockedAccounts` is the accounts info of token locking
- `userAddress`: address of accounts
- `typeId`: accounts type, including 1-community, 2-private sale, 3-team, 4-Ecosystem foundation
- `lockedAmount`: token locked amount in wei
- `lockedTime`: token locked time in seconds
- `periodAmount`: the total amount of periods, which is 30 day in one period

`code` is the deployed bytecode of the contract

### validators info

The config is located in item of `validators`, Demo config is showed as following:

```json
{
    "validators": [
        {
            "address": "0x8Cc5A1a0802DB41DB826C2FcB72423744338DcB0",
            "manager": "0xd0db65Fc3fa41001C70E8E6F31F837E3010c6F68",
            "rate": "20",
            "stake": "350",
            "acceptDelegation": true
        },
        {
            "address": "0xd0db65Fc3fa41001C70E8E6F31F837E3010c6F68",
            "manager": "0xd0db65Fc3fa41001C70E8E6F31F837E3010c6F68",
            "rate": "10",
            "stake": "550",
            "acceptDelegation": true
        }
    ]
}
```

the info of validator contains:
- `address`: the address of validator
- `manager`: the address of manager of validator
- `rate`: the percentage of commission for all block rewards
- `stake`: the amount of staking native token, the unit of this feild is `CUBE`
- `acceptDelegation`: whether accepting delegation or not

One thing need to be explained more specifically is `rate`. The validator can take rate% of block rewards as commission, and the left part is the one that will be allocated according to their token staked.

## Verify genesis

### Check tool

Compile the tool of checking genesis file by running
```
make all
```

And the tool `genesis-check` will be found in `build/bin`

The source code of `genesis-check` is `cmd/genesis-check/main.go`

### Check method

Running the following command to generate genesis block hash from genesis config file
```
./genesis-check <genesis file>
```

The outputs looks like
```
Genesis Hash: <Genesis Hash>
Is Mainnet: false
Is Testnet: true
```

Then you may compare if the generated genesis hash is the expected one

### Built-in genesis

The genesis blocks of testnet and mainnet are hard-coding in binary. 

Mainnet genesis block(not confirmed) is defined in `core.DefaultGenesisBlock()`, which is located in `core/genesis.go:453`

Testnet genesis block is defined in `core.DefaultTestnetGenesisBlock()`, which is located in `core/genesis.go:468`

Also, related type definations of corresponding genesis file are `Genesis`, `GenesisAlloc`, `GenesisAccount`, `Init`, `LockedAccount` and `ValidatorInfo`, which are all located in `core/genesis.go`

## mainnet

**Genesis block hash**: 0x878c0e84bf867ddd5ede830d3b17f853eeb985530e1a74468ce898094427d41f

> `MainnetGenesisHash` in the code: <https://github.com/cube-network/Cube/blob/master/params/config.go#L30>

[View or download mainnet genesis file](/static/mainnet_genesis.json ':ignore')

## testnet
**Genesis block hash**: 0x2eeaa9dab31682321c7d8afd3ec0ddef772f9361e7f21126e3d17f2756e49482

[View or download testnet genesis file](/static/testnet_genesis.json ':ignore')
