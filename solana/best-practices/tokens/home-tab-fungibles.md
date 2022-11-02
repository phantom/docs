# Home Tab (Fungibles)

### Displaying Fungible Tokens

Phantom prioritizes on-chain metadata that follows the [Token Metadata Standard](https://docs.metaplex.com/programs/token-metadata/overview). For [Fungible tokens](https://docs.metaplex.com/programs/token-metadata/token-standard#the-fungible-standard) specifically, Phantom will show the following fields:

| Field  | Description                              |
| ------ | ---------------------------------------- |
| name   | The name of the token. (i.e. “USD Coin”) |
| symbol | The symbol of the token. (i.e. ”USDC”)   |
| image  | A URI pointing to the token's logo.      |

If a `Fungible` token has `name` and `symbol` fields present on both its on-chain Metadata Account and off-chain JSON file (linked via the on-chain `uri` field), Phantom will prioritize the on-chain fields.

If a `Fungible` token does not have an on-chain Metadata Account, Phantom will fallback to displaying data from the [Solana Labs Token List](https://github.com/solana-labs/token-list). This list is considered deprecated and should not be used to host new tokens. When reading from the token list, Phantom will display the following fields:

| Field                  | Description                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------ |
| name                   | The name of the token. (i.e. “USD Coin”)                                                         |
| symbol                 | The symbol of the token. (i.e. ”USDC”)                                                           |
| logoURI                | A URI pointing to the token's logo.                                                              |
| extensions.coingeckoID | The token ID as defined by the Coingecko API. Phantom uses this to fetch the price of the token. |
