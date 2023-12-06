# Snapshots API

The Snapshots API fetches the owners or balances of ERC20, ERC721, and ERC1155 tokens on a given block number or timestamp.

## Inputs

### Filters

| Name           | Type                        | Description                                                                     |
| -------------- | --------------------------- | ------------------------------------------------------------------------------- |
| `blockNumber`  | `Range_Comparator_Exp`      | Allows filtering based on specific block number (integer), 14562584             |
| `date`         | `Date_Range_Comparator_Exp` | Allows filtering based on specific date with YYYY-MM-DD format, e.g. 2023-10-25 |
| `owner`        | `Identity_Comparator_Exp`   | Identity: blockchain address, domain name, social identity                      |
| `timestamp`    | `Time_Range_Comparator_Exp` |                                                                                 |
| `tokenAddress` | `String_Comparator_Exp`     | Token contract address on the blockchain (ERC20, ERC721, ERC1155)               |
| `tokenId`      | `String_Comparator_Exp`     | Token ID of specified NFT (only for ERC721 and ERC1155)                         |
| `tokenType`    | `TokenType_Comparator_Exp`  | `ERC20`, `ERC721`, or `ERC1155`                                                 |

### Blockchain

| Name   | Description  |
| ------ | ------------ |
| `base` | Base mainnet |

### Outputs

```graphql
type Snapshots {
  amount: String! # Token balance amount
  blockNumber: Integer # Block number
  blockchain: Blockchain # Blockchain where the token balance is located
  chainId: String! # Unique blockchain identifier
  endBlockNumber:
  endBlockTimestamp:
  formattedAmount: # Token amount
  id: # Airstack ID
  owner: Wallet! # **Nested query** allowing to retrieve address, domain names, and social profiles of the owner
  startBlockNumber
  startBlockTimestamp
  token: Token # **Nested query** to get token contract level data
  tokenAddress: Address! # Token contract address
  tokenId: String # Unique NFT token ID, if applicable
  tokenNfts: # **Nested query** allowing to get NFT token data (images, traits, etc.)
  tokenType: TokenType # ERC20, ERC721, or ERC1155
}
```
