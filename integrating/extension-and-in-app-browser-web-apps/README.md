# Extension & In-app Browser (Web Apps)

The Phantom browser extension and mobile in-app browser are both designed to interact with web applications. For mobile apps, we recommend integrating via [universal links or deeplinks](../deeplinks-ios-and-android/).

There are two main ways to integrate Phantom into your web application:

### Direct Integration

The most direct way to interact with Phantom is via the provider that Phantom injects into your web application. This provider is globally available at `window.solana` and its methods will always include Phantom's most up-to-date functionality. This documentation is dedicated to covering all aspects of the provider.

### Solana Wallet Adapter

Another quick and easy way to get up and running with Phantom is via the [Solana Wallet Adapter](https://github.com/solana-labs/wallet-adapter/) package. The wallet adapter is a set of modular TypeScript components that allow developers to easily integrate multiple Solana wallets into their applications. This package includes starter files, setup and usage instructions, and a live demo showcasing multiple UI frameworks.
