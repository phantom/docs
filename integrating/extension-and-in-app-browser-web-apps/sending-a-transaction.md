# Sending a Transaction

Once a web application is connected to Phantom, it can prompt the user for permission to send transactions on their behalf.

In order to send a transaction, a web application must:

1. Create an unsigned transaction.
2. Have the transaction be signed and submitted to the network by the user's Phantom wallet.
3. Optionally await network confirmation using a Solana JSON RPC connection.

{% hint style="info" %}
For more information about the nature of Solana transactions, please review the [`solana-web3.js` docs](https://solana-labs.github.io/solana-web3.js/) as well as the [Solana Cookbook](https://solanacookbook.com/core-concepts/transactions.html#transactions).
{% endhint %}

For a sample Phantom transaction, check out our [developer sandbox](../../resources/sandbox.md#sandbox).

## Signing and Sending a Transaction

Once a transaction is created, the web application may ask the user's Phantom wallet to sign and send the transaction. If accepted, Phantom will sign the transaction with the user's private key and submit it via a Solana JSON RPC connection. By far the **easiest** and most **recommended** way of doing this is by using the `signAndSendTransaction` method on the provider, but it is also possible to do with `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `signature`.

{% tabs %}
{% tab title="signAndSendTransaction()" %}
```javascript
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const { signature } = await window.solana.signAndSendTransaction(transaction);
await connection.confirmTransaction(signature);
```
{% endtab %}

{% tab title="request()" %}
```javascript
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const { signature } = await window.solana.request({
    method: "signAndSendTransaction",
    params: {
         message: bs58.encode(transaction.serializeMessage()),
    },
});
await connection.confirmTransaction(signature);
```
{% endtab %}
{% endtabs %}

You can also specify a `SendOptions` [object](https://solana-labs.github.io/solana-web3.js/modules.html#SendOptions) as a second argument into `signAndSendTransaction` or as an `options` parameter when using `request`.

## Other Signing Methods

The following methods are also supported, but are not recommended over `signAndSendTransaction`. It is safer for users, and a simpler API for developers, for Phantom to submit the transaction immediately after signing it instead of relying on the application to do so. If you use the methods below, Phantom will display a warning message to users.

### Signing a Transaction (Without Sending)

Once a transaction is created, a web application may ask the user's Phantom wallet to sign the transaction _without_ also submitting it to the network. The easiest and most recommended way of doing this is via the `signTransaction` method on the provider, but it is also possible to do via `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for the signed transaction. After the transaction has been signed, an application may submit the transaction itself via [web3js's `sendRawTransaction`](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#sendRawTransaction).

{% tabs %}
{% tab title="signTransaction()" %}
```javascript
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const signedTransaction = await window.solana.signTransaction(transaction);
const signature = await connection.sendRawTransaction(signedTransaction.serialize());
```
{% endtab %}

{% tab title="request()" %}
```javascript
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const signedTransaction = await window.solana.request({
    method: "signTransaction",
    params: {
         message: bs58.encode(transaction.serializeMessage()),
    },
});
const signature = await connection.sendRawTransaction(signedTransaction.serialize());
```
{% endtab %}
{% endtabs %}

### Signing Multiple Transactions

It is also possible to sign and send multiple transactions at once. This is exposed through the `signAllTransactions` method on the provider.

{% tabs %}
{% tab title="signAllTransactions()" %}
```javascript
const signedTransactions = await window.solana.signAllTransactions(transactions);
```
{% endtab %}

{% tab title="request()" %}
```javascript
const message = transactions.map(transaction => {
    return bs58.encode(transaction.serializeMessage());
});
const signedTransactions = await window.solana.request({
    method: "signAllTransactions",
    params: { message },
});
```
{% endtab %}
{% endtabs %}

&#x20;
