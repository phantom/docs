---
description: >-
  Phantom refers to all NFT-like tokens as collectibles and will display them
  separately from Fungible tokens that appear on the Home tab. Specifically,
  Phantom will display all FungibleAsset, NonFungib
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# NFTs & Semi-Fungibles

On Solana, NFTs are often thought of as [SPL Tokens](https://spl.solana.com/token#example-create-a-non-fungible-token) with 0 decimals and a supply of 1. According to the [Token Metadata Standard](https://docs.metaplex.com/programs/token-metadata/overview), however, it is possible for a range of different tokens to have NFT-like characteristics. Phantom refers to all NFT-like tokens as **collectibles** and will display them separately from `Fungible` tokens that appear on the Home tab. Specifically, Phantom will display all `FungibleAsset`, `NonFungible`, `NonFungibleEdition` and `ProgrammableNonFungible` tokens on their own Collectibles tab.

### Grouping Collectibles

Phantom groups collectibles by their [Certified Collections](https://docs.metaplex.com/programs/token-metadata/certified-collections) introduced in [v1.1.0 of the Token Metadata Standard](http://docs.metaplex.com/token-metadata/Versions/v1.1.0/overview). In order to be grouped together, individual NFTs should all reference the same verified collection mint address. This mint address is itself home to an NFT with metadata that describes the collection ([Example](https://solscan.io/token/SMBH3wF6baUj6JWtzYvqcKuj2XCKWDqQxzspY12xPND#metadata)). Creators must ensure that this collection is [verified on-chain](https://docs.metaplex.com/programs/token-metadata/instructions#verify-the-collection) (i.e. that `verified` is set to `true`).

If no verified collection is found, Phantom will fallback to grouping NFTs by the first verified creator's address in the on-chain `creators` field. If two unverified items share the same creator address at the 0 index of their `creators` array, they will be grouped into the same collection.

### Naming Grouped Collectibles

When a group is created, a best-effort process is used to determine that group’s name. Phantom will look to these fields in the following order of preference:

1. `name` of the verified on-chain collection NFT
2. `collection.name`
3. `collection.family`
4. `external_url` (parsed to remove url parts)
5. `name` (of a single collectible)
6. `symbol`
7. address of the first verified creator in the `creators` array (also used to group the collection)

### Displaying an Individual Collectible

When displaying the detail view of an individual collectible, Phantom will prioritize on-chain data in the Metadata Account over off-chain JSON linked via the `uri` field. This impacts both the `name` and `symbol` field which appears in both locations.

### Rendering Collectible Media

**Supported Media Types**

Phantom supports a wide-range of media types. For a full list, please refer to:

{% content-ref url="supported-media-types.md" %}
[supported-media-types.md](supported-media-types.md)
{% endcontent-ref %}

**Selecting Media**

When determining what media to display for a given collectible, Phantom will search the off-chain JSON for data in the following order of preference:

1. `animation_url` — Phantom will select the media source at the collectible's `animation_url` field.
2. `properties.files` — If no `animation_url` is found, Phantom will choose the first file where the `cdn` property is set to `true`. Otherwise, a file will be chosen based on the media type in the following order of preference:
   1. `image`
   2. `audio`
   3. `video`
   4. `vr` or `model`
3. `image` — Finally, if Phantom still cannot find media to display, it will fallback to the media source at the collectible's `image` field.

**Determining Media Type**

If a media source is found in `properties.files`, and that source is defined as an object, Phantom will determine the media type based on that file's `type` property. Under the Token Metadata Standard, file objects are defined with the following structure:

| Field | Type               | Description                                                                                                                                                                                                                                                                                 |
| ----- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type  | string             | The media type of the file. If selected, **Phantom will use this to determine the media type** (Example: "image/png").                                                                                                                                                                      |
| uri   | string             | The uri source of the file (Example: [https://asfh3uxyeoyvtkfqc7jagy3mhtsszhyubnc3wfss5ismdgtw.arweave.net/BIp90vgjs\_VmosBfSA2NsPOUsnxQLRbsWUuo-kwZp2o?ext=png](https://asfh3uxyeoyvtkfqc7jagy3mhtsszhyubnc3wfss5ismdgtw.arweave.net/BIp90vgjs_VmosBfSA2NsPOUsnxQLRbsWUuo-kwZp2o?ext=png)) |
| cdn   | boolean (optional) | An optional flag that dictates if the file is hosted on a cdn. If `true`, Phantom will select this file as the primary source file.                                                                                                                                                         |

In cases where Phantom cannot find a source from `properties.files`, it may fallback to a media source that is defined as a `string` (e.g. `animation_url` or `image`). In these cases, Phantom will look for data in the following order of preference:

1. The media source uri’s `?ext=` query string parameter (`https://example.com/foo?ext=png`)
2. The media source uri’s pathname extension (`https://example.com/foo.png`)
3. If the media source uri comes from the `animation_url`, Phantom will infer the media type based on the collectible's `properties.category` field.
4. If the media source uri comes from the `image` field, Phantom will default to assume it is a png.

If no supported media type can be determined, no media will be selected, and users may see a placeholder image instead.

**Resizing Images**

Phantom will resize all collectible images to 256x256 pixels. For best results, we recommend images with a square aspect ratio and a power-of-2 dimension (e.g. 256x256, 512x512, or 1024x1024).
