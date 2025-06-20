# Solana Actions & Blinks

{% embed url="https://youtu.be/TU87JjHxQQo" %}

[Solana Actions](https://solana.com/docs/advanced/actions) are APIs to deliver signable transactions from a variety of web contexts. Blockchain links, or [Blinks](https://solana.com/docs/advanced/actions#blinks), are client applications that introspect Action APIs and construct user interfaces to interact with Actions.

As of June 2024, the Phantom Extension supports rendering Blinks on Twitter/X.com. If an Action URL is posted to Twitter/X.com, the Phantom extension is can construct a UI to interact with the transaction from directly within the user's timeline.

This feature is disabled by default. Users can opt-in to enabling this feature via by navigating to `Experimental Settings` -> `Enable Solana Actions on X.com`. Phantom will only render Actions that are registered under the Dialect-maintained [Actions Registry](https://solana.com/docs/advanced/actions#dialect's-actions-registry).

