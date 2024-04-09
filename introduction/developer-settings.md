# Developer Settings

Developers can can configure the following settings on Phantom's extension or mobile apps:

* [Testnet Mode](developer-settings.md#testnet-mode)
* [Preferred Explorer](developer-settings.md#preferred-explorer)


<figure><img src="../.gitbook/assets/Developer Settings.png" alt=""><figcaption></figcaption></figure>


### Testnet Mode

Developers can configure the following testnets in testnet mode:

* Solana
  * Solana Testnet
  * Solana Devnet
* Ethereum and Polygon
  * Ethereum Goerli
  * Ethereum Sepolia
  * Polygon Mumbai

### Web View Debugging

Developers can debug their mobile web dapps using PC browsers.

## iOS
1. Tether a device to your PC via USB
2. Open Safari Preferences -> "Advanced" tab -> enable checkbox "Show Develop menu in menu bar"
3. Open your web dapp in Phantom on your tethered device
4. Safari -> Develop -> [device name] -> [app name] -> [url - title]

## Android
1. Tether a device to your PC via USB 
2. Open your web dapp in Phantom on your tethered device
3. Open chrome://inspect/#devices on Chrome
4. Select your device on the left and select "Inspect" on the WebView contents you'd like to inspect

See the [React Native Webview Docs](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Debugging.md#debugging-webview-contents) for more info.

<figure><img src="../.gitbook/assets/Preferred Explorer.jpg" alt=""><figcaption></figcaption></figure>

### Preferred Explorer

Developers can choose the default block explorers for all the three chains that are currently supported by Phantom: Solana, Ethereum and Polygon. The following options can be chosen:

* Solana
  * [Solana Beach](https://solanabeach.io/)
  * [Solscan](https://solscan.io/) (Default)
  * [Solana Explorer](https://explorer.solana.com/)
  * [Solana FM](https://solana.fm/)
* Ethereum
  * [Etherscan](https://etherscan.io/)
* Polygon
  * [Polygonscan](https://polygonscan.com/)