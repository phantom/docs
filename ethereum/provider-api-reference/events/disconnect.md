# Disconnect

Event emitted upon the wallet losing connection to the RPC provider.

This is **not** a user "disconnecting" from a dapp, or otherwise revoking access between the dapp and the wallet.

```typescript
 window.ethereum.on('disconnect', () => {
      console.log('lost connection to the rpc)
});
```

You can see an example of [hooking into this event ](https://github.com/phantom-labs/eth\_sandbox/blob/main/src/App.tsx#L106-L112)in our sandbox.
