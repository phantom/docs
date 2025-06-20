# Provider API Reference

Phantom's provider API is exposed to the user through the `window.ethereum` object that is injected into the browser. This same provider is made available at `window.phantom.ethereum` to prevent namespace collisions.&#x20;

This API is how a dApp will make requests to the user; reading account data, connecting to the website, signing messages, and sending transactions will all be done through this provider object.\
\
This provider API is specified in greater detail in [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193).\
\
This area of the documentation will contain information about the API's properties, events, and methods.
