# Non-Fungible Tokens

On Solana, NFTs are simply [SPL Tokens](https://spl.solana.com/token#example-create-a-non-fungible-token), often, but not necessarily, with 0 decimals and a supply of 1. Phantom will display tokens as "Collectibles" in their own dedicated tab when they are identified as such.

If Phantom cannot identify a particular mint as an NFT, it will display the token as a fungible token instead. This means that tokens which Phantom knows nothing about will appear in the main token list.

![An NFT with metadata defined being displayed in Phantom](../../.gitbook/assets/nft-detail-.png)

## Token Metadata Program

In order to specify metadata for a specific mint you must use [Token Metadata Program](https://github.com/metaplex-foundation/metaplex/tree/master/rust/token-metadata/program), which allows you to map a mint address to an on-chain account with the relevant data.

The address of the program is: `metaqbxxUerdq28cj1RbAWkYQm3ybzjb6a8bt518x1s`. 

{% hint style="info" %}
You can read more about the metadata standard in the [Metaplex Developer Guide](https://www.notion.so/Metaplex-Developer-Guide-afefbc19841744c28587ab948a08cfac)
{% endhint %}

### Metadata Structure

The on-chain metadata contains the following fields, only a few of which are displayed by Phantom.

| Field | Description | Used  |
| :--- | :--- | :--- |
| `name` | The name of the item. | ✅ |
| `symbol` | The symbol for the item.  | ❌ |
| `uri` | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. | ✅ |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales \(0-10000\). | ❌ |
| `creators` | Array of creators. | ❌ |

### URI JSON Schema

The URI JSON schema is compatible with the [ERC1155 JSON Schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema), as well as the [OpenSea NFT Metadata Standard](ashttps://docs.opensea.io/docs/metadata-standards#section-metadata-structure), which specifies additional fields.

Here are the fields that Phantom makes use of.

| Field | Description |
| :--- | :--- |
| `image` | This is the URL to the image of the item. |
| `name` | Name of the item. |
| `description` | A human readable description of the item. |
| `animation_url` | A URL to a multi-media attachment for the item. Currently only 3d models \(`.glb` and `.gltf` \) are supported. The type of file is derived from the file extension, or a `?ext=` query param. |
| `properties.files` | An array of objects specifying a `uri` and a `type` for files that are associated with the item. The `type` represents the file extension. |

  


