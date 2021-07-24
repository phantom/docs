# Signing a Message

When the web application is connected to Phantom, it can also request that the user signs a given message. 

In order to send a message for the user to sign, the web application must: 

* Provide a hex or UTF-8 encoded string as a Uint8Array.
* Have it be signed by the user's Phantom Wallet.

The developer sandbox provides an example of signing a message.

Refer to [tweetnacl-js](https://github.com/dchest/tweetnacl-js/blob/master/README.md#naclsigndetachedverifymessage-signature-publickey) for verifying the signature of a message.

{% page-ref page="../resources/sandbox.md" %}

{% tabs %}
{% tab title="signMessage\(\)" %}
```javascript
const message = `To avoid digital dognappers,
    sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await window.solana.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request\(\)" %}
```javascript
const message = `To avoid digital dognappers,
    sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await window.solana.request({
    method: "signMessage",
    params: {
         message: encodedMessage,
         display: "hex",
    },
});
```
{% endtab %}
{% endtabs %}

