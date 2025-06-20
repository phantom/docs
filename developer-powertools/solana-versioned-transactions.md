# Solana Versioned Transactions

### Introduction

Phantom supports [Versioned Transactions](https://docs.solana.com/developing/versioned-transactions) as well as [Legacy Transactions](../solana/sending-a-transaction.md). Versioned Transactions and Address Lookup Tables (LUTs) were introduced by Solana in an effort to improve the developer and end-user experience. The proposed changes are as follows:

1. Introduce a program which manages on-chain address lookup tables
2. Add a new transaction format which can make use of the above on-chain address lookup tables

Why do we need the above?

Legacy transactions have a major issue: Maximum allowed size of 1232 bytes, and hence the number of accounts that can fit in an atomic transaction: 35 addresses.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

This is problematic as developers are limited with this threshold and are unable to include >35 signature-free account / program addresses in a single transaction. This is where Address Lookup Tables (LUT) come into the picture.



### Address Lookup Tables (LUTs)

The idea behind Address Lookup Tables is to store account addresses in array-like data structures on-chain. Once accounts are stored in this table, the address of the table can be referenced in a transaction message using 1-byte u8 indices.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

This opens up space as addresses need not be stored inside transaction messages, but only be referenced. This allows 2^8=256 accounts to be included in 1 tx, as accounts are referenced using u8 indices. Here's an example of its power:

{% embed url="https://twitter.com/PierreArowana/status/1545186073034334208?s=20" %}

### RPC Changes <a href="#heading-rpc-changes" id="heading-rpc-changes"></a>

Transaction responses will require a new version field: `maxSupportedTransactionVersion` to indicate to clients which transaction structure needs to be followed for deserialisation.

The following methods need to be updated to avoid errors:

* `getTransaction`
* `getBlock`

The following parameter needs to be added to the requests:

`maxSupportedTransactionVersion: 0`

If `maxSupportedTransactionVersion` is not explicitly added to the request, the transaction version will fallback to `legacy`. Any block that contains a versioned transaction will return with an error by the client in the case of a legacy transaction.

{% embed url="https://twitter.com/jacobvcreech/status/1551673297972101120?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1551673302283960324%7Ctwgr%5E4eec7dc9ba28cb9513757c79cddba04e61854a74%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fanvit.hashnode.dev%2Fversioned-transactions" %}

You can set this via JSON formatted requests to the RPC endpoint like below:

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d \
'{"jsonrpc": "2.0", "id":1, "method": "getBlock", "params": [430, {
  "encoding":"json",
  "maxSupportedTransactionVersion":0,
  "transactionDetails":"full",
  "rewards":false
}]}'

```

You can also do the same using the [`@solana/web3.js`](https://solana-labs.github.io/solana-web3.js/) library.

```javascript
// connect to the `devnet` cluster and get the current `slot`
const connection = new web3.Connection(web3.clusterApiUrl("devnet"));
const slot = await connection.getSlot();

// get the latest block (allowing for v0 transactions)
const block = await connection.getBlock(slot, {
  maxSupportedTransactionVersion: 0,
});

// get a specific transaction (allowing for v0 transactions)
const getTx = await connection.getTransaction(
  "3jpoANiFeVGisWRY5UP648xRXs3iQasCHABPWRWnoEjeA93nc79WrnGgpgazjq4K9m8g2NJoyKoWBV1Kx5VmtwHQ",
  {
    maxSupportedTransactionVersion: 0,
  },
);
```



Check out how to build and send Versioned Transactions and Lookup Tables [on this page](../solana/sending-a-transaction-1.md).



### Further Reading

{% embed url="https://beta.docs.solana.com/developing/versioned-transactions#how-create-a-versioned-transaction" %}

{% embed url="https://beta.docs.solana.com/developing/lookup-tables#how-to-create-an-address-lookup-table" %}

{% embed url="https://anvit.hashnode.dev/versioned-transactions" %}
