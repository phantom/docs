# selectedAddress

## window.phantom.ethereum.selectedAddress

The address of the wallet that is currently connected to the dapp. This value will update upon [accountsChanged ](../events/accounts-changed.md)and  [connect](../events/connect.md) events.\
\
Returns a hexadecimal string.

```javascript
const address = window.phantom.ethereum.selectedAddress;
console.log(address);
// "0xDAFEA492D9c6733ae3d56b7Ed1ADB60692c98Bc5"
```
