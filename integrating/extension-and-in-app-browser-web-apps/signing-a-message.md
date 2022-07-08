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
const provider = getProvider();
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.signMessage(encodedMessage, "utf8");
```
{% endtab %}

{% tab title="request()" %}
```javascript
const provider = getProvider();
const message = `To avoid digital dognappers, sign below to authenticate with CryptoCorgis`;
const encodedMessage = new TextEncoder().encode(message);
const signedMessage = await provider.request({
    method: "signMessage",
    params: {
         message: encodedMessage,
         display: "hex",
    },
});
```
{% endtab %}
{% endtabs %}
