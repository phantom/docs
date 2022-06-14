# Detecting the Provider

To detect if a user has already installed Phantom, a web application should check for the existence of a `solana` object. Phantom's browser extension and mobile in-app browser will both inject a `solana` object into the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) of any web application the user visits. The `solana` object is also available on `window.phantom` to prevent namespace collisions.

To make it easy to detect Phantom specifically, Phantom also adds an additional `isPhantom` flag.

```javascript
const isPhantomInstalled = window.solana && window.solana.isPhantom
```

If Phantom is not installed, we recommend you redirect your users to [our website](https://phantom.app/). Altogether, this may look like the following.

```javascript
const getProvider = () => {
  if ("solana" in window) {
    const provider = window.solana;
    if (provider.isPhantom) {
      return provider;
    }
  }
  window.open("https://phantom.app/", "_blank");
};
```
