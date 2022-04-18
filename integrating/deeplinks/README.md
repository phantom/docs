# Deeplinks

As of Phantom `v22.04.11`, iOS and Android apps can now natively interact with Phantom through either [universal links](https://developer.apple.com/ios/universal-links/) **(recommended)** or [deeplinks](https://en.wikipedia.org/wiki/Mobile\_deep\_linking). We refer to both of these workflows collectively as "deeplinks".

All [Provider Methods](provider-methods/) will follow a protocol format of:

```
https://phantom.app/ul/<version>/<method>
```

In addition to these provider methods, Phantom also supports [Other Methods](other-methods/) that are accessible via deeplinks. Specifically, users can open web apps within Phantom's in-app browser via the [Browse](other-methods/browse.md) deeplink.
