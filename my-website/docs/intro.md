---
sidebar_position: 1
---

# General

The Ethereum bridge transfers (value, data) between assets on the Ethereum network (ETH/ERC20) and Secret tokens, specified by the SNIP-20 spec. The bridge is bi-directional, so those SNIP-20 assets can then be redeemed for their Ethereum equivalent.


Bridges between networks require long-term commitment and support, and each chain comes with its own set of challenges that may need to be addressed, even if it supports the EVM. 

The bridge itself consists of a number of different components, each requiring a slightly different skill-set to maintain, and anyone operating or creating a new bridge based on this code must be aware of the technologies used:

1.	Python
2.	MongoDB
3.	NodeJS/Typescript
4.	React/Typescript

Each component in the system can be replaced by a custom implementation – we leave this up to the operator to decide for themselves which parts to pick and choose


## Architecture

The core components of the bridge are:
1.	Bridge oracles
2.	Bridge contracts
3.	Bridge Database
4.	Backend services
5.	UI
