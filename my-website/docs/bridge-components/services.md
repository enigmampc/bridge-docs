---
sidebar_position: 4
---

# Backend Services

The “backend” in this case refers to the backend services powering the bridge UI. It is separate from the bridge infrastructure, and does not affect the swaps themselves, only the display layer.

You can find the codebase here: enigmampc/EthereumBridgeBackend (github.com)

The backend has two types of services:


* API - this is mainly a gateway that the UI uses to access the database. Most of the logic is database read/writes without too much heavy lifting. This includes – token lists, swap history & statuses, etc. You can find a list of endpoints here: https://github.com/enigmampc/EthereumBridgeBackend/blob/363c35b85737035043aedff53905df753c61cb0c/src/app.ts#L91

* Scheduled tasks - These are tasks that we want to perform once every X amount of time, and while they might affect the UI, they are not triggered by it, or they are expensive and should be executed independently. These will appear under the /serverless folder.

There are two types of scheduled tasks –
* 	Bridge tasks – these are the tasks we care about for the bridge. They are the ones under /serverless/bridge-tasks
*	Secretswap tasks – these are general tasks that relate to secretswap. They can be ignored for the purpose of creating a bridge.

The current scheduled bridge tasks are -
1.	Price fetch - fetches the prices for every pair in USD (in USDT, but close enough).
2.	TVL updates - checks the balance of each token & ETH locked in the multisig contract and updates the database with those values. Also uses the price to update the TVL in USD (so the API doesn’t have to do it each time)
3.	ScrtSender - sends a small amount of SCRT to each new address that crosses the bridge
4.	TotalLockedFetch - fetches the total locked amount of each coin stored in the bridge

Scheduled tasks are deployed as Azure functions (serverless functions), while the API is deployed as an Azure Web App. 
Explaining how this works is beyond the scope of what I feel like writing right now, but for what we’re doing, most of the stuff is intuitive. 
There are also CD scripts for Github Actions in the repository to setup automatic deployment. 

We chose Azure since we were using it anyway for other stuff, not for any technical reason. Porting the functions to another serverless infrastructure (AWS lambda, Google whatever, etc.) should be relatively straightforward.

