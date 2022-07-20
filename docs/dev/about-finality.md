# About finality

The Cube use a BFT attestation scheme for fast finality. There will be three status for a block:

|  result  | status  |  mark |
| ------------ | ------------ | ------------ |
| 0  | Unknown   |  The status of the current block is unknown |
| 1  | Justified | The current block has reached a preliminary consensus, but there is a certain chance to discard it  |
| 2  | Finalized | The current block has reached a final consensus and will not be forked and discarded  |


Besides, the Cube use a `longest chain` (actually, largest total difficulty) rule to chose a branch that without Finalized blocks.

So, about the finality, there will be three conditions:

1. Check the block status by using the RPC API [eth_getblockpredictstatus](/dev/sdk?id=eth_getblockpredictstatus), if the block is Finalized, then it reach the finality. **`Normally, a block can be finalized as fast as there are 3 block comfirmations`**.

Or you can use [eth_getlastfinalizedblockinfo](/dev/sdk?id=eth_getlastfinalizedblockinfo) to get the latest finalized block.

2. If a block and any of its descendant block is not Finalized, then it's **`probabilistic safe for finality in 25 comfirmations`**.
3. If a block and any of its descendant block is not Finalized, then it's **`safe for finality in 81 comfirmations`**
