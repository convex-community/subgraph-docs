# Overview

The Curve Pool Subgraphs track the history of various key metrics (reserves, volume, fees) of Curve liquidity pools across all chains on which The Graph is supported and Curve contracts are deployed.&#x20;

The main contract tracked by the subgraphs is Curve's address provider (`0x0000000022d53366457f9d5e68ec105046fc4383` on all chains), which is then used to retrieve the registry and factory contracts and all associated pools. Due to the reliance on the registries, pools are only tracked once they are added to a registry.

## Deployment addresses

| Chain                | URL                                                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ethereum             | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-mainnet](https://thegraph.com/hosted-service/subgraph/convex-community/volume-mainnet)     |
| Arbitrum             | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-arbitrum](https://thegraph.com/hosted-service/subgraph/convex-community/volume-arbitrum)   |
| Avalanche            | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-avalanche](https://thegraph.com/hosted-service/subgraph/convex-community/volume-avalanche) |
| Fantom               | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-fantom](https://thegraph.com/hosted-service/subgraph/convex-community/volume-fantom)       |
| Harmony (deprecated) | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-harmony](https://thegraph.com/hosted-service/subgraph/convex-community/volume-harmony)     |
| Matic                | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-matic](https://thegraph.com/hosted-service/subgraph/convex-community/volume-matic)         |
| Moonbeam             | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-moonbeam](https://thegraph.com/hosted-service/subgraph/convex-community/volume-moonbeam)   |
| Optimism             | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-optimism ](https://thegraph.com/hosted-service/subgraph/convex-community/volume-optimism)  |
| xDai / Gnosis        | [https://thegraph.com/hosted-service/subgraph/convex-community/volume-xdai](https://thegraph.com/hosted-service/subgraph/convex-community/volume-xdai)           |

## Github Repository

[https://github.com/curvefi/volume-subgraphs](https://github.com/curvefi/volume-subgraphs)
