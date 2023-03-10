# Displaying Tokens

If you've created a token on Solana using the [SPL Token Program](https://spl.solana.com/token), then your token is compatible with Phantom. If Phantom users own a certain balance of an SPL token, that balance will always appear in their wallet. However, if Phantom cannot find more metadata about that token, it will display the token as "Unknown".

### Searching for Metadata

When searching for metadata, Phantom will first look to the [Token Metadata Program](https://docs.metaplex.com/programs/token-metadata/overview) established by [Metaplex](https://www.metaplex.com/). This program enhances ordinary SPL token [mints](https://docs.solana.com/integrations/exchange#token-mints) with a **Metadata Account** that describes additional fields such as the token's `symbol`, `image`, and `description`. Some of these fields exist on-chain in the Metadata Account itself, while others exist off-chain in a JSON file that follows a [standard format](https://docs.metaplex.com/programs/token-metadata/token-standard). The link to this off-chain JSON file is found at the Metadata Account's `uri` field.

If a Metadata Account is found, Phantom will prioritize on-chain fields (e.g. `name`, `symbol`) before off-chain fields described in the `uri` JSON file.

If a token does not have a Metadata Account, Phantom will fallback to reading metadata from the [Solana Labs Token List](https://github.com/solana-labs/token-list). This token list is considered deprecated and should not be used to host new tokens.

### Categorizing Tokens

Phantom will categorize and display tokens based on their [Token Standard](https://docs.metaplex.com/programs/token-metadata/token-standard#token-standards). The `tokenStandard` field can be found in the token's on-chain Metadata Account and is used to describe a token's fungibility. The `tokenStandard` field has four options:

* `Fungible`: A token with simple metadata that can be freely mixed with others of the same mint. Common examples include [USDC](https://www.circle.com/en/usdc) and [SRM](https://www.projectserum.com/).
* `FungibleAsset`: A token with metadata that can also have NFT-like attributes. Commonly referred to as Semi-Fungible, these tokens are often used in gaming contexts to support stackable items like a piece of wood.
* `NonFungible`: A non-fungible token with a [Master Edition](https://docs.metaplex.com/programs/token-metadata/accounts#master-edition) account. This is the most popular type of NFT, encompassing well known collections like [Solana Monkey Business](https://solanamonkey.business/) and [DeGods](https://www.degods.com/).
* `NonFungibleEdition`: A non-fungible token with an [Edition](https://docs.metaplex.com/programs/token-metadata/accounts#edition) account (printed from a Master Edition). This is a helpful feature for creators who want to offer multiple copies of their 1/1 NFTs.
* `ProgrammableNonFungible`: A new non-fungible asset class which allows for flexible configuration of various lifecycle rules triggered by specific actions. More info about Programmable NFTs or pNFTs can be found [here](https://github.com/metaplex-foundation/metaplex-program-library/blob/master/token-metadata/program/ProgrammableNFTGuide.md).

If no `tokenStandard` is set, Phantom will fallback to categorizing tokens based on the following logic:

1. If the total mint supply is 1, Phantom will consider the token to be `NonFungible`.
2. If the total mint supply is greater than 1 and the mint has 0 decimals, Phantom will consider the token to be a `FungibleAsset`.
3. Phantom will consider all other tokens to be `Fungible`.

### Displaying Tokens

Phantom will display all `Fungible` tokens in the Home tab. For more information on `Fungible` token best practices, please see:

{% content-ref url="home-tab-fungibles.md" %}
[home-tab-fungibles.md](home-tab-fungibles.md)
{% endcontent-ref %}

All other token standards (`FungibleAsset`, `NonFungible`, and `NonFungibleEdition`) will be displayed in the Collectibles tab. For more information on collectible best practices, please refer to:

{% content-ref url="collectibles-nfts-and-semi-fungibles.md" %}
[collectibles-nfts-and-semi-fungibles.md](collectibles-nfts-and-semi-fungibles.md)
{% endcontent-ref %}
