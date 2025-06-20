# Connect

Event emitted upon connecting to a dapp.

```typescript
interface connectionInfo {
  chainId: string;
}

window.ethereum.on('connect', (connectionInfo: connectionInfo) => {
      console.log(connectionInfo.chainId);
      // "0x1" On Ethereum
});
```

You can see an example of [hooking into this event](https://github.com/phantom-labs/eth_sandbox/blob/main/src/App.tsx#L98-L104) in our sandbox.
