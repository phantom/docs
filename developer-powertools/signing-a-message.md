# Sign-In-With (SIW) Standards

Applications that rely on `signMessage` for authenticating users can choose to opt-in to one of the various Sign In With (SIW) standards. If a message follows one of the supported standards, Phantom will verify required fields at the time of signing.&#x20;

At the time of this writing, Phantom supports:

* **Sign In With Solana** ([Specification](https://github.com/phantom/sign-in-with-solana))
* **Sign In With Ethereum** ([EIP-4361](https://eips.ethereum.org/EIPS/eip-4361))
* **Sign In With X** ([CAIP-122](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-122.md))

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

<table><thead><tr><th width="153">Name</th><th width="119">Type</th><th width="114" data-type="checkbox">Required?</th><th>Description</th></tr></thead><tbody><tr><td><code>domain</code></td><td><code>string</code></td><td>true</td><td>The authority that is requesting the signing.</td></tr><tr><td><code>address</code></td><td><code>string</code></td><td>true</td><td>The blockchain address that is performing the signing.</td></tr><tr><td><code>statement</code></td><td><code>string</code></td><td>false</td><td>A human-readable ASCII assertion that the user will sign. It <em>MUST NOT</em> contain <code>\n</code>.</td></tr><tr><td><code>uri</code></td><td><code>string</code></td><td>true</td><td>A URI referring to the resource that is the subject of the signing (i.e. the subject of the claim).</td></tr><tr><td><code>version</code></td><td><code>string</code></td><td>true</td><td>The current version of the message.</td></tr><tr><td><code>chain-id</code></td><td><code>string</code></td><td>true</td><td>The Chain ID to which the session is bound, and the network where Contract Accounts MUST be resolved.</td></tr><tr><td><code>nonce</code></td><td><code>string</code></td><td>true</td><td>A randomized token to prevent signature replay attacks.</td></tr><tr><td><code>issued-at</code></td><td><code>string</code></td><td>true</td><td>The issuance time.</td></tr><tr><td><code>expiration-time</code></td><td><code>string</code></td><td>false</td><td>The time at which the signed authentication message is no longer valid.</td></tr><tr><td><code>not-before</code></td><td><code>string</code></td><td>false</td><td>The time at which the signed authentication message starts being valid.</td></tr><tr><td><code>request-id</code></td><td><code>string</code></td><td>false</td><td>A system-specific identifier used to uniquely refer to the authentication request.</td></tr><tr><td><code>resources</code></td><td><code>string[]</code></td><td>false</td><td>A list of uris the user wishes to have resolved as part of the authentication by the relying party.</td></tr></tbody></table>

### Sign In With Solana

Please refer to our [specification and integration guide on GitHub](https://github.com/phantom/sign-in-with-solana).

### Sign In With Ethereum

The Sign In With Ethereum standard is defined by [EIP-4361](https://eips.ethereum.org/EIPS/eip-4361).

#### Example

{% tabs %}
{% tab title="signMessage()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Ethereum account:
0xb9c5714089478a327f09197987f16f9e5d936e8a

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: 1
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
const message = `magiceden.io wants you to sign in with your Ethereum account:
0xb9c5714089478a327f09197987f16f9e5d936e8a

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: 1
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

### Sign In With X

The Sign In With X standard is defined by [CAIP-122](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-122.md). It uses [CAIP-10](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) identifiers for the `address` field and [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) for `chain-id`.

{% hint style="info" %}
While CAIP-122 is technically chain-agnostic, only Ethereum and Solana parsing are supported at this time.
{% endhint %}

#### Ethereum Example

{% tabs %}
{% tab title="signMessage()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Ethereum account:
eip155:1:0xb9c5714089478a327f09197987f16f9e5d936e8a

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: eip155:1
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");java
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider(); // see "Detecting the Provider"
const message = `magiceden.io wants you to sign in with your Ethereum account:
eip155:1:0xb9c5714089478a327f09197987f16f9e5d936e8a

Click Sign or Approve only means you have proved this wallet is owned by you.

URI: https://magiceden.io
Version: 1
Chain ID: eip155:1
Nonce: bZQJ0SL6gJ
Issued At: 2022-10-25T16:52:02.748Z
Resources:
- https://foo.com
- https://bar.com`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params: {
         message: encodedMessage,
         display: "utf8",
    
});
```
{% endtab %}
{% endtabs %}

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

