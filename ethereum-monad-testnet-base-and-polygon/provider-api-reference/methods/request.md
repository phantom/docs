---
description: Sends a JSON RPC request to the wallet
---

# request

Params: `method: string;` `params?: unknown[] | object;`

Returns: `Promise<unknown>`

`Example:`

```javascript
const accounts = await window.phantom.ethereum.request({ 
    method: "eth_requestAccounts", params: [] 
})
console.log(accounts)
// ["0xDAFEA492D9c6733ae3d56b7Ed1ADB60692c98Bc5"]
```

The code above demonstrates how you can use the `request` method to ask the user to connect to your dapp.\
\
The `request` method is the go to way for you to interface with the wallet in your dapp. It accepts most [JSON RPC requests](https://ethereum.org/en/developers/docs/apis/json-rpc/#json-rpc-methods) that would need to interact with the wallet. \
\
However it **will not** work for methods that don't make sense for a wallet. E.g. you can't use the provider object Phantom injects to call something like `eth_getTransactionByHash`. If you send a method that the provider object does not support, it will throw an error. You can see a list of errors, and the shape that they will take on [this](../errors.md) page.
