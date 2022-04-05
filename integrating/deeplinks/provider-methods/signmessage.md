# SignMessage

Once it's connected to Phantom, an app can request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within Phantom's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

In order to send a message for the user to sign, an application must:&#x20;

1. Provide a **hex** or **UTF-8** encoded string as a Uint8Array and then **base58-encoded it**.
2. Request that the encoded message is signed via the user's Phantom wallet.

The [deeplinking demo app](../../../resources/sandbox.md#deeplinking-demo-app) provides an example of signing a message.

{% hint style="info" %}
For more information on how to verify the signature of a message, please refer to [Encryption Resources](../encryption.md#encryption-resources).
{% endhint %}

### Base URL

```
https://phantom.app/ul/v1/signMessage
```

### Query String Parameters

* `dapp_encryption_public_key` **(required)**: The original encryption public key used from the app side for an existing [Connect](connect.md) session.
* `nonce` **(required)**: A nonce used for encrypting the request, encoded in base58.
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon completion. Please review [Specifying Redirects](../specifying-redirects.md) for more details.
*   `payload` **(required)**: An encrypted JSON string with the following fields:

    ```json
    {
      "message": "...", // the message, base58 encoded
      "session": "...", // token received from connect-method
      "display": "utf8" | "hex", // the encoding to use when displaying the message 
    }
    ```

    * `message` **(required)**: The message that should be signed by the user, encoded in base58. Phantom will display this message to the user when they are prompted to sign.
    * `session` **(required)**: The session token received from the [Connect](connect.md) method. Please see [Handling Sessions](../handling-sessions.md) for more details.
    * `display` **(optional)**: How you want us to display the string to the user. Defaults to `utf8`

### Returns

#### :white\_check\_mark: Approve

* `nonce`: A nonce used for encrypting the response, encoded in base58.
*   `data`: An encrypted JSON string. Refer to [Encryption](../encryption.md) to learn how apps can decrypt `data` using a shared secret. Encrypted bytes are encoded in base58.

    ```json
    // content of decrypted `data`-parameter
    {
        signature: "...", // message-signature
    }
    ```

    * `signature`: The message signature, encoded in base58. For more information on how to verify the signature of a message, please refer to [Encryption Resources](../encryption.md#encryption-resources).

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```
