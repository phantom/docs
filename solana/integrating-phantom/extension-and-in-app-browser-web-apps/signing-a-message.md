# Signing a Message

When a web application is connected to Phantom, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within Phantom's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

In order to send a message for the user to sign, a web application must:&#x20;

1. Provide a **hex** or **UTF-8** encoded string as a Uint8Array.
2. Request that the encoded message is signed via the user's Phantom wallet.

The [`handleSignMessage` section of our developer sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L242) provides an example of signing a message.

{% hint style="info" %}
For more information on how to verify the signature of a message, please refer to [tweetnacl-js](https://github.com/dchest/tweetnacl-js/blob/master/README.md#naclsigndetachedverifymessage-signature-publickey).
{% endhint %}

{% tabs %}
{% tab title="signMessage()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request()" %}
<pre class="language-javascript"><code class="lang-javascript">const provider = getProvider(); // see "Detecting the Provider"
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
<strong>const signedMessage = await provider.request({
</strong>    method: "signMessage",
    params: {
         message: encodedMessage,
         display: "hex",
    },
});
</code></pre>
{% endtab %}
{% endtabs %}

## Support for "Sign In With" Standards

Applications that rely on `signMessage` for authenticating users can choose to opt-in to one of the various Sign In With (SIW) standards. If a message follows one of the supported standards, Phantom will verify required fields at the time of signing.&#x20;

At the time of this writing, Phantom supports:

* **Sign In With X** ([CAIP-122](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-122.md))
* **Sign In With Ethereum** ([EIP-4361](https://eips.ethereum.org/EIPS/eip-4361))
* **Sign In With Solana** ([EIP-4361](https://eips.ethereum.org/EIPS/eip-4361) with Solana `address` & `chain-id` grammar)

The serialized format of SIW messages is as follows:

```
${domain} wants you to sign in with your ${blockchain} account:
${address}

${statement}

URI: ${uri}
Version: ${version}
Chain ID: ${chain-id}
Nonce: ${nonce}
Issued At: ${issued-at}
Expiration Time: ${expiration-time}
Not Before: ${not-before}
Request ID: ${request-id}
Resources:
- ${resources[0]}
- ${resources[1]}
...
- ${resources[n]}
```

<table><thead><tr><th>Name</th><th>Type</th><th data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td><code>domain</code></td><td><code>string</code></td><td>true</td><td>The authority that is requesting the signing.</td></tr><tr><td><code>address</code></td><td><code>string</code></td><td>true</td><td>The blockchain address that is performing the signing.</td></tr><tr><td><code>statement</code></td><td><code>string</code></td><td>false</td><td>A human-readable ASCII assertion that the user will sign. It <em>MUST NOT</em> contain <code>\n</code>.</td></tr><tr><td><code>uri</code></td><td><code>string</code></td><td>true</td><td>A URI referring to the resource that is the subject of the signing (i.e. the subject of the claim).</td></tr><tr><td><code>version</code></td><td><code>string</code></td><td>true</td><td>The current version of the message.</td></tr><tr><td><code>chain-id</code></td><td><code>string</code></td><td>true</td><td>The Chain ID to which the session is bound, and the network where Contract Accounts MUST be resolved.</td></tr><tr><td><code>nonce</code></td><td><code>string</code></td><td>true</td><td>A randomized token to prevent signature replay attacks.</td></tr><tr><td><code>issued-at</code></td><td><code>string</code></td><td>true</td><td>The issuance time.</td></tr><tr><td><code>expiration-time</code></td><td><code>string</code></td><td>false</td><td>The time at which the signed authentication message is no longer valid.</td></tr><tr><td><code>not-before</code></td><td><code>string</code></td><td>false</td><td>The time at which the signed authentication message starts being valid.</td></tr><tr><td><code>request-id</code></td><td><code>string</code></td><td>false</td><td>A system-specific identifier used to uniquely refer to the authentication request.</td></tr><tr><td><code>resources</code></td><td><code>string[]</code></td><td>false</td><td>A list of uris the user wishes to have resolved as part of the authentication by the relying party.</td></tr></tbody></table>

### Sign In With X

The Sign In With X standard is defined by [CAIP-122](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-122.md). It uses [CAIP-10](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) identifiers for the `address` field and [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) for `chain-id`.

{% hint style="info" %}
While CAIP-122 is technically chain-agnostic, only Ethereum and Solana parsing are supported at this time.
{% endhint %}

#### Solana Example

{% tabs %}
{% tab title="signMessage()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Solana account:
solana:mainnet:FYpB58cLw5cwiN763ayB2sFT8HLF2MRUBbbyRgHYiRpK

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: solana:mainnet
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Solana account:
solana:mainnet:FYpB58cLw5cwiN763ayB2sFT8HLF2MRUBbbyRgHYiRpK

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: solana:mainnet
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params
         message: encodedMessage,
         display: "utf8",
    
});
```
{% endtab %}
{% endtabs %}

### Sign In With Solana

Sign In With Solana is an informal extension of [EIP-4361](https://eips.ethereum.org/EIPS/eip-4361) with Solana `address` and `chain-id` grammar.

#### Example

{% tabs %}
{% tab title="signMessage()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Solana account:
FYpB58cLw5cwiN763ayB2sFT8HLF2MRUBbbyRgHYiRpK

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: mainnet
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Solana account:
FYpB58cLw5cwiN763ayB2sFT8HLF2MRUBbbyRgHYiRpK

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: mainnet
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params
         message: encodedMessage,
         display: "utf8",
    
});
```
{% endtab %}
{% endtabs %}
