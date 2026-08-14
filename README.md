# duc-blockscout

**duc-blockscout** is a fork of [Blockscout](https://github.com/blockscout/blockscout)
(currently rebased on **v11.2.6**), configured as the block explorer for
**Ducros** (ticker **DUC**, chainId **271017**) — the RandomX Proof-of-Work
chain built on [`duchain`](https://github.com/Duc-inc/duchain), a
go-ethereum fork.

Blockscout is consensus-agnostic: it indexes chain data purely over
JSON-RPC (`eth_getBlockByNumber`, `debug_traceTransaction`, etc.), so it
doesn't need to know or care that blocks are mined with RandomX instead of
Ethash/PoS — the miner/coinbase, difficulty, nonce and other header fields
are read the same way regardless of consensus engine.

## Ducros-specific notes

- `CHAIN_ID=271017`, coin `DUC`.
- Block reward display may need adjustment: Ducros mints a fixed 9 DUC per
  block with a 10% treasury cut baked into `core/state_transition.go` on
  `duchain`, which doesn't match upstream's standard Ethereum emission
  assumptions. Worth checking once the explorer is live against real blocks.
- Requires the `duchain` node to expose `debug`/`trace` JSON-RPC APIs (in
  addition to `eth`/`net`/`web3`) for internal transactions.

---

<h1 align="center">Blockscout</h1>
<p align="center">Blockchain Explorer for inspecting and analyzing EVM Chains.</p>
<div align="center">

[![Discord](https://img.shields.io/badge/chat-Blockscout-green.svg)](https://discord.gg/blockscout)

</div>


Blockscout provides a comprehensive, easy-to-use interface for users to view, confirm, and inspect transactions on EVM (Ethereum Virtual Machine) blockchains. This includes Ethereum Mainnet, Ethereum Classic, Optimism, Gnosis Chain and many other **Ethereum testnets, private networks, L2s and sidechains**.

See our [project documentation](https://docs.blockscout.com/) for detailed information and setup instructions.

For questions, comments and feature requests see the [discussions section](https://github.com/blockscout/blockscout/discussions) or via [Discord](https://discord.com/invite/blockscout).

## About Blockscout

Blockscout allows users to search transactions, view accounts and balances, verify and interact with smart contracts and view and interact with applications on the Ethereum network including many forks, sidechains, L2s and testnets.

Blockscout is an open-source alternative to centralized, closed source block explorers such as Etherscan, Etherchain and others.  As Ethereum sidechains and L2s continue to proliferate in both private and public settings, transparent, open-source tools are needed to analyze and validate all transactions.

## Supported Projects

Blockscout currently supports several hundred chains and rollups throughout the greater blockchain ecosystem. Ethereum, Cosmos, Polkadot, Avalanche, Near and many others include Blockscout integrations. A comprehensive list is available at [chains.blockscout.com](https://chains.blockscout.com). If your project is not listed, contact the team in [Discord](https://discord.com/invite/blockscout).

## Getting Started

See the [project documentation](https://docs.blockscout.com/) for instructions:

- [Manual deployment](https://docs.blockscout.com/for-developers/deployment/manual-deployment-guide)
- [Docker-compose deployment](https://docs.blockscout.com/for-developers/deployment/docker-compose-deployment)
- [Kubernetes deployment](https://docs.blockscout.com/for-developers/deployment/kubernetes-deployment)
- [Manual deployment (backend + old UI)](https://docs.blockscout.com/for-developers/deployment/manual-old-ui)
- [Ansible deployment](https://docs.blockscout.com/for-developers/ansible-deployment)
- [ENV variables](https://docs.blockscout.com/setup/env-variables)
- [Configuration options](https://docs.blockscout.com/for-developers/configuration-options)

## Acknowledgements

We would like to thank the EthPrize foundation for their funding support.

## Contributing

See [CONTRIBUTING.md](.github/CONTRIBUTING.md) for contribution and pull request protocol. We expect contributors to follow our [code of conduct](.github/CODE_OF_CONDUCT.md) when submitting code or comments.

## License

[![License: Blockscout Software Licence](https://img.shields.io/badge/License-Blockscout%20Software%20Licence-blue.svg)](LICENSE)

This project is licensed under the Blockscout Software Licence. See the [LICENSE](LICENSE) file for full terms.

Third-party components included in this repository remain subject to their own licenses. See dependency manifests and bundled third-party notices for component-level license terms.
