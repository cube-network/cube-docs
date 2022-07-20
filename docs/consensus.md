# Consensus

The consensus used in `Cube` is called `Chaos Consensus`, is a hybrid DPoS protocol with random selection, and use BFT attesstation for fast finality. Moreover, the `Chaos` consensus introduce a pipelined procedure through the whole process of consensus. For more detail, see the [technical white paper](https://github.com/cube-network/techical-whitepaper).

In here, let us just explain the other aspect of the consensus, **the staking and rewards mechanism**.

## Glossary 
- Validator. Responsible for packaging out blocks for on-chain transactions.
- Active validator. The current set of validators responsible for packing out blocks, with a maximum of 21.
- Epoch. Time interval in blocks, currently 1 epoch = 200 blocks on Cube. At the end of each epoch, the blockchain interacts with the system contracts to update active validators.

## System contract

The system contracts are under the `contracts/builtin/` directory of the `Cube` repo.

The management of the current validators are all done by the system contracts.
- `Staking`  It's the only entry of the staking and validators management. Responsible for managing access to validators and managing validator register and staking management, and punishing operations.
- `Validator` Responsible for recording inner staking and delegation information, inner settle staking rewards. Each validator has and only has one `Validator` contract, and the `Validator` contract is created by `Staking` contract when register a new validator, and all functions that invove staking are only accessed by `Staking` contract.

Blockchain call system contracts: 
- At the end of each block, the `Staking` contract is called and the fees for all transactions in the block are distributed to active validators.
- The `Staking` contract is called to punish the validator  when the validator is  not  working properly.
- At the end of each epoch, the `Staking` contract is called to update active validators, based on the ranking.

## Staking

1. Cube will choose the top n (currently n = 21) validators by their total staking to form an active validator set for each epoch (an epoch will last hundred of blocks, eg. 200), this set will be response to propose and validate the blocks of the epoch.
2. There will be a funder validator set that is set on the genesis block.
3. User can register to be a validator through the system `Staking` contract. Before the Cube is fully decentralized, only the admin of the system contract can register a validator, and after the Cube is permitionless, then any user can register to be a validator as long as it provides enough stakes.
4. The following fields are needed to register a validator:  
   a)	validator address: The address that represents the validator, and to propose blocks. Unchangeable.  
   b)	manager address: The manager address, responsible for all manage operations. Changeable.  
   c)	accept delegation: A bool value indicates wheter the validator accepts delegation or not. Unchangeable.  
   d)	commission rate: If a validator accepts delegation, then for all staking rewards it can get, it will first take commission acording to the commission rate, then the remaining staking rewards will distribute Stakes accordingly. Range on [0,100]. Unchangeable.  
5. Validator need a minimal self-staking: 50000 CUBE;
6. To avoid all users to staking (delegates) to a few validators, Cube limits a single validator's max total stakes to `24 million` CUBE.
7. The funder validators on the genesis block, have a lock-requiement. See WhitePaper for details.
8. All stakes, can receive staking rewards, besides, when the user unbinds its stakes, there will be extra bonus acording to the staking-time, See WhitePaper for details.
9. 20% gas fee on a block will be sent to a `communityPool` address, which will be used to rewards high quality developers, and 80% of the gas fee will be shared equally by all active validators( the validator that can propose blocks).
10. There's a 21days locking when unbinds the stakes.

## Punishment

There are penalties for malicious behavior or validator inactivity: 

1. Whenever a validator is found not to propose block as predefined, the `Staking` contract is automatically called at the end of this block and the validator is counted. When the counter reaches 48, the validator will be removed from the list of active validators,  0.1% of his total staking will be slashed and the validator will be disqualified.

2. Whenever a validator signs different blocks at a same height, the validator will be `double-sign-punished` through `doubleSignPunish` function of `Staking` contract. The validator will be removed from the list of active validators, 1% of his total staking will be slashed and the validator will be disqualified.

3. If a validator was punished, then all delegators of the validator will share the slashing.
