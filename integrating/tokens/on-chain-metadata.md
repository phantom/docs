# On-Chain Metadata

For both fungible and non-fungible tokens, Phantom supports the [Token Metadata Program](https://github.com/metaplex-foundation/metaplex/tree/master/rust/token-metadata/program),  which allows you to map a mint address to an on-chain account with the relevant data.

The address of the program is: `metaqbxxUerdq28cj1RbAWkYQm3ybzjb6a8bt518x1s`. 

{% hint style="info" %}
You can read more about the metadata standard in the [Token Metadata Standard](https://docs.metaplex.com/nft-standard) guide.
{% endhint %}

### Metadata Structure

The on-chain metadata contains the following fields. How or whether the fields are used depend on whether the token is considered fungible, or non-fungible. If the `uri` field is present, the token is considered non-fungible and will appear in the collectibles tab, otherwise it is considered fungible and appears in the home tab.

| Field | Description |
| :--- | :--- |
| `name` | The name of the item. |
| `symbol` | The symbol of the item.  |
| `uri` | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. |
| `creators` | An array of public keys for each creator of the item. |
| `update_authority` | The public key of the metadata owner. |
| `primary_sale_happened` | A boolean flag describing whether the primary sale of the item happened. |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales \(0-10000\). |

{% page-ref page="tokens.md" %}

{% page-ref page="non-fungible-tokens.md" %}

