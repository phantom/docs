# Mobile Web Debugging

<figure><img src="../.gitbook/assets/IMG_4683 (1).jpg" alt=""><figcaption></figcaption></figure>

Developers can debug their mobile web dapps using PC browsers. To enable this in Phantom mobile go to Settings -> Developer Settings and toggle Web View Debugging on. Then follow the steps below for your platform.

**iOS**

1. Tether your iPhone to your computer via USB
2. On your iPhone go to Settings -> Safari -> Advanced and enable "Web Inspector"
3. On your computer open Safari. Then from the menu bar Safari -> Settings -> Advanced tab -> enable "Show features for web developers"
4. Open your web dapp in Phantom on your tethered device
5. Back on your computer from the Safari menu bar go to Develop ->  \[Your Device] ->  \[Your Dapp]

**Android**

1. Tether your Android phone to your computer via USB
2. Open your web dapp in Phantom on your tethered device
3. On your computer open chrome://inspect/#devices on Chrome
4. Select your device on the left and select "Inspect" on the Phantom dapp contents you'd like to inspect

See the [React Native Webview Docs](https://github.com/react-native-webview/react-native-webview/blob/master/docs/Debugging.md#debugging-webview-contents) for more info.

{% embed url="https://www.loom.com/share/1b1817d84af649c9a38e5dbd8ed25ca9?t=1" %}
