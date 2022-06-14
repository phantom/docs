# FAQ

## Are hardware wallets supported?

Yes, Phantom currently supports [Ledger](https://www.ledger.com/) and requires no special treatment on the application side.

## Why isn't my token displaying properly?

If an SPL token's [On-Chain Metadata Structure](../best-practices/tokens/on-chain-metadata.md#metadata-structure) contains a `uri` field, Phantom will consider it to be non-fungible. Otherwise, Phantom will consider the token to be fungible. Creators of [Non-Fungible tokens (NFTs)](../best-practices/tokens/non-fungible-tokens.md) should ensure that their [URI JSON Schema](../best-practices/tokens/non-fungible-tokens.md#uri-json-schema) properly specifies the item's file extension with the `?ext=` query parameter. Creators of [Fungible Tokens](../best-practices/tokens/tokens.md) should ensure that their token's metadata is merged into the [Solana Token List](https://github.com/solana-labs/token-list).

## What types of NFTs are supported?

Phantom supports a range of NFT media types including images, audio files, video files, and 3d models. At this time, Phantom does not support HTML files. For a full list of the types of NFTs that Phantom will display, please reference [Supported Media Types](../best-practices/tokens/non-fungible-tokens.md#supported-media-types).

## How does Phantom import wallet addresses?

When importing addresses from an existing seed phrase, Phantom will scan for 20 addresses in each of our three supported derivation paths (`bip44change`, `bip44`, and a deprecated path), for a total of 60 addresses. For the convenience of the user, Phantom will filter this list of addresses down to wallets that have ever had signatures (i.e. have ever been used). Phantom will then sort this filtered list based on how many signatures each wallet has had plus the amount of [lamports](https://docs.solana.com/terminology#lamport) it currently owns. &#x20;

## Why does Phantom prepend an additional instruction on standard SPL token transfers?

When transferring SPL tokens, Phantom will first double check that the owner of the receiving token account is the address you expect to send to. To do this, Phantom calls a custom deployment of the [Serum Assert Owner](https://github.com/project-serum/serum-dex/tree/6138ca98280f6433deecde560f3d23cc4a749bae/assert-owner) program. The program address of this deployment is `DeJBGdMFa1uynnnKiwrVioatTuHmNLpyFKnmB5kaFdzQ` and is available on Solana's Devnet, Testnet, and Mainnet.
