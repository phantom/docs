# Chain Changed

Event emitted upon the wallet losing connection to the RPC provider.

This is **not** a user "disconnecting" from a dapp, or otherwise revoking access between the dapp and the wallet.

```typescript
ethereum.on('chainChanged', (chainId: string) => {
  console.log(chainId)
  // "0x1" on Ethereum
  /* Phantom will handle all of the internal changes needed to handle the new chain.
   * As the dapp developer, 
   * you just need to make sure all of your transaction requests
   * populate the correct chainId
   */
});
```
