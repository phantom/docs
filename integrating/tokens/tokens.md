# Fungible Tokens

For fungible token metadata, in addition to [On-Chain Metadata](on-chain-metadata.md), Phantom currently uses the [@solana/spl-token-registry](https://github.com/solana-labs/token-list), which adheres to the [Token Lists standard](https://tokenlists.org/).

| Field | Description |
| :--- | :--- |
| `address` | The mint address of the SPL token. |
| `symbol` | The symbol of the SPL token \(ie. USDC\). |
| `name` | The name of the SPL token \(ie. USD Coin\). |
| `logoUri` | A URI pointing to an image file of the token logo. |
| `extensions.coingeckoId` | The token ID as defined by the [Coingecko API](https://www.coingecko.com/en/api). Phantom uses this to fetch the price of the token. |

### On-Chain Metadata

When using [On-Chain Metadata](on-chain-metadata.md), Phantom supports the following fields, if present.

| Field | Description | Used  |
| :--- | :--- | :--- |
| `name` | The name of the item. | ✅ |
| `symbol` | The symbol of the item.  | ✅ |
| `uri` | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. | ❌ |
| `creators` | An array of public keys for each creator of the item. | ❌ |
| `update_authority` | The public key of the metadata owner. | ❌ |
| `primary_sale_happened` | A boolean flag describing whether the primary sale of the item happened. | ❌ |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales \(0-10000\). | ❌ |

