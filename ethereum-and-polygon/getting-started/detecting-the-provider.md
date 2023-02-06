# Detecting the Provider

To detect if a user has already installed Phantom, a web application should check for the existence of an `ethereum` object. Phantom's browser extension and mobile in-app browser will both inject an `ethereum` object into the [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) of any web application the user visits, provided that site is using `https://`, on `localhost`, or is `127.0.0.1.` Phantom will not inject the provider into [iframes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) or sites using `http://`.

If the `ethereum` object exists, Ethereum dapps can interact with Phantom via the API found at `window.ethereum`. This object is also made available at `window.phantom` to prevent namespace collisions.&#x20;

The code examples we will show use `window.phantom` as that is the most reliable way to work with Phantom. By interacting with `window.phantom`, you ensure that you don't accidentally call any other software that may be injecting into your browser.

If you are set on using the `window.ethereum` API (the `window.phantom.ethereum` object is identical), you will need to check that Phantom is installed and injecting this `ethereum` object in your browser. To detect if Phantom is installed, an application should check for an additional `isPhantom` flag.

```typescript
const isPhantomInstalled = window?.ethereum?.isPhantom
```

If Phantom is not installed, we recommend you redirect your users to [our website](https://phantom.app/). Altogether, this may look like the following.

{% tabs %}
{% tab title="window.phantom" %}
```typescript
const getProvider = () => {
  if ('phantom' in window) {
    const anyWindow: any = window;
    const provider = anyWindow.phantom?.ethereum;
   
    if (provider) {
      return provider;
    }
  }

  window.open('https://phantom.app/', '_blank');
};
```
{% endtab %}

{% tab title="window.ethereum" %}
```typescript
const getProvider = () => {
  if ('ethereum' in window) {
    const anyWindow: any = window;
    const provider = anyWindow.ethereum;
   
    if (provider?.isPhantom) {
      return provider;
    }
  }

  window.open('https://phantom.app/', '_blank');
};
```
{% endtab %}
{% endtabs %}

For an example of how a React application can detect Phantom, please refer to the [`getProvider` function in our sandbox](https://github.com/phantom-labs/eth\_sandbox/blob/main/src/utils/getProvider.ts).&#x20;
