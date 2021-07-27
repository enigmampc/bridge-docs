---
sidebar_position: 7
---

# Adding New Tokens

To add a new token, you have to do the following steps:


1.	Whitelist token on the Ethereum multisig contract (multisig transaction)
2.	Deploy token on Secret Network
3.	Remove deployer from token minters
4.	Add swap contract to token minters
5.	Change token contract admin to multisig address
6.	Whitelist token on Secret Network swap contract (multisig transaction)
7.	Update database with new token pairing

Not going to go into detail on each of these steps, as they are all described in the document above.

Adding a new token on the SCRT side is now easily automated via http://localhost:8888/add_token?<params>. You’ll need the API key from the docker-compose file. 
