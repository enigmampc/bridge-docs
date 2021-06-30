---
sidebar_position: 2
---

# Contracts

There are two sets of contracts – one on Secret Network, and one on the EVM chain.

## Secret Network

On the SCRT side each pair of assets (e.g. ETH<->secretETH) is managed by 2 secret contracts. The first is the SNIP-20 contract itself, which manages the token. This is the contract that a user will interact with to manage his token. That way, from the user's perspective, there is no difference between a bridged asset, and any other SNIP-20 asset (secret-secret, for instance).

The second is the swap contract. This contract is what the bridge multisig address interacts with.

You can find the source code of these contracts here

## EVM

TODO
