# Sending a Transaction

Once the web application is connected to Phantom, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the web application must:

* Create an unsigned transaction or transactions.
* Have it be signed and submitted to the network by the user's Phantom wallet.
* Optionally await the confirmation using a Solana JSON RPC connection.

For more information about the nature of transactions on Solana, it is recommended to review the `solana-web3.js` [docs](https://solana-labs.github.io/solana-web3.js/) as well as the official [Solana docs](https://docs.solana.com/developing/programming-model/transactions).

For a sample transaction check out our developer sandbox.

{% content-ref url="../resources/sandbox.md" %}
[sandbox.md](../resources/sandbox.md)
{% endcontent-ref %}

## Signing and Sending a Transaction

Once a transaction is created, the web application may ask the user's Phantom wallet to sign and send the transaction using their account's private key and Solana JSON RPC connection. By far the **easiest **and **recommended **way of doing this is by using the `signAndSendTransaction` method on the provider, but it is also possible to do with `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `signature`.

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

## Deprecated Methods

The following methods are still supported, but are no longer recommended and may be removed in a future version of Phantom. It is safer for users, and a simpler API for developers, for Phantom to submit the transaction immediately after signing it instead of relying on the application to do so. For now, if you use the methods below, Phantom will display a warning message to users.

### Signing a Transaction

Once a transaction is created, the web application may ask the user's Phantom wallet to sign the transaction using their account's private key. By far the **easiest **and **recommended **way of doing this is by using the `signTransaction` method on the provider, but it is also possible to do with `request`. In both cases, the call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for the signed transaction.

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
