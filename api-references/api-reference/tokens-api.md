---
description: >-
  Learn all the detailed references of Tokens API that provide ERC20/721/1155
  token information, including the input filters, supported chains, and output
  fields.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# Tokens API

The Tokens API retrieves high-level information on ERC20 tokens, ERC721 tokens (also known as NFT contracts, "Collections," or "NFT Collections," containing multiple tokens), and ERC1155 tokens (also called NFT contracts, "Collections," or "NFT Collections," containing several tokens).

## Inputs

### Filters

| Name      | Type                       | Description                                                                                                 |
| --------- | -------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `address` | `Address_Comparator_Exp`   | Token contract address on the blockchain (ERC20, ERC721, ERC1155).                                          |
| `name`    | `String_Comparator_Exp`    | Name of the contract (e.g. "Moonbirds"). Note that this will fetch all contracts with the name "Moonbirds". |
| `owner`   | `Identity_Comparator_Exp`  | Identity: blockchain address, domain name, social identity of the owner of the contract                     |
| `symbol`  | `String_Comparator_Exp`    | Symbol of the contract (e.g. "BAYC"). Note - it will return all contracts that have the same symbol.        |
| `type`    | `TokenType_Comparator_Exp` | ERC20, ERC721, ERC1155                                                                                      |
| `isSpam`  | `Boolean_Comparator_Exp`   | It will return only NFTs that are spams (true) or not spams (false).                                        |

{% hint style="warning" %}
Querying by name or symbol returns all contracts with matching data. These fields are not unique.
{% endhint %}

### Blockchain

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `polygon`  | Polygon mainnet  |
| `base`     | Base mainnet     |

### Order

| Name     | Description                                                                                                                                           |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`   | Sort the result by the token name in alphabetical order, either in ascending (`ASC`) or descending (`DESC`) order.                                    |
| `symbol` | Sort the result by the token symbol in alphabetical order, either in ascending (`ASC`) or descending (`DESC`) order.                                  |
| `type`   | Sort the result by the token type in alphabetical order (`ERC1155`, `ERC20`, and `ERC721`), either in ascending (`ASC`) or descending (`DESC`) order. |

## Outputs

| Name                    | Type                                      | Description                                                                                                |
| ----------------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `address`               | `Address!`                                | Token contract address on the blockchain                                                                   |
| `baseURI`               | `String`                                  | Returns the base URI                                                                                       |
| `blockchain`            | `Blockchain`                              | Blockchain where the token is deployed                                                                     |
| `chainId`               | `String`                                  | Unique blockchain identifier                                                                               |
| `contractMetaData`      | `ContractMetadata`                        | Token's contract metadata object                                                                           |
| `contractMetaDataURI`   | `String`                                  | URI for the token's contract metadata                                                                      |
| `decimals`              | `Int`                                     | Number of decimal places for the token's smallest unit                                                     |
| `id`                    | `ID`                                      | Airstack unique identifier for the contract                                                                |
| `isSpam`                | `Boolean`                                 | Indicate whether a token is a spam or not.                                                                 |
| `lastTransferBlock`     | `Int`                                     | Block number of the token's most recent transfer                                                           |
| `lastTransferHash`      | `String`                                  | Hash of the token's most recent transfer                                                                   |
| `lastTransferTimestamp` | `Time`                                    | Timestamp of the token's most recent transfer                                                              |
| `logo`                  | `LogoSizes`                               | Logo image for the contract in various sizes (if available)                                                |
| `name`                  | `String`                                  | Token contract name                                                                                        |
| `owner`                 | [`Wallet`](wallet-api.md)                 | \*\*Nested query\*\* allowing to retrieve address, domain names, and social profiles of the contract owner |
| `projectDetails`        | `ProjectDetails`                          | Off-chain data such as project name, description, website, Discord & Twitter                               |
| `rawContractMetaData`   | `Map`                                     | Token contract metadata as it appears inside the contact                                                   |
| `symbol`                | `String`                                  | Token contract symbol (ticker)                                                                             |
| `tokenBalances`         | [`[TokenBalance!]`](tokenbalances-api.md) | \*\*Nested query\*\* - allows querying Token Balance information                                           |
| `tokenNfts`             | [`[TokenNft!]`](tokennfts-api.md)         | \*\*Nested query\*\* - individual NFT token data                                                           |
| `tokenTraits`           | `Map`                                     | Returns count of all token attribute types and values                                                      |
| `totalSupply`           | `String`                                  | Total supply of the token                                                                                  |
| `type`                  | `TokenType`                               | ERC20, ERC721, or ERC1155                                                                                  |
