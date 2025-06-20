# Sending a Transaction

Once a dapp is connected to Phantom, it can request user permission to send transactions on their behalf. To initiate a transaction, ensure you have the necessary parameters. Refer to [Sui’s developer documentation](https://sdk.mystenlabs.com/typescript/transaction-building/basics) for details on constructing transactions.

```typescript
const transactionParams = {
        transaction: await tx.toJSON(), // Replace with actual transaction
        address: address.toString(), // Sender address
        networkID: SupportedSuiChainIds.SuiMainnet, // or your network ID
};
```

To prompt Phantom to send a transaction to the network, refer to the following code snippet.

```typescript
try {
    const signature = await provider.signTransaction(params);
    return signature;
} catch (error) {
    throw new Error(error instanceof Error ? error.message : 'Failed to sign transaction');
}
```

