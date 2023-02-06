# Establishing a Connection

Once an application has [detected the provider](../../solana/integrating-phantom/extension-and-in-app-browser-web-apps/detecting-the-provider.md), it can then request to connect to Phantom. This connection request will prompt the user for permission to share their public key, indicating that they are willing to interact further. Users must approve a connection request before the app can make additional requests such as [signing a message](signing-a-message.md) or [sending a transaction](sending-a-transaction.md).

Once permission is established for the first time, the web application's domain will be whitelisted for future connection requests. After a connection is established, it is possible to terminate the connection from both the application and the user side.

## Connecting

The default way to connect to Phantom is by calling `window.ethereum.request` function.

```javascript
const provider = getProvider(); // see "Detecting the Provider"
try {
    const accounts = await provider.request({ method: "eth_requestAccounts" });
    console.log(accounts[0]);
    // 0x534583cd8cE0ac1af4Ce01Ae4f294d52b4Cd305F
} catch (err) {
    // { code: 4001, message: 'User rejected the request.' }
}
```

The `eth_requestAccounts` method will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise). If it resolves, it is an array where the connected address is in the 0th index, and rejects (throw when awaited) when the user declines the request or closes the pop-up. See [Errors](../../solana/integrating-phantom/errors.md) for a breakdown of error messages Phantom may emit.

When the user accepts the request to connect, the provider will also emit a `connect` event that contains the chainId of the network the user is connected to.

```typescript
provider.on("connect", (connectionInfo: { chainId: string }) => console.log(`Connected to chain: ${connectionInfo.chainId}`));
```

Once the web application is connected to Phantom, it will be able to read the connected account's address and prompt the user for additional transactions. It also exposes a convenience `isConnected` boolean.

```javascript
console.log(provider.selectedAddress);
// 0x534583cd8cE0ac1af4Ce01Ae4f294d52b4Cd305F 
console.log(provider.isConnected());
// true
```

## Disconnecting

There is no way to programmatically disconnect a user from their connection once they have established one.\
\
Once a user has established a connection, Phantom will add the website they opened a connection with to a list of "trusted apps." The user can then revoke access through the UI at any time, and will then need to reconnect. Phantom will attempt to reconnect to any application that is added to the users "trusted apps" automatically.

## Changing Accounts

Phantom allows users to seamlessly manage multiple accounts (i.e. addresses) from within a single extension or mobile app. Whenever a user switches accounts, Phantom will emit an `accountsChanged` event.

If a user changes accounts while already connected to an application, and the new account had already whitelisted that application, then the user will stay connected and Phantom will pass the public key of the new account:

```typescript
provider.on('accountsChanged', (publicKeys: String[]) => {
    if (publicKeys) {
        // Set new public key and continue as usual
        console.log(`Switched to account ${publicKeys[0]}`);
    } 
});
```

If Phantom does not pass the public key of the new account, an application can either do nothing or attempt to reconnect:

```typescript
provider.on('accountsChanged', (publicKeys: String[]) => {
    if (publicKeys) {
      // Set new public key and continue as usual
      console.log(`Switched to account ${publicKeys[0].toBase58()}`);
    } else {
      // Attempt to reconnect to Phantom
      provider.request({ method: "eth_requestAccounts" }).catch((error) => {
        // handle connection failure
      });
    }
});
```
