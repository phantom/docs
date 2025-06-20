# Establishing a Connection

Once an application has detected the provider, it can then request to connect to Phantom. This connection request will prompt the user for permission to share their public key, indicating that they are willing to interact further. Users must approve a connection request before the app can make additional requests such as [signing a message](https://app.gitbook.com/o/-MVOiTvUx4alcPCAxwFd/s/-MVOiF6Zqit57q_hxJYp/~/changes/161/sui-beta/signing-a-message) or [sending a transaction](https://app.gitbook.com/o/-MVOiTvUx4alcPCAxwFd/s/-MVOiF6Zqit57q_hxJYp/~/changes/161/sui-beta/signing-a-transaction).

Once permission is established for the first time, the web application's domain will be whitelisted for future connection requests. After a connection is established, it is possible for the user to terminate the connection from the Phantom settings UI.

## Connecting

The **recommended** and **easiest** way to connect to Phantom is by calling `window.phantom.sui.requestAccount()`.&#x20;

```typescript
const provider = getProvider(); // see "Detecting the Provider"
try {
    const resp = await provider.requestAccount();
    console.log(resp.publicKey.toString());
} catch (err) {
    // { code: 4001, message: 'User rejected the request.' }
}
```
