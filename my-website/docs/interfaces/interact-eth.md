---
sidebar_position: 1
---

# Transfer native coins or ERC20s to Secret Network

intro...

## swap (Transfer ETH)

```javascript
function swap(bytes memory _recipient)
```

Call the method swap on our MultiSig contract, specifying the amount sent in the value, and the destination in the arguments.

```python
tx_hash = send_contract_tx(multisig_wallet.tracked_contract, 'swap',
                           account, bytes.fromhex(private_key), "secret13l72vhjngmg55ykajxdnlalktwglyqjqv9pkq4", value=200)
```


# swapToken (Transfer ERC20)

```javascript
function swapToken(bytes memory _recipient, uint256 _amount, address _tokenAddress)
```

Give the multisig contract allowance, then call the swapToken function on our multisig contract, which has the signature swapToken(bytes memory recipient, uint256 amount, address token). The first parameter being the destination address, the second being the number of tokens, and the 3rd the address of the ERC-20 token we wish to transfer.
Note that the transaction will fail if attempted to transfer from a token which is not on the token whitelist

```python
TRANSFER_AMOUNT = 100

_ = erc20_contract.tracked_contract.functions.approve(multisig_wallet.address, TRANSFER_AMOUNT). \
    transact({'from': address})


tx_hash = multisig_wallet.tracked_contract.functions.swapToken(dest_address.encode(),
                                                       TRANSFER_AMOUNT,
                                                       erc20_contract.address). \
    transact({'from': web3_provider.eth.coinbase}).hex().lower()

```

# SupportedTokens (View supported tokens)

```javascript
function SupportedTokens()
```
