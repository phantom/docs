# Encryption

Deeplinks are encrypted using symmetric key encryption generated from a [Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange). While deeplink sessions will be created in plaintext, an encrypted channel will be created to prevent session tokens from getting hijacked.

### Encryption & Decryption Workflow

Phantom deeplinks are encrypted with the following workflows:

#### Connect

1. <mark style="color:green;">\[dapp]:</mark> On the initial [`connect` deeplink](provider-methods/connect.md), dapps should include a `dapp_encryption_public_key` query parameter. It's recommended to create a new x25519 keypair for every session started with `connect`. In all methods, the public key for this keypair is referred to as `dapp_encryption_public_key`.
2. <mark style="color:purple;">\[phantom]:</mark> Upon handling a `connect` deeplink, Phantom will also generate a new x25519 keypair.
   * Phantom will return this public key as `phantom_encryption_public_key` in the `connect` response.
   * Phantom will create a secret key using Diffie-Hellman with `dapp_encryption_public_key` and the _**private key**_ associated with `phantom_encryption_public_key`.
   * Phantom will locally store a mapping of `dapp_encryption_public_key` to shared secrets for use with decryption in subsequent deeplinks.
3. <mark style="color:green;">\[dapp]:</mark> Upon receiving the `connect` response, the dapp should create a shared secret by using Diffie-Hellman with `phantom_encryption_public_key` and the _**private key**_ associated with `dapp_encryption_public_key`. This shared secret should then be used to decrypt the `data` field in the response. If done correctly, the user's public key will be available to share with the dapp inside the `data` JSON object.

#### Subsequent Deeplinks

1. <mark style="color:green;">\[dapp]:</mark> For any subsequent methods (such as [SignAndSendTransaction](provider-methods/signandsendtransaction.md) and [SignMessage](provider-methods/signmessage.md)), dapps should send a `dapp_encryption_public_key` (the public key side of the shared secret) used with Phantom along with an encrypted `payload` object.&#x20;
2. <mark style="color:purple;">\[phantom]:</mark> Upon approval, Phantom will encrypt the signed response as a JSON object with the encryption sent as a `data=` query param.
3. <mark style="color:green;">\[dapp]:</mark> Upon receiving the deeplink response, dapps should decrypt the object in the `data=` query param to view the signature.

### Encryption Resources

To learn more about encryption and decryption, please refer to the following libraries:

#### JavaScript

* [TweetNaCl.js](https://github.com/dchest/tweetnacl-js)

#### iOS

* [TweetNaCl SwiftWrap](https://github.com/bitmark-inc/tweetnacl-swiftwrap)

#### Android

* [Tink](https://github.com/google/tink)
* [TweetNaCl Java](https://github.com/InstantWebP2P/tweetnacl-java)
