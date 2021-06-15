# Detecting the Provider

The Phantom browser extension will inject an object called`solana` on the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) object of any web application the user visits.

To detect if a browser extension using this API is installed, you can check for the existence of the `solana` object.

To make it easy to detect Phantom specifically, the extension adds an additional `isPhantom` flag.

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

