# Signing a Message

When a web application is [connected](establishing-a-connection.md) to Phantom, it can also request that the user signs a given message. Applications are free to write their own messages which will be displayed to users from within Phantom's signature prompt. Message signatures do not involve network fees and are a convenient way for apps to verify ownership of an address.

```typescript
try {
    const encodedMessage = new TextEncoder().encode(message);
    const signature = await provider.signMessage(encodedMessage, address);
    return signature;
} catch (error) {
    throw new Error(error instanceof Error ? error.message : 'Failed to sign message');
}
```

