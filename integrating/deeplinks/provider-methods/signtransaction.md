# SignTransaction

The **easiest** and **most recommended** way to send a transaction is via [SignAndSendTransaction](signandsendtransaction.md). It is safer for users, and a simpler API for developers, for Phantom to submit the transaction immediately after signing it instead of relying on the application to do so.

However, it is also possible for an app to request just the signature from Phantom. Once signed, an app can submit the transaction itself using [web3.js's `sendRawTransaction`](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#sendRawTransaction).&#x20;

### Base URL

```
https://phantom.app/ul/v1/signTransaction
```

### Query String Parameters

* `dapp_encryption_public_key` **(required)**: The original encryption public key used from the app side for an existing [Connect](connect.md) session.
* `nonce` **(required)**: A nonce used for encrypting the request, encoded in base58.
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon completion. Please review [Specifying Redirects](../specifying-redirects.md) for more details.
*   `payload` **(required)**: An encrypted JSON string with the following fields:

    ```json
    {
      "transactions": "...", // serialized transaction, base58 encoded
      "session": "...", // token received from connect-method
    }
    ```

    * `transaction` **(required)**: The [transaction](https://solana-labs.github.io/solana-web3.js/classes/Transaction.html) that Phantom will sign, serialized and encoded in base58.
    * `session` **(required)**: The session token received from the [Connect](connect.md) method. Please see [Handling Sessions](../handling-sessions.md) for more details.

### Returns

#### :white\_check\_mark: Approve

* `nonce`: A nonce used for encrypting the response, encoded in base58.
*   `data`: An encrypted JSON string. Refer to [Encryption](../encryption.md) to learn how apps can decrypt `data` using a shared secret. Encrypted bytes are encoded in base58.

    ```json
    // content of decrypted `data`-parameter
    {
        transactions: "...", // signed serialized transaction, base58 encoded
    }
    ```

    * `transaction`: The signed, serialized transaction that is base58 encoded. Phantom will not submit this transactions. An application can submit this transactions itself via [web3.js's sendRawTransaction](https://solana-labs.github.io/solana-web3.js/classes/Connection.html#sendRawTransaction).

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```
