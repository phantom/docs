# Swap

{% embed url="https://youtu.be/qJNv7lpYmtw" %}

Phantom supports deeplinking directly to the in-app swapper. Developers can specify which tokens should be prefilled in the `buy` and `sell` fields. In addition to swapping tokens on a single chain, the swapper can also be used to bridge stablecoins across chains (i.e. USDT on Ethereum -> USDC on Solana).

The `swap` deeplink can be used at anytime. It does not need to be proceeded by a [Connect](https://docs.phantom.app/phantom-deeplinks/provider-methods/connect) event, as it does not require a `session` param.

{% hint style="info" %}
`swap` deeplinks are not intended to be pasted into mobile web browsers. These deeplinks must either be handled by an app or tapped on by an end user.
{% endhint %}

### URL Structure <a href="#url-structure" id="url-structure"></a>

```
https://phantom.app/ul/v1/swap?buy=<buy>&sell=<sell>
```

### Parameters <a href="#parameters" id="parameters"></a>

* `buy`: The [CAIP-19](https://github.com/ChainAgnostic/CAIPs/blob/main/CAIPs/caip-19.md) address of the token that should be bought, URL-encoded. Defaults to SOL if omitted.
* `sell`: The [CAIP-19](https://github.com/ChainAgnostic/CAIPs/blob/main/CAIPs/caip-19.md) address of the token that should be sold, URL-encoded. Defaults to SOL if omitted.

### Examples <a href="#parameters" id="parameters"></a>

Using a mobile device, tap the following links to try out these `swap` deeplinks:

[Swap SOL to WIF](https://phantom.app/ul/v1/swap/?buy=solana%3A101%2Faddress%3AEKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm\&sell=)

```
https://phantom.app/ul/v1/swap/?buy=solana%3A101%2Faddress%3AEKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm&sell=
```

[Bridge USDC (SOL) to USDT (ETH)](https://phantom.app/ul/v1/swap/?buy=eip155%3A1%2Faddress%3A0xdAC17F958D2ee523a2206206994597C13D831ec7\&sell=solana%3A101%2Faddress%3AEPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v)

```
https://phantom.app/ul/v1/swap/?buy=eip155%3A1%2Faddress%3A0xdAC17F958D2ee523a2206206994597C13D831ec7&sell=solana%3A101%2Faddress%3AEPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v
```
