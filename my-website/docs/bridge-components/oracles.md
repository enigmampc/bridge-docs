---
sidebar_position: 1
---

# Oracles
[enigmampc/EthereumBridge (github.com)](https://github.com/enigmampc/EthereumBridge)

The bridge oracles use a leader->signer architecture. The leader is responsible for watching the chain for new events. Once a new event is found, a transaction is proposed.

The signers then take that proposed transaction, validate that the proposed tx was indeed triggered by an on-chain event and then sign (or broadcast an approval for) the transaction.

The tradeoffs of such an architecture are that the leader must always remain online, however there are no race conditions that can lead to duplication of funds.
On the ETH side, once the number of signers passes the threshold it is executed automatically, while on the SCRT side we need an extra step done by the leader - broadcasting the signed transaction. The difference is due to how multisig is implemented on the different networks.

The leader syncs with the signers via a shared Database. The database is used to 

[add image]

