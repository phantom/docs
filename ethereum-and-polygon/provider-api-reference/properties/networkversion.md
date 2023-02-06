# networkVersion

## window.ethereum.networkVersion

The network number of the blockchain that you are connected to. This property is available for legacy purposes. It is recommended that modern dapps refer to the `chainId` property to determine what chain a user is connected to currently.

```javascript
const networkVersion = window.phantom.ethereum.networkVersion;
console.log(networkVersion);
// "1"
// Ethereum Mainnet's Network Version
```

