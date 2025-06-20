# Signing a Message

When a web application is connected to Phantom, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within Phantom's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address. You can see our[ handleSignMessage implementation](https://github.com/phantom-labs/eth_sandbox/blob/main/src/App.tsx#L193-L211) to see how you can use libraries such as [ethers.js](https://docs.ethers.org/v5/) to abstract away some of these intricacies&#x20;

```javascript
const message = 'To avoid digital dognappers, sign below to authenticate with CryptoCorgis.';
const from = accounts[0];
const msg = `0x${Buffer.from(message, 'utf8').toString('hex')}`;
const sign = await provider.request({
        method: 'personal_sign',
        params: [msg, from, 'Example password'],
    });
```

## Support for "Sign In With" Standards

Applications that rely on signing messages to authenticate users can choose to opt-in to one of the various Sign In With (SIW) standards. You can read more about them [here](../developer-powertools/signing-a-message.md).
