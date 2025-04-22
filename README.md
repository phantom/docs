---
cover: .gitbook/assets/phantom-background.jpeg
coverY: -50.54759898904802
---

# Introduction

**Phantom** is a crypto wallet that can be used to manage digital assets and access decentralized applications on [Solana](https://solana.com/), [Sui](https://sui.io/), [Polygon](https://polygon.technology/), and [Ethereum](https://ethereum.org/en/).&#x20;

Phantom is currently available as:

* A [browser extension](https://phantom.app/download)
* An [iOS app](https://apps.apple.com/us/app/phantom-solana-wallet/id1598432977)
* An [Android app](https://play.google.com/store/apps/details?id=app.phantom)

At its core, Phantom works by creating and managing private keys on behalf of its users. These keys can then be used within Phantom to store funds and sign transactions.&#x20;

Developers can interact with Phantom via both **web** applications as well as **iOS and Android** applications.

To interact with web applications, the Phantom [extension and mobile in-app browser](broken-reference) injects a `phantom` object into the javascript context of every site the user visits. A given web app may then interact with Phantom, and ask for the user's permission to perform transactions, through this injected provider.

It's also possible to interact with the Phantom mobile app through [universal links and deeplinks](phantom-deeplinks/deeplinks-ios-and-android.md). With deeplinks, mobile apps can prompt their users to connect, sign, and send with Phantom directly. Once complete, Phantom will redirect users back to their referring applications.

This documentation is intended for developers who are building applications with Phantom. If you are a developer looking for help with an integration, please check out our [developer discord.](https://discord.gg/j5Dp7ztzvW) For all other support requests, please visit our [Help Center](https://help.phantom.app/).

