---
sidebar_position: 4
---

# Oracle Deployment

## Set up Environment

The bridge oracles (both leader/signer) are based on the same Docker image. We use a docker-compose file to set the environment variables, executed inside a Docker Swarm. You can find an example docker-compose file here.
For EVM-chain keys you can use either a raw private key, or keys stored in SoftHSMv2 on the host machine. Other PKCS#11 stores may be added upon request.

Of all available config parameters, the ones that require setting environment variables are:

General
SWAP_ENV – Set this to the environment you created in the setup steps above.

Node addresses
eth_node - address of EVM RPC gateway (or service like infura)
secret_node - address of secret network rpc node

Secret Network private key
secret_key_name
secret_key_file
secret_key_password

Ethereum private key
eth_private_key
eth_address

OR if PKCS11 is used
token
user_pin
label
pkcs11_module

Provided by leader
db_username - database username
db_password - database password
db_host - hostname of database service provider
multisig_acc_addr - Secret Network multisig address
multisig_wallet_address - Ethereum multisig contract address
secret_signers - comma-separated list of the public keys of addresses that comprise the address in multisig_acc_addr
scrt_swap_address - address of secret contract handling our swaps
swap_code_hash - code hash of the secret contract handling our swaps

## Create id_tx_io.json
Create a new file, in the path specified by /replace/this/with/keys/path/, called id_tx_io.json. This file will contain the shared transactional key, so the signers can decrypt the transactions that the leader creates. Both the leader and the signers share this file, as it allows them to share encryption/decryption keys.

## Start the signer
docker-compose up

## UI
Amazing! Assuming everything up till now went smoothly, you have a bridge working between your new EVM chain and Secret Network! Now, lets see how we can get a UI running.

