# Handling Sessions

When a user [connects to Phantom](provider-methods/connect.md) for the first time, Phantom will return a `session` param that represents the user's connection. The app should pass this `session` param back to Phantom on all subsequent [Provider Methods](provider-methods/). It is the app's responsibility to store this `session`.

Sessions do not expire. Once a user has connected with Phantom, the corresponding app can indefinitely make requests such as [SignAndSendTransaction](provider-methods/signandsendtransaction.md) and [SignMessage](provider-methods/signmessage.md) without prompting the user to re-connect with Phantom. Apps will still need to re-connect to Phantom after a [Disconnect](provider-methods/disconnect.md) event or an [Invalid Session](handling-sessions.md#invalid-sessions).

### Session Structure

The entire `session` param is encoded in base58. A `session` should contain the following data:

* **JSON Data Signature**: A base58 signature of the JSON data that is 64 bytes. Phantom will check the signature against the actual message that was signed.
* **JSON Data**: A JSON object with the following fields:
  * `app_url` (string): A url used to fetch app metadata (i.e. title, icon) using the same properties found in [Displaying Your App](../best-practices/displaying-your-app.md).
  * `timestamp` (number): The timestamp at which the user approved the connection. At the time of this writing, sessions do not expire.
  * `chain` (string): The chain that the user connected to at the start of the session. Sessions cannot be used across two different chains with the same keypair (e.g. the user cannot connect to Solana and then sign on Ethereum). At the time of this writing, Phantom only supports `solana`.
  * `cluster` (string) (optional): The approved cluster that the app and user initially connected to. Solana-only. Can be either: `mainnet-beta`, `testnet`, or `devnet`. Defaults to `mainnet-beta`.

### Decoding Sessions

Phantom will decode and validate the `session` param on every request. To decode the session, we decode it with `bs58`, slice off the first 64 bytes of the signature, and the treat the rest as JSON data. We then sign the JSON data again with the same keypair and compare that signature against the signature in the session. If the signatures are the same, the session is valid. Otherwise, we conclude that the session has been faked, as the signature does not belong to the keypair it claims it does.

{% hint style="info" %}
Calling `nacl.sign.open` conveniently verifies and returns the original object. For more information, please review [Encryption Resources](encryption.md#encryption-resources).
{% endhint %}

After we determine that the session is valid, we still need to ensure that the JSON fields line up with what we expect. An app could give a session for `pubkey A` when the user is currently using `pubkey B` in Phantom. In such a scenario, that session should not allow an app to request signatures. Instead, the app must issue a new connect request or use the correct session.

```javascript
// Encoding a session
const privateKey = ...;
const sessionData = JSON.stringify({
  "app_id": "APP_ID",
  "chain": "CHAIN",
  "cluster": "CLUSTER",
  "timestamp": 1644954984,
});
const bytes = Buffer.from(sessionData, "utf-8");

// tweetnacl-js formats signature in format <signature><sessionData>
const signature = bs58.encode(nacl.sign(bytes, privateKey));

// Decoding ja session
const publicKey = ...;
const verifiedSessionData = nacl.sign.open(bs58.decode(signature), publicKey.toBytes());
if (!verifiedSessionData) throw new Error(`This session was not signed by ${publicKey}`);
```

### Invalid Sessions

While sessions do not expire, there are a number of reasons why a sessions could still be deemed invalid:

1. It was not signed by the current wallet keypair. This could mean that the session is entirely fake, or that it was signed by another keypair in the user’s wallet.
2.  It was signed by the current wallet keypair, but the session's JSON `data` does not pass muster. There are a few reasons why this might occur:

    1. The user switched chains (or possibly networks).
    2. The `app_url` could be blocked if malicious. See [Blocklist](../developer-powertools/blocklist.md) for more information.

