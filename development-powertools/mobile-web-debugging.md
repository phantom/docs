
### Mobile Web Debugging

<figure><img src="../.gitbook/assets/Developer Settings.png" alt=""><figcaption></figcaption></figure>

Developers can debug their mobile web dapps using PC browsers. To enable this in Phantom mobile go to Settings -> Developer Settings and toggle Web View Debugging. Then follow the steps below for your platform.

## iOS
1. Tether you iPhone to your computer via USB
2. On your iphone go to Settings -> Safari -> Advanced and enable "Web Inspector" 
3. On your computer open Safari, then from the menu bar "Safari" -> "Settings" -> "Advanced" tab -> enable checkbox "Show features for web developers"
4. Open your web dapp in Phantom on your tethered device
5. Safari -> Develop -> [device name] -> [app name] -> [url - title]

## Android
1. Tether Android phone to your computer via USB 
2. Open your web dapp in Phantom on your tethered device
3. Open chrome://inspect/#devices on Chrome
4. Select your device on the left and select "Inspect" on the WebView contents you'd like to inspect

See the [React Native Webview Docs](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Debugging.md#debugging-webview-contents) for more info.