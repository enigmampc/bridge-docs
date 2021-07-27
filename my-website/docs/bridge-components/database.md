---
sidebar_position: 3
---

# Database



## Connecting


a.	Connect to Database - To connect to the database you can use a few options -
i.	Via the Atlas UI (works for everything, but viewing logs here is hard)
ii.	Via connection string - requires username & password (and I’m not leaking them here) using either Robo3T or Jetbrains DB panel


## Database collections

*	swap - 
Main collection used to record data about bridge swaps. It contains the data you will need to troubleshoot a swap. The main fields that matter are: status, amount, dst_address, src_address. Status 4 means that the swap was successful (and was confirmed on-chain), status 5 means that the transaction failed and status 3 means that the transaction is still pending/sending.
*	swap_tracker_object -
A collection used by the leader/signers to track which block (or transaction) they are monitoring on chain. E.g. if the leader is restarted, it shouldn’t have to go through the entire blockchain again - merely start from where it left off.
There are two types of values here. Ethereum block heights - used by the signers monitoring the ETH side. Secret token nonces - used by the signers on the Secret side to find new swaps. Note, that the nonce value is the last seen transaction - so if you see nonce 44 in the database, that means that #44 has been executed and it is now searching for #45.
*	logs -
The leader/signers sends all their logs of level Warning and above to the DB, where we can inspect them easily. This can sometimes (all the time) get spammy, so be sure to inspect by date. TBD - fix the bugs/issues that cause the logs to get spammed in certain cases
*	token_pairing -
A map of tokens supported on the bridge. This collection contains documents that specify the mapping between ETH (or other forgein network) and their Secret counterparts. The leaders/signers update from this collection automatically. The UI uses this collection as well. For this reason, the “display_props” field exists - it contains mostly things used by the UI for display purposes. Labels, Icons, and other fields.
*	operations -
Operations are an entity used by the UI to manage the status of bridge swap transactions. We use them because the UI wants to be able to give feedback on the status of user interactions even before it reaches the leader/signers (e.g. metamask transaction failed).
Each operation is identified by a UUID. This UUID is what the user sees, and it is the field we ask users to provide when trying to troubleshoot a failed transaction. You can find the details for this operation in the Swap collection, by filtering {“operation”: “<UUID>”}. If it doesn’t appear, that means there is no swap that was triggered by that operation, and it probably failed in an earlier stage.
*	signatures -
Stores the Secret Network signatures created by the signers, and is used by the leader to aggregate the signatures into a multisig transaction. There is a map from the Signatures and the Swap collections by the id of the swap object.
*   eth_signatures - 
    Stores a objects that map swap id and ethereum tx hash. This is useful for the signers to know who else signed the transaction, and for operators to quickly locate transactions that relate to a specific swap
*	scrt_tip_addresses -
Record of all addresses that got a tip from the ScrtSender Azure Function (the thing that sends you 0.7 SCRT on your first swap. Used by the ScrtSender to know which addresses are new and which have already received their tip.
*	scrt_retry -
Used when trying to retry a Secret -> ETH transaction. Retries are sent using a special token and nonce to the multisig contract. This collection keeps a map of the original secret network token/nonce and the retried one. This allows the signers to validate that the transaction really happened on Secret, and that the details match the Withdraw transaction (on ETH)
*   token_price - 

## Swap Statuses
	
* 1 = SWAP_UNSIGNED - swap is waiting to be signed
* 2 = SWAP_SIGNED - swap has been signed by all parties (ETH -> SCRT only)
* 3 = SWAP_SUBMITTED - swap is sent on chain, waiting for confirmation
* 4 = SWAP_CONFIRMED - swap successful
* 5 = SWAP_FAILED - swap failed
* 6 = SWAP_RETRY - retrying swap
 
Additional Operation specific statuses (for UI only) -
* 7 = SWAP_WAIT_SEND
* 8 = SWAP_WAIT_APPROVE
* 9 = SWAP_SENT


## Database Permissions

The bridge leader and the signers have different permissions for the Database. 

#### Leader Permissions
The leader needs read/write on all the database collections. 

#### Signer Permissions

The signers only need read access on the Swap collection.  