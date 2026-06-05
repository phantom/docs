# SignAllTransactions

Once an app is connected, it is also possible to sign multiple transactions at once. Unlike [SignAndSendTransaction](signandsendtransaction.md), Phantom will not submit these transactions to the network. Applications can submit signed transactions using [web3.js's `sendRawTransaction`](https://solana-labs.github.io/solana-web3.js/v1.x/classes/Connection.html#sendRawTransaction).&#x20;

### Base URL

```
https://phantom.app/ul/v1/signAllTransactions
```

### Query String Parameters

* `dapp_encryption_public_key` **(required)**: The original encryption public key used from the app side for an existing [Connect](connect.md) session.
* `nonce` **(required)**: A nonce used for encrypting the request, encoded in base58.
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon completion. Please review [Specifying Redirects](../specifying-redirects.md) for more details. URL-encoded.
*   `payload` **(required)**: An encrypted JSON string with the following fields:

    ```json
    {
      "transactions": [
        "...", // serialized transaction, bs58-encoded
        "...", // serialized transaction, bs58-encoded
      ],
      "session": "...", // token received from connect-method
    }
    ```

    * `transactions` **(required)**: An array of [transactions](https://solana-labs.github.io/solana-web3.js/v1.x/classes/Transaction.html) that Phantom will sign, serialized and encoded in base58.
    * `session` **(required)**: The session token received from the [Connect](connect.md) method. Please see [Handling Sessions](../handling-sessions.md) for more details.

### Returns

#### :white\_check\_mark: Approve

* `nonce`: A nonce used for encrypting the response, encoded in base58.
*   `data`: An encrypted JSON string. Refer to [Encryption](../encryption.md) to learn how apps can decrypt `data` using a shared secret. Encrypted bytes are encoded in base58.

    ```json
    // content of decrypted `data`-parameter
    {
        transactions: [
            "...", // signed serialized transaction, bs58-encoded
            "...", // signed serialized transaction, bs58-encoded
        ] 
    }
    ```

    * `transactions`: An array of signed, serialized transactions that are base58 encoded. Phantom will not submit these transactions. Applications can submit these transactions themselves via [web3.js's `sendRawTransaction`](https://solana-labs.github.io/solana-web3.js/v1.x/classes/Connection.html#sendRawTransaction).&#x20;

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../getting-started-with-solana/errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```

### Example

Please refer to the [`signAllTransactions` method implemented in our React Native demo application](https://github.com/phantom-labs/deep-link-demo-app/blob/20f19f2154e98699f0d5a6b28bc4bb3d5acbcefd/App.tsx#L229).
