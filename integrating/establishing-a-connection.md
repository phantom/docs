# Establishing a Connection

In order to start interacting with Phantom, you must first establish a connection. This connection request will prompt the user for permission to share their public key, indicating that they are willing to interact further. Once permission is established the first time, the web application's domain will be whitelisted for future connection requests.&#x20;

Similarly, it is possible to terminate the connection both on the application and the user side.

## Connecting

The **recommended** and **easiest** way to connect to Phantom is by calling `window.solana.connect()` . However, the provider also exposes a `request` JSON RPC interface.

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

The call will return a Promise that resolves when the user has accepted the connection request, and reject (throw when awaited) when the user declines the request or closes the pop-up. See [Errors](errors.md) for a breakdown of error messages Phantom may emit.

When the user has accepted the request to connect, the provider will also emit a `connect` event.

```javascript
window.solana.on("connect", () => console.log("connected!"))
```

Once the web application is connected to Phantom, it will be able to read the connected account's public key and send transactions. It also exposes a convenience `isConnected` boolean.

```javascript
window.solana.publicKey.toString()
// 26qv4GCcx98RihuK3c4T6ozB3J7L6VwCuFVc7Ta2A3Uo 
window.solana.isConnected
// true
```

## Eagerly Connecting

After a web application connects to Phantom for the first time it becomes trusted, and it is possible for the application to automatically connect to Phantom on subsequent visits, or page refreshes. This is often called "eagerly connecting".

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

When using this flag, Phantom will only connect and emit a `connect` event if the application is trusted. Therefore, this can be safely called on page load for new users, as they won't be bothered by a pop-up window even if they have never connected to Phantom.

Here is an example of how you might use this to implement "eagerly connecting" in a React application.

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

## Disconnecting

Disconnecting is similar to connecting, however, it is possible for the disconnection to originate from the wallet.

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
