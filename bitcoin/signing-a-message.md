# Signing a Message

When a dApp is connected to Phantom, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within Phantom's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

<figure><img src="../.gitbook/assets/image (18).png" alt="" width="375"><figcaption><p>An example of signing "hello world" with the Phantom Bitcoin provider</p></figcaption></figure>

Phantom’s `signMessage` function accepts two parameters: an encoded `message` and an `address` that should be used to sign the message. It returns a Promise that resolves if the user approves the signature request. Once resolved, it contains the resulting `signature`.

```typescript
// function signature
function signMessage(
    address: string,
    message: Uint8Array,
  ): Promise<{
    signature: Uint8Array;
  }>;
```

The following is an example of how to construct and sign a message with Phantom:

```typescript
const message = new TextEncoder().encode('hello world');
const address = "bc1phajtersv55fwud5xr0t70p5234swy396a6avqhuny0qf83zssvrsm7tl4q"; // see "Establishing a Connection"
const { signature } = await phantomProvider.signMessage(address, message);
```

To verify that a message was signed by a given address, we recommend using the [bip322-js](https://github.com/ACken2/bip322-js) library like so:

```typescript
import { Verifier } from "bip322-js";

// helper method
function bytesToBase64(bytes) {
  const binString = String.fromCodePoint(...bytes);
  return btoa(binString);
}

const isValidSignature = Verifier.verifySignature(
  address,
  new TextDecoder().decode(message),
  bytesToBase64(signature),
);
console.log(isValidSignature);
// true
```
