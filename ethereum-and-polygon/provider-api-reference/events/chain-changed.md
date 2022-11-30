# Chain Changed

Event emitted upon the dapp or wallet changing the network/chain you are connected to

Phantom abstracts the concept of networks, and network switching. So there is no action required on your end as a dapp developer.

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
