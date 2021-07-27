---
sidebar_position: 6
---

# UI

## Deploying a new UI

## Adding new chain to an existing UI

The UI is mostly extensible, although it does take some work. This section will describe all the sections that must be changed to add a new network.

src/blockchain-bridge/eth/networks.ts – Add your new network to the enum list
src/blockchain-bridge/eth/index.ts – Methods & contract definitions for new chain
src/blockchain-bridge/eth/chainProps.ts - Definitions for your chain, including logo, token name, etc.
src/services/index.ts – your network to availableNetworks, a url to the specific backend to BACKENDS and the appropriate environment variable for the backend
