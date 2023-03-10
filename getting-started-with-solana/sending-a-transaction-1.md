# Sending a Versioned Transaction

On October 10, 2022, Solana introduced the concept of [Versioned Transactions](https://edge.docs.solana.com/developing/versioned-transactions). Currently, the Solana runtime supports two types of transactions: `legacy` (see [Sending a Legacy Transaction](sending-a-transaction.md)) and `v0` (transactions that can include [Address Lookup Tables (LUTS)](https://edge.docs.solana.com/developing/versioned-transactions)).

The goal of `v0` is to increase the maximum size of a transaction, and hence the number of accounts that can fit in a single atomic transaction. With LUTs, developers can now build transactions with a maximum of 256 accounts, as compared to the limit of 35 accounts in legacy transactions that do not utilize LUTs.

{% hint style="info" %}
For a dive deep on Versioned Transactions, LUTs, and how the above changes affect the anatomy of a transaction, you can read [this detailed guide](https://anvit.hashnode.dev/versioned-transactions).
{% endhint %}

On this page, we'll go over the following:

1. Building a Versioned Transaction
2. Signing and Sending a Versioned Transaction
3. Building an Address Lookup Table (LUT)
4. Extending an Address Lookup Table (LUT)
5. Signing and Sending a Versioned Transaction utilising a LUT

## Building a Versioned Transaction

Versioned transactions are built in a very similar fashion to [legacy transactions](sending-a-transaction.md). The only difference is that developers should use the `VersionedTransaction` class rather than the `Transaction` class.

The following example show how to build a simple transfer instruction. Once the transfer instruction is made, a `MessageV0` formatted transaction message is constructed with the transfer instruction. Finally, a new `VersionedTransaction` is created, parsing in the `v0` compatible message.

{% tabs %}
{% tab title="createTransactionV0()" %}
```typescript
// create array of instructions
const instructions = [
  SystemProgram.transfer({
    fromPubkey: publicKey,
    toPubkey: publicKey,
    lamports: 10,
  }),
];

// create v0 compatible message
const messageV0 = new TransactionMessage({
  payerKey: publicKey,
  recentBlockhash: blockhash,
  instructions,
}).compileToV0Message();

// make a versioned transaction
const transactionV0 = new VersionedTransaction(messageV0);
```
{% endtab %}
{% endtabs %}

Check out the [`createTransferTransactionV0()`](https://github.com/phantom-labs/sandbox/blob/main/src/utils/createTransferTransactionV0.ts) function in our code sandbox for a live example of creating a versioned transaction.

## Signing and Sending a Versioned Transaction

Once a Versioned transaction is created, it can be signed and sent via Phantom using the `signAndSendTransaction` method on the provider. The call will return a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise) for an object containing the `signature`. This is the same way a legacy transaction is sent via the Phantom provider.

{% tabs %}
{% tab title="signAndSendTransaction()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const network = "<NETWORK_URL>";
const connection = new Connection(network);
const versionedTransaction = new VersionedTransaction();
const { signature } = await provider.signAndSendTransaction(versionedTransaction);
await connection.getSignatureStatus(signature);
```
{% endtab %}
{% endtabs %}

You can also specify a `SendOptions` [object](https://solana-labs.github.io/solana-web3.js/modules.html#SendOptions) as a second argument into `signAndSendTransaction()` or as an `options` parameter when using `request`.

For a live demo of signing and sending a Versioned transaction, please refer to the [`handleSignAndSendTransactionV0()`](https://github.com/phantom-labs/sandbox/blob/78dc35fe140140a961345a6af30a058e1e19a7aa/src/App.tsx#L191) section of our developer sandbox.

## Building an Address Lookup Table (LUT)

Address Lookup Tables (LUTs) can be used to load accounts into table-like data structures. These structures can then be referenced to significantly increase the number of accounts that can be loaded in a single transaction.&#x20;

This lookup method effectively "_compresses_" a 32-byte address into a 1-byte index value. This "_compression_" enables storing up to 256 address in a single lookup table for use inside any given transaction.

With the `@solana/web3.js` [`createLookupTable`](https://solana-labs.github.io/solana-web3.js/classes/AddressLookupTableProgram.html#createLookupTable) function, developers can construct the instruction needed to create a new lookup table, as well as determine its address. Once we have the lookup table instruction, we can construct a transaction, sign it, and send it to create a lookup table on-chain. Address lookup tables can be created with either a `v0` transaction or a `legacy` transaction. However, the Solana runtime can only retrieve and handle the additional addresses within a lookup table while using `v0` transactions.

Here's a code snippet that creates a Lookup Table.

{% tabs %}
{% tab title="createAddressLookupTable()" %}
<pre class="language-typescript"><code class="lang-typescript">// create an Address Lookup Table
<strong>const [lookupTableInst, lookupTableAddress] = AddressLookupTableProgram.createLookupTable({
</strong>  authority: publicKey,
  payer: publicKey,
  recentSlot: slot,
});

// To create the Address Lookup Table on chain:
// send the `lookupTableInst` instruction in a transaction
const lookupMessage = new TransactionMessage({
  payerKey: publicKey,
  recentBlockhash: blockhash,
  instructions: [lookupTableInst],
}).compileToV0Message();

const lookupTransaction = new VersionedTransaction(lookupMessage);
const lookupSignature = await signAndSendTransaction(provider, lookupTransaction);</code></pre>
{% endtab %}
{% endtabs %}

Please refer to the [`handleSignAndSendTransactionV0WithLookupTable()`](https://github.com/phantom-labs/sandbox/blob/78dc35fe140140a961345a6af30a058e1e19a7aa/src/App.tsx#L218) in our code sandbox for a live demo of creating a lookup table.

## Extending an Address Lookup Table (LUT)

Once an Address Lookup Table is created, it can then be extended (i.e. accounts can be appended to the table). Using the `@solana/web3.js` library, you can create a new `extend` instruction using the [`extendLookupTable`](https://solana-labs.github.io/solana-web3.js/classes/AddressLookupTableProgram.html#extendLookupTable) method. Once the extend instruction is created, it can be sent in a transaction.

{% tabs %}
{% tab title="extendAddressLookupTable()" %}
```typescript
// add addresses to the `lookupTableAddress` table via an `extend` instruction
const extendInstruction = AddressLookupTableProgram.extendLookupTable({
  payer: publicKey,
  authority: publicKey,
  lookupTable: lookupTableAddress,
  addresses: [
    publicKey,
    SystemProgram.programId,
    // more `publicKey` addresses can be listed here
  ],
});

// Send this `extendInstruction` in a transaction to the cluster
// to insert the listing of `addresses` into your lookup table with address `lookupTableAddress`
const extensionMessageV0 = new TransactionMessage({
  payerKey: publicKey,
  recentBlockhash: blockhash,
  instructions: [extendInstruction],
}).compileToV0Message();

const extensionTransactionV0 = new VersionedTransaction(extensionMessageV0);
const extensionSignature = await signAndSendTransaction(provider, extensionTransactionV0);
```
{% endtab %}
{% endtabs %}

Please refer to the [`handleSignAndSendTransactionV0WithLookupTable()`](https://github.com/phantom-labs/sandbox/blob/78dc35fe140140a961345a6af30a058e1e19a7aa/src/App.tsx#L218) in our code sandbox for a live demo of extending a lookup table.

## Signing and Sending a Versioned Transaction utilizing a LUT

Up until now, we have:

1. Learned how to create a `VersionedTransaction`
2. Created an Address Lookup Table
3. Extended the Address Lookup Table

At this point, we are now ready to sign and send a `VersionedTransaction` utilizing an Address Lookup Table.

First, we need to fetch the account of the created Address Lookup Table.

{% tabs %}
{% tab title="getAddressLookupTable()" %}
```typescript
// get the table from the cluster
const lookupTableAccount = await connection.getAddressLookupTable(lookupTableAddress).then((res) => res.value);
// `lookupTableAccount` will now be a `AddressLookupTableAccount` object
console.log('Table address from cluster:', lookupTableAccount.key.toBase58());
```
{% endtab %}
{% endtabs %}

We can also parse and read all the addresses currently stores in the fetched Address Lookup Table.

{% tabs %}
{% tab title="Parse and Read addresses" %}
```typescript
// Loop through and parse all the address stored in the table
for (let i = 0; i < lookupTableAccount.state.addresses.length; i++) {
  const address = lookupTableAccount.state.addresses[i];
  console.log(i, address.toBase58());
}
```
{% endtab %}
{% endtabs %}

We can now create the instructions array with an arbitrary transfer instruction, just the way we did while creating the `VersionedTransaction` earlier. This `VersionedTransaction` can then be sent using the `signAndSendTransaction()` provider function.

{% tabs %}
{% tab title="" %}
```typescript
// create an array with your desired `instructions`
// in this case, just a transfer instruction
const instructions = [
  SystemProgram.transfer({
    fromPubkey: publicKey,
    toPubkey: publicKey,
    lamports: minRent,
  }),
];

// create v0 compatible message
const messageV0 = new TransactionMessage({
  payerKey: publicKey,
  recentBlockhash: blockhash,
  instructions,
}).compileToV0Message([lookupTableAccount]);

// make a versioned transaction
const transactionV0 = new VersionedTransaction(messageV0);
const signature = await signAndSendTransaction(provider, transactionV0);
```
{% endtab %}
{% endtabs %}

Please refer to the [`handleSignAndSendTransactionV0WithLookupTable()`](https://github.com/phantom-labs/sandbox/blob/78dc35fe140140a961345a6af30a058e1e19a7aa/src/App.tsx#L218) in our code sandbox for a live demo of signing and sending a versioned transaction utilizing an Address Lookup Table.
