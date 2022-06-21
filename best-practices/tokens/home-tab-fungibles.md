# Home Tab (Fungibles)

### Displaying Fungible Tokens

Phantom will prioritize on-chain metadata that follows the [Token Metadata Standard](https://docs.metaplex.com/programs/token-metadata/overview). Specifically, Phantom will show the following fields from an on-chain Metadata Account:

| Field  | Description                              |
| ------ | ---------------------------------------- |
| name   | The name of the token. (i.e. “USD Coin”) |
| symbol | The symbol of the token. (i.e. ”USDC”)   |
| image  | A URI pointing to the token's logo.      |

If a `Fungible` token does not have an on-chain Metadata account, Phantom will fallback to displaying data from the [Solana Labs Token List](https://github.com/solana-labs/token-list). This list is considered deprecated and should not be used to host new tokens. When reading from the token list, Phantom will display the following fields:

| Field                  | Description                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------ |
| name                   | The name of the token. (i.e. “USD Coin”)                                                         |
| symbol                 | The symbol of the token. (i.e. ”USDC”)                                                           |
| logoURI                | A URI pointing to the token's logo.                                                              |
| extensions.coingeckoID | The token ID as defined by the Coingecko API. Phantom uses this to fetch the price of the token. |
