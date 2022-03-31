# Non-Fungible Tokens (NFTs)

On Solana, NFTs are simply [SPL Tokens](https://spl.solana.com/token#example-create-a-non-fungible-token), often, but not necessarily, with 0 decimals and a supply of 1. Phantom will display tokens as "Collectibles" in their own dedicated tab when they are identified as such.

If Phantom cannot identify a particular mint as an NFT, it will display the token as a fungible token instead. This means that tokens which Phantom knows nothing about will appear in the main token list.

![An NFT with metadata defined being displayed in Phantom](<../../.gitbook/assets/NFT (Detail).png>)

### On-Chain Metadata

For non-fungible tokens Phantom uses the following fields from the [On-Chain Metadata](on-chain-metadata.md).

{% hint style="warning" %}
When displaying an NFT, Phantom will default to the information provided within its [URI JSON Schema](non-fungible-tokens.md#uri-json-schema).
{% endhint %}

| Field                     | Description                                                                   | Used  |
| ------------------------- | ----------------------------------------------------------------------------- | ----- |
| `name`                    | The name of the item.                                                         | ✅     |
| `symbol`                  | The symbol of the item.                                                       | ✅     |
| `uri`                     | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. | ✅     |
| `creators`                | An array of public keys for each creator of the item.                         | ✅     |
| `update_authority`        | The public key of the metadata owner.                                         | ❌     |
| `primary_sale_happened`   | A boolean flag describing whether the primary sale of the item happened.      | ❌     |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales (0-10000).      | ❌     |

### URI JSON Schema

When displaying an NFT, Phantom will first look for an item's [URI JSON Schema](https://docs.metaplex.com/token-metadata/Versions/v1.0.0/nft-standard#uri-json-schema) which comes from the [Token Metadata Standard](https://docs.metaplex.com/token-metadata/Versions/v1.0.0/nft-standard#json-structure). This schema is also compatible with the [OpenSea NFT Metadata Standard](https://docs.opensea.io/docs/metadata-standards#section-metadata-structure), which specifies additional fields.

{% hint style="warning" %}
Phantom will derive a file type from the file extension specified in a given field's `?ext=` query parameter.&#x20;
{% endhint %}

Phantom makes use of the following fields:

| Field                 | Description                                                                                                                                                                                                                                                                                         |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `image`               | The URL to the image of the item. The type of file is derived from the file extension as specified in its `?ext=` query parameter. ****                                                                                                                                                             |
| `name`                | The name of the item. This name will be displayed to users and should include the item's ID if the item is part of a broader collection.                                                                                                                                                            |
| `description`         | A human readable description of the item.                                                                                                                                                                                                                                                           |
| `animation_url`       | <p>A URL to a multi-media attachment for the item. The type of file is derived from the file extension, or a <code>?ext=</code> query parameter. <strong></strong> <br><strong></strong><br><strong></strong>Phantom does not currently support HTML pages from the <code>animation_url</code>.</p> |
| `external_url`        | The URL that will appear below the item's description and will allow users to leave Phantom and view the item on your site.                                                                                                                                                                         |
| `attributes`          | A list of attributes to display below the item. Formatted the same way as [OpenSea attributes](https://docs.opensea.io/docs/metadata-standards#section-attributes).                                                                                                                                 |
| `collection`          | An object containing collection `name` and `family` (both should be strings).                                                                                                                                                                                                                       |
| `properties.files`    | An array of objects specifying a `uri` and a `type` for files that are associated with the item, include those specified in the `image` and `animation_url` fields. The `type` represents the file extension and should match the corresponding file's `?ext=` query parameter.                     |
| `properties.category` | The primary category of the item that Phantom uses to serve the correct experience for the item.                                                                                                                                                                                                    |
| `symbol`              | String symbol of the collection.                                                                                                                                                                                                                                                                    |

### Supported Media Types

Phantom supports a range of NFT media types including images, audio files, video files, and 3d models. At this time, Phantom does not support HTML files. The following list includes all file types supported by Phantom:

#### Images

* `.jpeg`
* `.jpg`
* `.png`
* `.gif`

#### Video

* `.mp4`
* `.mov`
* `.webm`
* `.m4v`
* `.ogv`
* `.ogg`

#### Audio

* `.mp3`
* `.wav`
* `.oga`
* `.flac`

#### 3D Models

* `.glb`
* `.gltf`
* `.gltf-binary`

### Grouping Non-Fungible Tokens

Phantom groups NFTs by the first verified creator's address in the `creators` array found on the on-chain metadata. If two items share the same creator address at the 0 index of their `creators` array, they will be grouped into the same collection.

When a group is created, a best-effort process is used to determine that group’s name. That is because not every collection includes all the uri schema JSON key / value pairs. The following data is therefore used in descending order of preference:

1. `collection.name`
2. `collection.family`
3. `external_url` (parsed to remove url parts)
4. `name` (of a single collectible)
5. `symbol`
6. address of the first verified creator in the `creators` array (also used to group the collection)

Please add the above mentioned data if you are a creator and want your collection to be displayed properly in Phantom.&#x20;
