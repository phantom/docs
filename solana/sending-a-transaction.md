# Sending a Legacy Transaction

Once a web application is connected to Phantom, it can prompt the user for permission to send transactions on their behalf.

In order to send a transaction, a web application must:

1. Create an unsigned transaction.
2. Have the transaction be signed and submitted to the network by the user's Phantom wallet.
3. Optionally await network confirmation using a Solana JSON RPC connection.

{% hint style="info" %}
For more information about the nature of Solana transactions, please review the [`solana-web3.js` docs](https://solana-labs.github.io/solana-web3.js/) as well as the [Solana Cookbook](https://solanacookbook.com/core-concepts/transactions.html#transactions).
{% endhint %}

For a sample Phantom transaction, check out our [developer sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L160).

## Signing and Sending a Transaction

Once a transaction is created, the web application may ask the user's Phantom wallet to sign and send the transaction. If accepted, Phantom will sign the transaction with the user's private key and submit it via a Solana JSON RPC connection. By far the **easiest** and most **recommended** way of doing this is by using the `signAndSendTransaction` method on the provider, but it is also possible to do with `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for an object containing the `signature`.

{% tabs %}
{% tab title="signAndSendTransaction()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const { signature } = await provider.signAndSendTransaction(transaction);
await connection.getSignatureStatus(signature);
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const { signature } = await provider.request({
    method: "signAndSendTransaction",
    params: {
         message: bs58.encode(transaction.serializeMessage()),
    },
});
await connection.getSignatureStatus(signature);
```
{% endtab %}
{% endtabs %}

You can also specify a `SendOptions` [object](https://solana-labs.github.io/solana-web3.js/modules.html#SendOptions) as a second argument into `signAndSendTransaction` or as an `options` parameter when using `request`.

For a live demo of `signAndSendTransaction`, please refer to the [`handleSignAndSendTransaction` section of our developer sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L160).

## Signing and Sending Multiple Transactions

It is also possible to sign and send multiple transactions at once. This is exposed through the `signAndSendAllTransactions` method on the provider. This method accepts an array of Solana transactions, and will optionally accept a [SendOptions](https://solana-labs.github.io/solana-web3.js/types/SendOptions.html) object as a second parameter. If successful, it will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for an object containing the array of string `signatures` and the `publicKey` of the signer.

{% tabs %}
{% tab title="signAndSendAllTransactions()" %}
```typescript
const provider = getProvider(); // see "Detecting the Provider"
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transactions = [new Transaction(), new Transaction()];
const { signatures, publicKey } = await provider.signAndSendAllTransactions(transactions);
await connection.getSignatureStatuses(signatures);
```
{% endtab %}
{% endtabs %}

## Other Signing Methods

The following methods are also supported, but are not recommended over `signAndSendTransaction`. It is safer for users, and a simpler API for developers, for Phantom to submit the transaction immediately after signing it instead of relying on the application to do so.

{% hint style="warning" %}
&#x20;If you use the methods below, Phantom may display a warning message to users at the time of signing.
{% endhint %}

### Signing a Transaction (Without Sending)

Once a transaction is created, a web application may ask the user's Phantom wallet to sign the transaction _without_ also submitting it to the network. The easiest and most recommended way of doing this is via the `signTransaction` method on the provider, but it is also possible to do via `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for the signed transaction. After the transaction has been signed, an application may submit the transaction itself via [web3js's `sendRawTransaction`](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#sendRawTransaction).

{% tabs %}
{% tab title="signTransaction()" %}
```javascript
const provider = getProvider();
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const signedTransaction = await provider.signTransaction(transaction);
const signature = await connection.sendRawTransaction(signedTransaction.serialize());
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider();
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const signedTransaction = await provider.request({
    method: "signTransaction",
    params: {
         message: bs58.encode(transaction.serializeMessage()),
    },
});
const signature = await connection.sendRawTransaction(signedTransaction.serialize());
```
{% endtab %}
{% endtabs %}

Please refer to the [`handleSignTransaction` section of our developer sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L187) for an example of `signTransaction`.

### Signing Multiple Transactions

For legacy integrations, Phantom supports signing multiple transactions at once without sending them. This is exposed through the `signAllTransactions` method on the provider. This method is **not recommended** for new integrations. Instead, developers should make use of `signAndSendAllTransactions`.

{% tabs %}
{% tab title="signAllTransactions()" %}
```javascript
const signedTransactions = await provider.signAllTransactions(transactions);
```
{% endtab %}

{% tab title="request()" %}
```javascript
const message = transactions.map(transaction => {
    return bs58.encode(transaction.serializeMessage());
});
const signedTransactions = await provider.request({
    method: "signAllTransactions",
    params: { message },
});
```
{% endtab %}
{% endtabs %}

For an example of `signAllTransactions`, please refer to the [`handleSignAllTransactions` section of our developer sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L213).
