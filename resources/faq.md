# FAQ

## Why can't I access Phantom on my website?

Phantom will only [inject its provider](../solana/detecting-the-provider.md) into websites that begin with `https://`, or if the host is `localhost` or `127.0.0.1`. If your website only uses `http://`, Phantom will not inject its provider and you will not be able to access the methods found at `window.phantom`. Encrypting your web traffic and upgrading to `https://` will restore functionality.

Phantom will also not inject its provider into any [iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe).

## Why isn't my token displaying properly?

Phantom supports the [Token Metadata Standard](https://docs.metaplex.com/programs/token-metadata/overview) established by [Metaplex](https://www.metaplex.com/). When displaying tokens, Phantom will first [categorize](../best-practices/tokens/#categorizing-tokens) them according to their [TokenStandard](https://docs.metaplex.com/programs/token-metadata/token-standard). If a token is considered `Fungible`, Phantom will display it on the [Home tab](../best-practices/tokens/home-tab-fungibles.md). Otherwise, Phantom will display it as a [Collectible](../best-practices/tokens/collectibles-nfts-and-semi-fungibles.md). For more information, please review:

{% content-ref url="../best-practices/tokens/" %}
[tokens](../best-practices/tokens/)
{% endcontent-ref %}

## What types of NFTs are supported?

Phantom supports a range of NFT media types including images, audio files, video files, and 3D models. At this time, Phantom does not support HTML files. For a full list of the types of NFTs that Phantom will display, please reference:

{% content-ref url="../best-practices/tokens/supported-media-types.md" %}
[supported-media-types.md](../best-practices/tokens/supported-media-types.md)
{% endcontent-ref %}

## Are hardware wallets supported?

Yes, Phantom currently supports [Ledger](https://www.ledger.com/) and requires no special treatment on the application side.

## How does Phantom import wallet addresses?

When importing addresses from an existing seed phrase, Phantom will scan for 20 addresses in each of our three supported derivation paths (`bip44change`, `bip44`, and a deprecated path), for a total of 60 addresses. For the convenience of the user, Phantom will filter this list of addresses down to wallets that have ever had signatures (i.e. have ever been used). Phantom will then sort this filtered list based on how many signatures each wallet has had plus the amount of [lamports](https://docs.solana.com/terminology#lamport) it currently owns. &#x20;

## Why does Phantom prepend an additional instruction on standard SPL token transfers?

When transferring SPL tokens, Phantom will first double check if a token account exists for the recipient you are sending to. If one does not exist, Phantom will help you create an Associated Token Account on the recipient's behalf. To do this check, Phantom calls a deployment of the [Serum Assert Owner](https://github.com/project-serum/serum-dex/tree/6138ca98280f6433deecde560f3d23cc4a749bae/assert-owner) program. The program address of this deployment is `DeJBGdMFa1uynnnKiwrVioatTuHmNLpyFKnmB5kaFdzQ` and is available on Solana's Devnet, Testnet, and Mainnet. This program has been in use since 2021. It was deployed by the Phantom team to keep this program address consistent across networks.

\
