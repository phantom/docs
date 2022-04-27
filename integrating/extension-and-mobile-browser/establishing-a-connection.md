# Establishing a Connection

In order to start interacting with Phantom, an app must first establish a connection. This connection request will prompt the user for permission to share their public key, indicating that they are willing to interact further. Once permission is established for the first time, the web application's domain will be whitelisted for future connection requests.&#x20;

After a connection is established, it is possible to terminate the connection from both the application and the user side.

## Connecting

The **recommended** and **easiest** way to connect to Phantom is by calling `window.solana.connect()`. However, the provider also exposes a `request` JSON RPC interface.

{% tabs %}
{% tab title="connect()" %}
```javascript
try {
    const resp = await window.solana.connect();
    resp.publicKey.toString()
    // 26qv4GCcx98RihuK3c4T6ozB3J7L6VwCuFVc7Ta2A3Uo 
} catch (err) {
    // { code: 4001, message: 'User rejected the request.' }
}
```
{% endtab %}

{% tab title="request()" %}
```javascript
try {
    const resp = await window.solana.request({ method: "connect" });
    resp.publicKey.toString()
    // 26qv4GCcx98RihuK3c4T6ozB3J7L6VwCuFVc7Ta2A3Uo 
} catch (err) {
    // { code: 4001, message: 'User rejected the request.' }
}
```
{% endtab %}
{% endtabs %}

The `connect()` call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) that resolves when the user accepts the connection request, and reject (throw when awaited) when the user declines the request or closes the pop-up. See [Errors](../errors.md) for a breakdown of error messages Phantom may emit.

When the user accepts the request to connect, the provider will also emit a `connect` event.

```javascript
window.solana.on("connect", () => console.log("connected!"))
```

Once the web application is connected to Phantom, it will be able to read the connected account's public key and prompt the user for additional transactions. It also exposes a convenience `isConnected` boolean.

```javascript
window.solana.publicKey.toString()
// 26qv4GCcx98RihuK3c4T6ozB3J7L6VwCuFVc7Ta2A3Uo 
window.solana.isConnected
// true
```

## Eagerly Connecting

After a web application connects to Phantom for the first time, it becomes trusted. Once trusted, it's possible for the application to automatically connect to Phantom on subsequent visits or page refreshes. This is often referred to as "eagerly connecting".

To implement this, Phantom allows an `onlyIfTrusted` option to be passed into the `connect()` call.

{% tabs %}
{% tab title="connect()" %}
```javascript
window.solana.connect({ onlyIfTrusted: true });
```
{% endtab %}

{% tab title="request()" %}
```javascript
window.solana.request({ method: "connect", params: { onlyIfTrusted: true }});
```
{% endtab %}
{% endtabs %}

When using this flag, Phantom will only connect and emit a `connect` event if the application is trusted. Therefore, this can be safely called on page load for new users, as they won't be bothered by a pop-up window even if they have never connected to Phantom before.

The following is an example of how a React application can eagerly connect to Phantom.

```javascript
import { useEffect } from "react";

useEffect(() => {
    // Will either automatically connect to Phantom, or do nothing.
    window.solana.connect({ onlyIfTrusted: true })
        .then(({ publicKey }) => {
            // Handle successful eager connection
        });
        .catch(() => {
            // Handle connection failure as usual
        })
}, []);
```

If a wallet disconnects from a trusted app and then attempts to reconnect at a later time, Phantom will still eagerly connect. Once an app is trusted, Phantom will only require the user to approve a connection request if the user revokes the app from within their Trusted Apps settings.

## Disconnecting

Disconnecting mirrors the same process as connecting. However, it is also possible for the wallet to initiate the disconnection, rather than the application itself. For this reason, it's important for applications to gracefully handle a `disconnect` event.

{% tabs %}
{% tab title="disconnect()" %}
```javascript
window.solana.disconnect();
```
{% endtab %}

{% tab title="request()" %}
```javascript
window.solana.request({ method: "disconnect" });
```
{% endtab %}
{% endtabs %}

```javascript
window.solana.on('disconnect', () => console.log("disconnected!"))
```
