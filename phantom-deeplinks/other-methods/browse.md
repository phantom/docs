# Browse

Deeplinks provide a convenient way for users to open web apps within Phantom. Using their phone’s camera, users can scan a QR code to open a page directly within Phantom’s in-app browser. If a web app detects that a user is on mobile, it can also prompt the user to open a specific page within Phantom's in-app browser.

The `browse` deeplink can be used before a [Connect](../provider-methods/connect.md) event takes places, as it does not require a `session` param. Please review [Extension & Mobile Browser](broken-reference) for more information on how apps can interact with Phantom from within the in-app browser.

{% hint style="info" %}
`browse` deeplinks are not intended to be pasted into mobile web browsers. These deeplinks must either be handled by an app or clicked on by an end user.
{% endhint %}

### URL Structure

```
https://phantom.app/ul/browse/<url>?ref=<ref>
```

### Parameters

* `url` **(required)**: The URL that should open within Phantom's in-app browser, URL-encoded.
* `ref` **(required)**: The URL of the requesting app, URL-encoded

The following is an example request to open an [NFT listed on Magic Eden](https://magiceden.io/item-details/ED8Psf2Zk2HyVGAimSQpFHVDFRGDAkPjQhkfAqbN5h7d):

```
https://phantom.app/ul/browse/https%3A%2F%2Fmagiceden.io%2Fitem-details%2FED8Psf2Zk2HyVGAimSQpFHVDFRGDAkPjQhkfAqbN5h7d?ref=https%3A%2F%2Fmagiceden.io
```

Result: [browse deeplink to NFT listed on Magic Eden](https://phantom.app/ul/browse/https%3A%2F%2Fmagiceden.io%2Fitem-details%2FED8Psf2Zk2HyVGAimSQpFHVDFRGDAkPjQhkfAqbN5h7d?ref=https%3A%2F%2Fmagiceden.io)
