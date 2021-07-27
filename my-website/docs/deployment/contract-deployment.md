---
sidebar_position: 3
---

# Contract Deployment

## Deploying EVM Contracts

Step 1 is getting some ETH.
If you changed the EVM contract, you will need to recompile it. I recommend using Remix for this - it’s simple. To get it to compile, simply replace the import statements with the commented ones, then compile, and you will end up with a json file.
The current compiled contract is saved here.
Then simply run something like:
contract = w3.eth.contract(abi=contract_source_code['abi'], bytecode=contract_source_code['data']['bytecode']['object'])
tx = contract.constructor(signer_accounts, config.signatures_threshold, "0xA48e330838A6167a68d7991bf76F2E522566Da33")
nonce = w3.eth.getTransactionCount(account.address, "pending")
raw_tx = tx.buildTransaction(transaction={'from': account.address, 'gas': 3000000, 'nonce': nonce})
signed_tx = account.sign_transaction(raw_tx)
tx_hash = w3.eth.sendRawTransaction(signed_tx.rawTransaction)
You can find the Eth contract deployment code here. If you’re using this script, you will need to set your eth_node configuration parameter (or environment variable) to either an infura endpoint or an Ethereum node.
In addition, it is important to remember to check the constructor parameters - (Signer addresses, multisig threshold, fee collector address). 

If everything went well, you should end up with an Ethereum contract address.

The last step is to whitelist any tokens we want the bridge to support. We use a whitelist so no one tries to swap some unsupported token then starts crying that he lost it.

To whitelist a token you need the token address, and to set a minimum value that can be swapped. Then, you call the addToken(address, uint256) method in the deployed multisig contract. You can also use our handy python code to do this from here

On mainnet transactions may take more than 120 to register on-chain, in which case they will timeout. In such a case, wait for the transaction to be approved, then manually continue from where it left off. Or fix the script so it doesn’t die after 120 seconds - either or.

When you’re all done save the multisig contract address in the appropriate configuration file, under the multisig_wallet_address parameter.

## Deploying Secret Contracts

## 

If you made any changes to the Secret Contracts, you will need to upload and redeploy the code. If you made no changes, feel free to skip to the next step

First things first, make sure you have an account with SCRT.

So lets store the bridge swap code and our token contract. For the token contract we use a SNIP-20 contract that has minting and burning. Both contracts are compiled and waiting here. 

Store both contracts using 
secretcli tx compute store swap.wasm.gz --from t1 --gas 1500000
secretcli tx compute store token.wasm.gz --from t1 --gas 1500000

Don’t forget to check the resulting code id.

### 