---
sidebar_position: 2
---

# Known Issues & Resolutions

## Bridge is down! What to do?

Usually all it takes is a restart.

We recommend making sure you're executing the bridge in an environment with an automated health check and remediation.
This way, the bridge will automatically restart the container, so if it’s down there is probably a bigger issue.

We use docker swarm for this (even though we're only running a single container on a single node).

In this case, you can make sure the container is up via docker ps. It should show something like 

```
CONTAINER ID   IMAGE                        COMMAND                  CREATED        STATUS                  PORTS     NAMES
a9ce6839e234   enigmampc/eth-bridge:1.4.0   "/bin/sh -c 'python3…"   42 hours ago   Up 42 hours (healthy)             bridge_leader.1.jldbev8hnqtd16rrdsfp2simw
```

You can follow the logs using:

	docker logs -f bridge_leader.1.xxxxxxx 

If the bridge stays down it’ll take some further investigation. Most likely causes are:

1. Secret Node crashing - you’ll see secretcli errors if this happens
2. Signer ran out of ETH/BNB - you’ll see some web3 signature errors if this happens. You can also check the current balance of the signer from http://localhost:8888/health
3. Possibly (although this never happened) there might be an error connecting to the DB - if this happens, check your DB provider, cry a lot and hope that a restart solves the problem

## Bridge is down! But we are online!	

Use the data in the “signer_health” collection in the DB to figure out which signer is offline. In the meantime, make sure to go to the logs and try to figure out what went wrong.

You should be monitoring the status of all signers to catch a signer dropping early so as to not reach a state where this causes a failure

## Someone reports losing funds. How to investigate?

Ask them for the operation id (it will be in the form of a uuid). Log in to the DB using your favorite browser, and go to the “operations” collection. Search for that ID. Does the swap field have an ID? If so, you can follow that to the “swap” collection (the id will match an object there) and look at the swap transaction. 

The only field you care about is the “Status” field. If the status is “4”, it was successful and you just have to look at how much was sent and where. If the status is “5”, the transaction failed. 

To investigate a failed transaction, you need to first check why it failed. Look at the timestamp, then go to the “logs” field. Find the error logs matching that timestamp and figure out what caused it to fail. Sometimes, ETH->SCRT transactions will fail due to one of the signers failing to verify it (most likely due to web3 errors). If that happens, you can just resend the transaction.

To resend any transaction, change the status of the transaction to “6”. Make sure to verify that the transaction really did fail before doing this.

The last case is that the transaction is stuck in “sending” (most likely in the SCRT->ETH direction).
First make sure that the transaction hash in “dst_tx_hash” appears on Etherscan. The leader can sometimes fail to send transactions for some reason which I’m not sure why.
If this is the case, you can resend the TX by setting the status to “6”.

If the tx hash is on-chain, this means that some signers missed the transaction, or are not online. First, check the status of the signers via the “signer_health” collection. If they are all online, go to the “eth_signatures” collection, and find that transaction. Take the tx_hashes from there, and use Etherscan to check why the transaction is still pending. Transactions that are “stuck” because a signer is offline will be resent once the signer comes back online, so be very careful when resending a stuck transaction. The way to make sure the offline signers do not resend the transaction when they come back online is to fast forward the tracking object in the “swap_tracker_object” collection matching the “signer-XXXX” key to the current Ethereum block (or any after the pending transaction).

## Backend API is down

Sometimes the API has trouble connecting to the database and needs a restart. Follow pretty much the same steps from above - restart it

HTTP 5xx error rate is a good indicator of if things are working
