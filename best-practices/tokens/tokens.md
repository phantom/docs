# Fungible Tokens

Fungible tokens are simple SPL tokens with limited metadata. These tokens are often highly divisible and will be displayed by Phantom on the home tab. Examples of these include USDC, USDT, and SRM.&#x20;

When displaying fungible tokens, Phantom will supersede any [On-Chain Metadata](on-chain-metadata.md) with information found on the [Solana Token List](https://github.com/solana-labs/token-list). This list is open to community contributions and adheres to the [Token Lists Standard](https://tokenlists.org/).

When referencing the Token List, Phantom will display the following fields:

| Field                    | Description                                                                                                                          |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| `address`                | The mint address of the SPL token.                                                                                                   |
| `symbol`                 | The symbol of the SPL token (ie. "USDC").                                                                                            |
| `name`                   | The name of the SPL token (ie. "USD Coin").                                                                                          |
| `logoUri`                | A URI pointing to an image file of the token logo.                                                                                   |
| `extensions.coingeckoId` | The token ID as defined by the [Coingecko API](https://www.coingecko.com/en/api). Phantom uses this to fetch the price of the token. |

### On-Chain Metadata

When no Token List entry is present, Phantom will fallback to displaying [On-Chain Metadata](on-chain-metadata.md). Phantom supports the following fields:

| Field                     | Description                                                                   | Used  |
| ------------------------- | ----------------------------------------------------------------------------- | ----- |
| `name`                    | The name of the item.                                                         | ✅     |
| `symbol`                  | The symbol of the item.                                                       | ✅     |
| `uri`                     | URI to [ERC1155](https://0xjac.github.io/EIPs/EIPS/eip-1155) compatible JSON. | ❌     |
| `creators`                | An array of public keys for each creator of the item.                         | ❌     |
| `update_authority`        | The public key of the metadata owner.                                         | ❌     |
| `primary_sale_happened`   | A boolean flag describing whether the primary sale of the item happened.      | ❌     |
| `seller_fee_basis_points` | Royalty basis points that goes to creators in secondary sales (0-10000).      | ❌     |
