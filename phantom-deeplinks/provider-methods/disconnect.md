# Disconnect

After an initial [Connect](connect.md) event has taken place, an app may disconnect from Phantom at anytime. Once disconnected, Phantom will reject all signature requests until another connection is established.

### Base URL

```
https://phantom.app/ul/v1/disconnect
```

### Query String Parameters

* `dapp_encryption_public_key` **(required)**: The original encryption public key used from the app side for an existing [Connect](connect.md) session.
* `nonce` **(required)**: A nonce used for encrypting the request, encoded in base58.
* `redirect_link` **(required)**: The URI where Phantom should redirect the user upon completion. Please review [Specifying Redirects](../specifying-redirects.md) for more details. URL-encoded.
*   `payload` **(required)**: An encrypted JSON string with the following fields:

    ```json
    {
        "session": "...", // token received from the connect method
    }
    ```

    * `session` **(required)**: The session token received from the [Connect](connect.md) method. Please see [Handling Sessions](../handling-sessions.md) for more details.

### Returns

#### :white\_check\_mark: Approve

No query params returned.

#### :x:Reject

An `errorCode` and `errorMessage` as query parameters. Please refer to [Errors](../../getting-started-with-solana/errors.md) for a full list of possible error codes.

```
{
  "errorCode": "...",
  "errorMessage": "..."
}
```

### Example

Please refer to the [`disconnect` method implemented in our React Native demo application](https://github.com/phantom-labs/deep-link-demo-app/blob/20f19f2154e98699f0d5a6b28bc4bb3d5acbcefd/App.tsx#L187).
