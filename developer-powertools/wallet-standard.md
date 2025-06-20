# Wallet Standard

### Introduction

[Wallet Standard](https://github.com/wallet-standard/wallet-standard) is a chain-agnostic set of interfaces and conventions that aim to improve how applications interact with injected wallets such as Phantom. The standard was pioneered on Solana and is used as the foundation for the [Solana Wallet Adapter](https://github.com/solana-labs/wallet-adapter). Phantom supports the Wallet Standard and is open to working with others to bring this innovation to other ecosystems.

### Motivation

Most desktop wallets today come in the form of browser extensions. These extensions interact with dApps by injecting code in the form of a [provider](../solana/detecting-the-provider.md) into every website the user visits. There are several issues with the way injection works today:

#### Issue #1: Interferences Between Multiple Injected Wallets

While wallets often injected into their own designated namespace (e.g. `window.phantom`), there is nothing stopping another wallet from injecting into the same namespace. Furthermore, any wallet can attempt to mimic another by changing its identifier flags (e.g. `window.solana.isPhantom = true`). As a result, users who have multiple injected wallets can often experience unwanted interferences.

#### Issue #2: Lack of Automatic dApp Support

Since wallets attach themselves to the window as global objects, dApps need to be made aware of how they can find these objects. Instead of automatically detecting all wallets that the user has installed, a dApp must choose to support a limited number of wallets that may not be relevant to the user.

#### Issue #3: Lack of Standardized Feature Support

Wallets inconsistently support features such as signing and sending transactions, signing more than one transaction, signing a "message" (arbitrary byte array), and encryption and decryption. To the extent they support these features, wallets may have different interfaces which can make integrations complex and time consuming.

### Wallet-Standard Overview

Wallet Standard addresses these issues by introducing an event-based model that

1. Standardizes the way wallets attach themselves to the window
2. Defines a set of standard APIs that dApps can rely upon

At a high-level, Wallet Standard defines the following [Window interface for events](https://github.com/wallet-standard/wallet-standard/blob/ce280f05f6eb16d0e77939827e0b7fa294460e1f/packages/core/base/src/window.ts#L16-L25):

```typescript
export interface WalletEventsWindow extends Omit<Window, 'addEventListener' | 'dispatchEvent'> {
    /** Add a listener for {@link WindowAppReadyEvent}. */
    addEventListener(type: WindowAppReadyEventType, listener: (event: WindowAppReadyEvent) => void): void;
    /** Add a listener for {@link WindowRegisterWalletEvent}. */
    addEventListener(type: WindowRegisterWalletEventType, listener: (event: WindowRegisterWalletEvent) => void): void;
    /** Dispatch a {@link WindowAppReadyEvent}. */
    dispatchEvent(event: WindowAppReadyEvent): void;
    /** Dispatch a {@link WindowRegisterWalletEvent}. */
    dispatchEvent(event: WindowRegisterWalletEvent): void;
}
```

When a wallet is ready to inject into a website, it will [dispatch](https://github.com/wallet-standard/wallet-standard/blob/ce280f05f6eb16d0e77939827e0b7fa294460e1f/packages/core/wallet/src/register.ts#L27-L38) a `register-wallet` event and listen for an `app-ready` event. Conversely, an app will [dispatch](https://github.com/wallet-standard/wallet-standard/blob/master/packages/core/app/src/wallets.ts#L39-L50) an `app-ready` event and listen for `register-wallet` events.

### Features of Wallet Standard

#### Avoids Code Bloat

Before Wallet Standard, if a dApp wanted to support 20 wallets, it had to load 5kb of code per wallet (100k total). This bloating increases linearly as the number of wallets increase. The vast majority of this code goes unused, as the user will only use one or two wallets.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Wallet Standard turns this inside out: If dapps want to support 100 wallets, they just to support Wallet Standard. Any wallet that supports the standard is will automatically be detected and supported. Implementations are replaced with interfaces.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

#### Eliminates Attack Surfaces

Before Wallet Standard, every new wallet had to land a PR in a library such as the Solana Wallet Adapter. Library maintainers had to vet hundreds of PRs to ensure they weren't introducing malware into the adapter. By eliminating plugins, these libraries can now eliminate attack surfaces.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

#### Multichain By Default

While Wallet Standard was pioneered on Solana, it is chain-agnostic by default. An example EIP-1193 implementation can be found [here](https://github.com/wallet-standard/wallet-standard/pull/85).

#### No web3.js Dependency

Wallet Standard will always input and output transactions, pubkeys, and signatures as raw bytes (`Uint8Array`). As such, there are no dependencies for `web3.js` .

### For dApp Developers

Phantom comes with built-in support for Wallet Standard on **Solana**. To get started, simply update the [Solana Wallet Adapter](https://github.com/solana-labs/wallet-adapter) to the latest release. For a demo, check out the [Wallet Adapter Example](https://solana-labs.github.io/wallet-adapter/example/).

{% hint style="danger" %}
**BREAKING CHANGE:** If you are migrating an existing Solana dApp to use Wallet Standard, please ensure that you are not mutating transactions in place. Failure to do will result in a breaking change. For more information, please see the below threads.
{% endhint %}

{% embed url="https://twitter.com/0xprof_lupin/status/1613252832525955073" %}

### Further Reading

{% embed url="https://github.com/wallet-standard/wallet-standard/blob/master/DESIGN.md" %}
