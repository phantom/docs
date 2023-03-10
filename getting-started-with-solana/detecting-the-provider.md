# Detecting the Provider

To detect if a user has already installed Phantom, a web application should check for the existence of a `phantom` object. Phantom's browser extension and mobile in-app browser will both inject a `phantom` object into the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) of any web application the user visits, provided that site is using `https://`, on `localhost`, or is `127.0.0.1.` Phantom will not inject the provider into [iframes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) or sites use `http://`.

If a `phantom` object exists, Solana apps can interact with Phantom via the API found at `window.phantom.solana`. This `solana` object is also available at `window.solana` to support legacy integrations.&#x20;

To detect if Phantom is installed, an application should check for an additional `isPhantom` flag.

```typescript
const isPhantomInstalled = window.phantom?.solana?.isPhantom
```

If Phantom is not installed, we recommend you redirect your users to [our website](https://phantom.app/). Altogether, this may look like the following.

```typescript
const getProvider = () => {
  if ('phantom' in window) {
    const provider = window.phantom?.solana;

    if (provider?.isPhantom) {
      return provider;
    }
  }

  window.open('https://phantom.app/', '_blank');
};
```

For an example of how a React application can detect Phantom, please refer to the [`getProvider` function in our sandbox](https://github.com/phantom-labs/sandbox/blob/b57fdd0e65ce4f01290141a01e33d17fd2f539b9/src/App.tsx#L43).&#x20;
