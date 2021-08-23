# Non-Fungible Tokens

On Solana, NFTs are simply [SPL Tokens](https://spl.solana.com/token#example-create-a-non-fungible-token), often, but not necessarily, with 0 decimals and a supply of 1. Phantom will display tokens as "Collectibles" in their own dedicated tab when they are identified as such.

If Phantom cannot identify a particular mint as an NFT, it will display the token as a fungible token instead. This means that tokens which Phantom knows nothing about will appear in the main token list.

![An NFT with metadata defined being displayed in Phantom](../../.gitbook/assets/nft-detail-.png)

## Token Metadata Program

In order to specify metadata for a specific mint you must use the [Token Metadata Program](https://github.com/metaplex-foundation/metaplex/tree/master/rust/token-metadata/program), which allows you to map a mint address to an on-chain account with the relevant data.

The address of the program is: `metaqbxxUerdq28cj1RbAWkYQm3ybzjb6a8bt518x1s`. 

{% hint style="info" %}
You can read more about the metadata standard in the [Solana NFT Metadata Standard](https://docs.metaplex.com/nft-standard) guide.
{% endhint %}

### Metadata Structure

The on-chain metadata contains the following fields, only a few of which are displayed by Phantom.

| Field | Description | Used  |
| :--- | :--- | :--- |
| `name` | The name of the item. | ✅ |
| `symbol` | The symbol of the item.  | ❌ |
| `uri` | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. | ✅ |
| `creators` | An array of public keys for each creator of the item. | ❌ |
| `update_authority` | The public key of the metadata owner. | ❌ |
| `primary_sale_happened` | A boolean flag describing whether the primary sale of the item happened. | ❌ |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales \(0-10000\). | ❌ |

### URI JSON Schema

The [URI JSON Schema](https://docs.metaplex.com/nft-standard#uri-json-schema) is compatible with the [ERC1155 JSON Schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema), as well as the [OpenSea NFT Metadata Standard](https://docs.opensea.io/docs/metadata-standards#section-metadata-structure), which specifies additional fields.

Here are the fields that Phantom makes use of.

| Field | Description |
| :--- | :--- |
| `image` | This is the URL to the image of the item. |
| `name` | The name of the item. |
| `description` | A human readable description of the item. |
| `animation_url` | A URL to a multi-media attachment for the item. The type of file is derived from the file extension, or a `?ext=` query param. |
| `attributes` | A list of attributes to display below the item. Formatted the same way as [OpenSea attributes](https://docs.opensea.io/docs/metadata-standards#section-attributes). |
| `properties.files` | An array of objects specifying a `uri` and a `type` for files that are associated with the item. The `type` represents the file extension. |

  


