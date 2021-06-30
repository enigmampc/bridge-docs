---
sidebar_position: 1
---

# Multisig

The bridge is based on multisig on the EVM and the Secret Network side.

It is important to know how multisig functions to be able to understand how the bridge works.

## Secret Network

On Secret Network (and all Cosmos chains) there is native support for multisig. 
This means that each signer holds a key that is not specifically used for signing entire transactions, merely for appending a signature onto an existing transaction. 

This type of multisig is more secure, although it requires an external party to coordinate generating the raw (unsigned) transaction, gathering the signatures from all signers & handling broadcasting the transaction. 

You can read more on how this works [here](link to assaf's thing)

## EVM

In general, On EVM chains multisig is smart-contract based. I.e. we use a smart contract to perform the logic of managing multiple signer addresses. 

Each of the signers is a stand-alone address on the network, containing his own balance of tokens used to broadcast his approvals for transactions. 

This method requires a relatively complex contract to act as the multisig wallet which on the one hand is error prone (which could cause a loss or theft of funds), but on the other allows signers to communicate independently of each other.