---
description: Welcome to the Phantom developer documentation
---

# Introduction

**Phantom** is a crypto wallet that can be used to manage digital assets and access decentralized applications on the [Solana](https://solana.com) blockchain.&#x20;

Phantom is currently available as:

* A [browser extension](https://phantom.app/download)
* An [iOS app](https://apps.apple.com/us/app/phantom-solana-wallet/id1598432977)
* An [Android app](https://play.google.com/store/apps/details?id=app.phantom) (in beta!)

At its core, Phantom works by creating and managing private keys on behalf of its users, allowing them to store funds and sign transactions.&#x20;

To interact with web applications, Phantom injects a `solana` object into the javascript context of every web application the user visits. A given application may then interact with Phantom, such as asking for permission to perform a transaction, through this injected object.

This documentation is intended for developers who are building applications with Phantom. To get help with using Phantom, please visit our [User Support Site](https://help.phantom.app).

