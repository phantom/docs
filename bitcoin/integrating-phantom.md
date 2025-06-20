# Getting Started With Bitcoin

The Phantom browser extension and mobile in-app browser both support interacting with Bitcoin and Ordinals dapps. As Phantom version `23.19`, users can enable Bitcoin by going to `Settings` > `Active Networks` and enabling Bitcoin like so:

<div data-full-width="false"><figure><img src="../.gitbook/assets/Screen Shot 2023-12-20 at 10.49.27 AM.png" alt="" width="355"><figcaption></figcaption></figure></div>

There are two main ways to integrate Phantom into your web application:

### Direct Integration

The most direct way to interact with Phantom is via the provider that Phantom injects into your web application. This provider is globally available at `window.phantom` and its methods will always include Phantom's most up-to-date functionality. This documentation is dedicated to covering all aspects of the provider.

When adding a Phantom button to your dapp’s wallet modal, we recommend using the name “Phantom” with an SVG/PNG icon which can be found [here](https://docs.phantom.app/resources/assets).

<figure><img src="../.gitbook/assets/image (14).png" alt="" width="375"><figcaption><p>A example Phantom button in a wallet modal</p></figcaption></figure>

### Wallet Standard

Applications can also integrate Phantom by adding support for [Wallet Standard](https://github.com/wallet-standard/wallet-standard). The Bitcoin-specific extensions for Wallet Standard can be found [here](https://github.com/ExodusMovement/bitcoin-wallet-standard?tab=readme-ov-file).
