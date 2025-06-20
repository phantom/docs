# Getting Started with EVM networks

The Phantom browser extension and mobile in-app browser are both designed to interact with web applications. EVM web apps can interact with Phantom via the provider that is injected at `window.phantom.ethereum`. This provider conforms to the [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193) standard and is also injected at `window.ethereum` to support legacy integrations. Phantom also supports [EIP-6963](https://eip6963.org/) for ease of integration.

Additionally, Phantom has enabled support for the Monad Testnet, allowing developers and users to interact with the Monad network before its mainnet launch.

Developers can integrate Monad Testnet support using Phantom’s existing EVM provider by adding the relevant [network parameters](https://docs.monad.xyz/getting-started/network-information) and referring to the Monad documentation for any [special considerations](https://docs.monad.xyz/getting-started/differences).

Note: Users must enable the Monad Testnet in Phantom’s settings before interacting with it. Once Monad mainnet support is enabled by default, this step will no longer be necessary.

This documentation is dedicated to covering all aspects of the provider.
