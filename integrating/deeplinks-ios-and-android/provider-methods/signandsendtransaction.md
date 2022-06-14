# SignAndSendTransaction

Once an application is connected to Phantom, it can prompt the user for permission to send transactions on their behalf.

In order to send a transaction, an application must:

1. Create an unsigned transaction.
2. Have the transaction be signed and submitted to the network by the user's Phantom wallet.
3. Optionally await network confirmation using a Solana JSON RPC connection.

{% hint style="info" %}
For more information about the nature of Solana transactions, please review the [`solana-web3.js` docs](https://solana-labs.github.io/solana-web3.js/) as well as the [Solana Cookbook](https://solanacookbook.com/core-concepts/transactions.html#transactions).
{% endhint %}

For a sample transaction using Phantom deeplinks, check out our [deeplinking demo app](../../../resources/sandbox.md#deeplinking-demo-app).

### Base URL

```
https://phantom.app/ul/v1/signAndSendTransaction
```

### Query String Parameters

* `dapp_encryption_public_key` **(required)**: The original encryption public key used from the app side for an existing [Connect](connect.md) session.
* `nonce` **(required)**: A nonce used for encrypting the request, encoded in base58.
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon completion. Please review [Specifying Redirects](../specifying-redirects.md) for more details. URL-encoded.
*   `payload` **(required)**: An encrypted JSON string with the following fields:

    ```json
    {
      "transaction": "...", // serialized transaction, base58-encoded
      "session": "...", // token received from the connect method
    }
    ```

    * `transaction` **(required)**: The [transaction](https://solana-labs.github.io/solana-web3.js/classes/Transaction.html) that Phantom will sign and submit, serialized and encoded in base58.
    * `session` **(required)**: The session token received from the [Connect](connect.md) method. Please see [Handling Sessions](../handling-sessions.md) for more details.

### Returns

#### :white\_check\_mark: Approve

* `nonce`: A nonce used for encrypting the response, encoded in base58.
*   `data`: An encrypted JSON string. Refer to [Encryption](../encryption.md) to learn how apps can decrypt `data` using a shared secret. Encrypted bytes are encoded in base58.

    ```json
    // content of decrypted `data`-parameter
    {
      "signature": "..." // transaction-signature
    }
    ```

    * `signature`: The first signature in the transaction, which can be used as its [transaction id](https://docs.solana.com/terminology#transaction-id).

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```

### Example

Please refer to the [`signAndSendTransaction` method implemented in our React Native demo application](https://github.com/phantom-labs/deep-link-demo-app/blob/20f19f2154e98699f0d5a6b28bc4bb3d5acbcefd/App.tsx#L204).
