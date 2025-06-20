---
description: >-
  To detect if a user has already installed Phantom, a web application should
  check for the existence of a phantom object.
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Detecting the Provider

Phantom's browser extension and mobile in-app browser will both inject a `phantom` object into the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) of any web application the user visits, provided that site is using `https://`, on `localhost`, or is `127.0.0.1.` Phantom will not inject the provider into [iframes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) or sites using `http://`.

If a `phantom` object exists, Bitcoin & Ordinals dApps can interact with Phantom via the API found at `window.phantom.bitcoin`. To detect if Phantom is installed, an application should check for an additional `isPhantom` flag like so:

```typescript
const isPhantomInstalled = window?.phantom?.bitcoin?.isPhantom
```

If Phantom is not installed, we recommend you redirect your users to [our website](https://phantom.app/). Altogether, this may look like the following:

```typescript
const getProvider = () => {
  if ('phantom' in window) {
    const anyWindow: any = window;
    const provider = anyWindow.phantom?.bitcoin;
   
    if (provider && provider.isPhantom) {
      return provider;
    }
  }

  window.open('https://phantom.app/', '_blank');
};
```

When adding a Phantom button to your dApp’s wallet modal, we recommend using the name “Phantom” with an SVG/PNG icon which can be found [here](https://docs.phantom.app/resources/assets).

<figure><img src="../.gitbook/assets/image (15).png" alt="" width="375"><figcaption><p>An example Phantom button in a wallet modal</p></figcaption></figure>
