# About EIP1559

The Cube Chain do not fully support EIP1559. 

Specificly, in Cube, the `BaseFee` will always be `zero`, and a miner (Block Proposer) can set its required minimal gas price.

User can still using the `eth_gasPrice` or `eth_maxPriorityFeePerGas` to get a suggested gasPrice.

When sending a `DynamicFeeTxType` transaction (e.g. when using metamask), user should set both `maxFeePerGas` and `maxPriorityFeePerGas` to a same value.
