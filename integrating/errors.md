# Errors

When making requests to Phantom in [Establishing a Connection](extension-and-mobile-browser/establishing-a-connection.md), [Sending a Transaction](extension-and-mobile-browser/sending-a-transaction.md), or [Signing a Message](extension-and-mobile-browser/signing-a-message.md), Phantom may respond with an error. The following is a list of all possible error codes and their meanings. These error messages are inspired by Ethereum's [EIP-1474](https://eips.ethereum.org/EIPS/eip-1474#error-codes) and [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193#provider-errors).

| Code   | Title                 | Description                                                              |
| ------ | --------------------- | ------------------------------------------------------------------------ |
| 4900   | Disconnected          | Phantom could not connect to the network.                                |
| 4100   | Unauthorized          | The requested method and/or account has not been authorized by the user. |
| 4001   | User Rejected Request | The user rejected the request through Phantom.                           |
| -32000 | Invalid Input         | Missing or invalid parameters.                                           |
| -32003 | Transaction Rejected  | Phantom does not recognize a valid transaction.                          |
| -32601 | Method Not Found      | Phantom does not recognize the method.                                   |
| -32603 | Internal Error        | Something went wrong within Phantom.                                     |

Typically, these errors will be easily parseable and have both a code and an explanation. For example:

```javascript
try {
  await window.solana.signMessage();
} catch (err) {
  //  {code: 4100, message: 'The requested method and/or account has not been authorized by the user.'}
}
```
