---
sidebar_position: 2
---

# Preparation

## Generate Multisig Addresses

### Secret Network
First, either collect the public keys from the bridge participants, or generate yourself if you’re running on testnet.

Add the public keys to your secretcli using:
secretcli keys add <name> --pubkey=secretpub1xasdjajdfsjasfdj

Then generate the multisig address using:
secretcli keys add smt1 --multisig=t1,t2,rt3 --multisig-threshold 2

Bam, you’re done. Now you should end up with the multisig address in your secretcli. In the configuration file, set the multisig_acc_addr with this address.

You can find a more detailed guide on how to create a multisig account here

### EVM
Gather all the addresses you need on the EVM side. You’ll need these when deploying the contract.

## Fund addresses

## Create Database
We use Mongo Atlas as a service provider for our Database, but you are free to use any Mongodb provider that you wish, or host your own. The requirements are not very high – 500MB of storage and 512MB of memory should be plenty, with up to 500 concurrent connections.

## Add a Config file
We use config files to specify the parameters of each chain. You can find them all under EthereumBridge/config/xxxx.json.
To add a new file, copy an existing file, then add a new environment file to src/util/config.py
We will update the values in this config file as we go. You can see the description of the config parameters here(link)
