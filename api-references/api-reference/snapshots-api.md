---
description: >-
  Learn all the detailed references of Snapshots API that balance/holder
  snapshots information during a specified timestamp, date, or block number.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Snapshots API

The Snapshots API fetches the owners or balances of ERC20, ERC721, and ERC1155 tokens on a given block number or timestamp or date.

The Snapshots API works by returning token balances/holders on a given time, followed by a range of timestamp (`startBlockTimestamp` and `endBlockTimetstamp`) and block numbers (`startBlockNumber` and `endBlockNumber`) to indicate the any changes in balances or token ownership

## Inputs

### filter

| Name           | Type                        | Description                                                                                     |
| -------------- | --------------------------- | ----------------------------------------------------------------------------------------------- |
| `blockNumber`  | `Range_Comparator_Exp`      | Allows filtering based on specific block number (integer), 14562584                             |
| `date`         | `Date_Range_Comparator_Exp` | Allows filtering based on specific date with YYYY-MM-DD format, e.g. 2023-10-25                 |
| `owner`        | `Identity_Comparator_Exp`   | 0x address, ENS domain, Lens profile, Farcaster                                                 |
| `timestamp`    | `Int_Comparator_Exp`        | Epoch Unix timestamp to fetch the specified time snapshots of balances/holders, e.g. 1702559139 |
| `tokenAddress` | `String_Comparator_Exp`     | Token contract address on the blockchain (ERC20, ERC721, ERC1155)                               |
| `tokenId`      | `String_Comparator_Exp`     | Token ID of specified NFT (only for ERC721 and ERC1155)                                         |
| `tokenType`    | `TokenType_Comparator_Exp`  | `ERC20`, `ERC721`, or `ERC1155`                                                                 |

### blockchain

| Name       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `base`     | Base mainnet     |
| `zora`     | Zora mainnet     |

### Outputs

| Name                  | Type              | Description                                                                                                                                                                                                                                                                                             |
| --------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amount`              | `String`          | <p>Token balance amount during specified snapshots.<br><br>It is important to note that <strong>AMM LP tokens</strong> or tokens that generate yield over time will not be reflected into the <code>amount</code> field. Instead, it will only return the initial amount after staking/depositing.</p>  |
| `blockchain`          | `TokenBlockchain` | Blockchain where the balance/holder snapshots is located                                                                                                                                                                                                                                                |
| `chainId`             | `String`          | Blockchain where the balance/holder snapshots is located                                                                                                                                                                                                                                                |
| `endBlockNumber`      | `Int`             | Block number when the token was **last** held. If still hold to present, it will return `-1`                                                                                                                                                                                                            |
| `endBlockTimestamp`   | `Time`            | Timestamp when the token was **last** held. If still hold to present, it will return present timestamp.                                                                                                                                                                                                 |
| `formattedAmount`     | `Float`           | <p>Formatted amount of token (in decimals).<br><br>It is important to note that <strong>AMM LP tokens</strong> or tokens that generate yield over time will not be reflected into the <code>formattedAmount</code> field. Instead, it will only return the initial amount after staking/depositing.</p> |
| `id`                  | `ID!`             | Airstack Internal ID                                                                                                                                                                                                                                                                                    |
| `owner`               | `Wallet!`         | Nested Queries – Owner details of the holder of a token                                                                                                                                                                                                                                                 |
| `startBlockNumber`    | `Int`             | Block number when the token was **first** held by owner.                                                                                                                                                                                                                                                |
| `startBlockTimestamp` | `Time`            | Timestamp when the token was **first** held.                                                                                                                                                                                                                                                            |
| `token`               | `Token`           | Nested Queries – Token details                                                                                                                                                                                                                                                                          |
| `tokenAddress`        | `Address!`        | Token Address                                                                                                                                                                                                                                                                                           |
| `tokenId`             | `String`          | Token ID (only available for ERC721 or ERC1155)                                                                                                                                                                                                                                                         |
| `tokenNft`            | `TokenNft`        | Nested Queries – Token NFT details (only available for ERC721 or ERC1155)                                                                                                                                                                                                                               |
| `tokenType`           | `TokenType`       | `ERC20`, `ERC721`, or `ERC1155`                                                                                                                                                                                                                                                                         |
