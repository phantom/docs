# Sending a Transaction

Once a web application is connected to Phantom, it can prompt the user for permission to send transactions on their behalf.

To send a transaction, you will need to have a valid transaction object. It should look a little like this:

```json
{
  from: "0xEA674fdDe714fd979de3EdF0F56AA9716B898ec8",
  to: "0xac03bb73b6a9e108530aff4df5077c2b3d481e5a",
  gasLimit: "21000",
  maxFeePerGas: "300",
  maxPriorityFeePerGas: "10",
  nonce: "0",
  value: "10000000000"
}
```

However, this transaction object needs to be signed using the sender's private key. This ensures that only the person that holds the private key can send transactions from the public address.

\
To prompt Phantom to send a transaction to the network, refer to the following code snippet

```javascript
const result = await provider.request({
        method: 'eth_sendTransaction',
        params: [
          {
            from: accounts[0],
            to: '0x0c54FcCd2e384b4BB6f2E405Bf5Cbc15a017AaFb',
            value: '0x0',
            gasLimit: '0x5028',
            gasPrice: '0x2540be400',
            type: '0x0',
          },
        ],
});
```

This is the building blocks of what you will need to send a transaction. However, if you were to copy/paste this, it would likely fail. There are several pieces of a transaction that are best provided in a dynamic manner. Take a look at our [sendTransaction function](https://github.com/phantom-labs/eth\_sandbox/blob/main/src/utils/sendTransaction.ts) in our sandbox for reference.
