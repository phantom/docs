# Sending a Transaction

Once the web application is connected to Phantom, it can send transactions on behalf of the user, with the user's permission.

In order to send a transaction, the web application must:

* Create an unsigned transaction or transactions.
* Have it be signed by the user's Phantom wallet.
* Send it to a Solana node for processing.

For more information about the nature of transactions on Solana, it is recommended to review the `solana-web3.js` [docs](https://solana-labs.github.io/solana-web3.js/class/src/transaction.js~Transaction.html) as well as the official [Solana docs](https://docs.solana.com/developing/programming-model/transactions).

For a sample transaction check out our developer sandbox.

{% page-ref page="../sandbox.md" %}

## Signing a Transaction

Once a transaction is created, the web application may ask the user's Phantom wallet to sign the transaction using their account's private key. By far the **easiest** and **recommended** way of doing this is by using the `signTransaction` method on the provider, but it is also possible to do with `request`. In both cases, the call with return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for the signed transaction.

{% tabs %}
{% tab title="signTransaction\(\)" %}
```javascript
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const transaction = new Transaction();
const signedTransaction = await window.solana.signTransaction(transaction);
const signature = await connection.sendRawTransaction(signedTransaction.serialize());
```
{% endtab %}

{% tab title="request\(\)" %}
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

## Signing Multiple Transactions

It is also possible to sign and send multiple transactions at once. This is exposed through the `signAllTransactions` method on the provider.

{% tabs %}
{% tab title="signAllTransactions\(\)" %}
```javascript
const signedTransactions = await window.solana.signAllTransactions(transactions);
```
{% endtab %}

{% tab title="request\(\)" %}
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

 

