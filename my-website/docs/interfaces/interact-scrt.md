---
sidebar_position: 2
---

# SNIP20

Few words on SNIP20s, link to spec. 

# Transfer SNIP20 tokens from Secret Network

## send (Send SNIP20 to bridge)

Call the _Send_ method of the SNIP20 token, with the recipient being the swap contract, and the msg field containing the base64 encoded ethereum address

```bash
secretcli tx compute execute '{"send": {"recipient": "secret1hx84ff3h4m8yuvey36g9590pw9mm2p55cwqnm6", "amount": "200", "msg": "MHhGQmE5OGFEMjU2QTM3MTJhODhhYjEzRTUwMTgzMkYxRTNkNTRDNjQ1"}}' --label <secret-contract-label> --from <key> --gas 500000
```

## Query Supported Tokens

```
Tokens {}
```

## Query Swap

You can query events emitted by the swap contract by specifying an address and nonce (together these make up a unique ID for a swap)

```
Swap { nonce: u32, token: HumanAddr }
```


