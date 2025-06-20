---
description: >-
  Phantom prioritizes on-chain metadata that follows the Token Metadata
  Standard. For Fungible tokens specifically, Phantom will show the following
  fields:
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

# Fungibles

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

#### Displaying Prices

Phantom will display prices for any token that is verified on [Coingecko](https://www.coingecko.com/). When verifying with Coingecko, be sure to include the token's `contract`. For Solana tokens, this should be the mint address. For example, [Bonk](https://www.coingecko.com/en/coins/bonk) is verified with the contract [DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263](https://solscan.io/token/DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263). Once a token has verified its `contract` address with Coingecko, Phantom will automatically begin displaying its price.

If Coingecko is unable to return a price for a given token, Phantom will fallback to using [Birdeye](https://birdeye.so/) for token prices.&#x20;
