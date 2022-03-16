# Errors

When making requests to Phantom in [Establishing a Connection](establishing-a-connection.md), [Sending a Transaction](sending-a-transaction.md), or [Signing a Message](signing-a-message.md), Phantom may respond with an error. Here are the error codes and their meanings. These are inspired the corresponding by [EIP-1474](https://eips.ethereum.org/EIPS/eip-1474#error-codes) and [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193#provider-errors) on Ethereum.

| Code   | Title                 | Description                                                              |
| ------ | --------------------- | ------------------------------------------------------------------------ |
| 4001   | User Rejected Request | User rejected the request through Phantom.                               |
| 4100   | Unauthorized          | The requested method and/or account has not been authorized by the user. |
| 4900   | Disconnected          | Phantom could not connect to the network.                                |
| -32603 | Internal Error        | Something went wrong within Phantom.                                     |
| -32601 | Method Not Found      | Phantom does not recognize the method.                                   |
| -32003 | Transaction Rejected  | Phantom does not the transaction as valid.                               |
| -32000 | Invalid Input         | Missing or invalid parameters.                                           |

Typically, these errors will be easily parseable and have both a code and an explanation. For example:

```javascript
try {
  await window.solana.signMessage();
} catch (err) {
  //  {code: 4100, message: 'The requested method and/or account has not been authorized by the user.'}
}
```
