# Connect

In order to start interacting with Phantom, an app must first establish a connection. This connection request will prompt the user for permission to share their public key, indicating that they are willing to interact further.

Once a user connects to Phantom, Phantom will return a `session` param that should be used on all subsequent methods. For more information on sessions, please review [Handling Sessions](../handling-sessions.md).

### Base URL

```
https://phantom.app/ul/v1/connect
```

### Query String Parameters

* `app_url` **(required)**: A url used to fetch app metadata (i.e. title, icon) using the same properties found in [Displaying Your App](../../best-practices/displaying-your-app.md). URL-encoded.
* `dapp_encryption_public_key` **(required)**: A public key used for end-to-end encryption. This will be used to generate a shared secret. For more information on how Phantom handles shared secrets, please review [Encryption](../encryption.md).&#x20;
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon connection. Please review [Specifying Redirects](../specifying-redirects.md) for more details. URL-encoded.
* `cluster` (optional): The network that should be used for subsequent interactions. Can be either: `mainnet-beta`, `testnet`, or `devnet`. Defaults to `mainnet-beta`.

### Returns

#### :white\_check\_mark: Approve

* `phantom_encryption_public_key`: An encryption public key used by Phantom for the construction of a shared secret between the connecting app and Phantom, encoded in base58.
* `nonce`: A nonce used for encrypting the response, encoded in base58.
*   `data`: An encrypted JSON string. Refer to [Encryption](../encryption.md) to learn how apps can decrypt `data` using a shared secret. Encrypted bytes are encoded in base58.

    ```json
    // content of decrypted `data`-parameter
    {
      // base58 encoding of user public key
      "public_key": "BSFtCudCd4pR4LSFqWPjbtXPKSNVbGkc35gRNdnqjMCU",

      // session token for subsequent signatures and messages
      // dapps should send this with any other deeplinks after connect
      "session": "..."
    }
    ```

    * `public_key`: The public key of the user, represented as a base58-encoded string.
    * `session`: A string encoded in base58. This should be treated as opaque by the connecting app, as it only needs to be passed alongside other parameters. Sessions do not expire. For more information on sessions, please review [Handling Sessions](../handling-sessions.md).

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../getting-started-with-solana/errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```

### Example

Please refer to the [`connect` method implemented in our React Native demo application](https://github.com/phantom-labs/deep-link-demo-app/blob/20f19f2154e98699f0d5a6b28bc4bb3d5acbcefd/App.tsx#L175).
