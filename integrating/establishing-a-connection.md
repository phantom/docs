# Establishing a Connection

In order to start interacting with Phantom you must first establish a connection. This connection request will prompt the user for permission to share their public key, and indicate that they are willing to interact further. 

Once permission is established the first time, the web application's domain will be whitelisted for future connection requests. The user may also indicate that they are willing to auto-approve certain transactions from the application as well.

Similarly, it is possible to terminate the connection both on the application and the user side.

## Connecting

The **recommended** and **easiest** way to connect to Phantom is by calling `window.solana.connect()` . However, the provider also exposes a `request` JSON RPC interface.

{% tabs %}
{% tab title="connect\(\)" %}
```javascript
window.solana.connect();
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript
window.solana.request({ method: "connect" })
```
{% endtab %}
{% endtabs %}

When the user has accepted the request to connect, the provider will emit a `connect` event.

```javascript
window.solana.on('connect' () => console.log("connected!"))
```

Once the web application is connected to Phantom, it will be able to read the connected account's public key and send transactions. It also exposes a convenience `isConnected` boolean, and will indicate whether automatic transaction approvals have been enabled by the user.

```javascript
window.solana.publicKey.toString()
// 26qv4GCcx98RihuK3c4T6ozB3J7L6VwCuFVc7Ta2A3Uo 
window.solana.isConnected
// true
window.solana.autoApprove
// true or false
```

## Disconnecting

Disconnecting is similar to connecting, however, it is possible for the disconnection to originate from the wallet.

{% tabs %}
{% tab title="disconnect\(\)" %}
```javascript
window.solana.disconnect();
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript
window.solana.request({ method: "disconnect" });
```
{% endtab %}
{% endtabs %}

```javascript
window.solana.on('disconnect' () => console.log("disconnected!"))
```



