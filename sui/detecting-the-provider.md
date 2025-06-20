# Detecting the Provider

To detect if a user has already installed Phantom, a web application should check for the existence of a `phantom` object. Phantom's browser extension and mobile in-app browser will both inject a `phantom` object into the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) of any web application the user visits, provided that site is using `https://`, on `localhost`, or is `127.0.0.1`. Phantom will not inject the provider into sites using http://.

If a `phantom` object exists, Sui apps can interact with Phantom via the API found at `window.phantom.sui`.

To detect if Phantom is installed, an application should check for an additional `isPhantom` flag.

```typescript
const isPhantomInstalled = window.phantom?.sui?.isPhantom
```

If Phantom is not installed, we recommend you redirect your users to [our website](https://www.phantom.com). Altogether this may look like the following.

```typescript
const getProvider = () => {
  if ('phantom' in window) {
    const provider = window.phantom?.sui;

    if (provider?.isPhantom) {
      return provider;
    }
  }
  window.open('https://phantom.com/', '_blank');
};
```

