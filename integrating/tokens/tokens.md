# Fungible Tokens

For fungible token metadata, Phantom currently searches the [@solana/spl-token-registry](https://github.com/solana-labs/token-list), which adheres to the [Token Lists standard](https://tokenlists.org/).

| Field | Description |
| :--- | :--- |
| `address` | The mint address of the SPL token. |
| `symbol` | The symbol of the SPL token \(ie. USDC\). |
| `name` | The name of the SPL token \(ie. USD Coin\). |
| `logoUri` | A URI pointing to an image file of the token logo. |
| `extensions.coingeckoId` | The token ID as defined by the [Coingecko API](https://www.coingecko.com/en/api). Phantom uses this to fetch the price of the token. |

