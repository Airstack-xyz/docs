---
description: >-
  Learn all the detailed references of TokenNfts API that provide ERC721/1155
  NFT detail information, including the input filters, supported chains, and
  output fields.
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

# TokenNfts API

The TokenNFTs API provides detailed data about a specific NFT token within an NFT Collection, either an ERC721 or ERC1155 contract. You can use this API to return token-specific metadata, including traits, and <mark style="background-color:green;">**resized images**</mark>.

{% hint style="info" %}
Resized NFT images size guide:

* `extra_small`: 125x125px
* `small`: 250x250px
* `medium`: 500x500px
* `large`: 750x750px
{% endhint %}

## Inputs

### filter

| Name       | Type                     | Description                                                        |
| ---------- | ------------------------ | ------------------------------------------------------------------ |
| `address`  | `Address_Comparator_Exp` | Contract address (ERC721, ERC1155)                                 |
| `metaData` | `NftMetadataFilter`      | allows querying by NFT Token Name, or specific trait or attribute. |
| `tokenId`  | `String_Comparator_Exp`  | Unique NFT token ID                                                |

### blockchain

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `base`     | Base mainnet     |
| `zora`     | Zora mainnet     |
| `gold`     | Gold Chain L3    |
| `degen`    | Degen Chain L3   |
| `ham`      | Ham Chain L3     |

## Outputs

| Name                    | Type                                      | Description                                               |
| ----------------------- | ----------------------------------------- | --------------------------------------------------------- |
| `address`               | `Address!`                                | NFT contract address on the blockchain                    |
| `blockchain`            | `TokenBlockchain`                         | Blockchain where the NFT token is deployed                |
| `chainId`               | `String!`                                 | Unique blockchain identifier                              |
| `contentType`           | `String`                                  | Content type of the NFT token (image, video, audio, etc.) |
| `contentValue`          | `Media`                                   | NFT Media - resized images, animation, videos, etc.       |
| `erc6551Accounts`       | [`Accounts`](accounts-api.md)             | Any ERC6551 accounts owned by the associated NFT.         |
| `id`                    | `ID!`                                     | Airstack unique identifier for the NFT token              |
| `lastTransferBlock`     | `Int`                                     | Block number of the token NFT's most recent transfer      |
| `lastTransferHash`      | `String`                                  | Hash of the token NFT's most recent transfer              |
| `lastTransferTimestamp` | `Time`                                    | Timestamp of the token NFT's most recent transfer         |
| `metaData`              | `NftMetadata`                             | Metadata associated with the NFT token                    |
| `rawMetaData`           | `Map`                                     | NFT token metadata as defined in the contract             |
| `token`                 | [`Token`](tokens-api.md)                  | **Nested query** allowing to query contract level data    |
| `tokenBalances`         | [`TokenBalances`](tokenbalances-api.md)   | **Nested query** – with token balance data & owner data   |
| `tokenId`               | `String!`                                 | Unique NFT token ID                                       |
| `tokenTransfers`        | [`TokenTransfers`](tokentransfers-api.md) | **Nested query** – with token transfers data              |
| `tokenURI`              | `String`                                  | URI for the token NFT's resources                         |
| `totalSupply`           | `String`                                  | Total supply of the NFT token                             |
| `type`                  | `TokenType`                               | Type of NFT - ERC721 or ERC1155                           |
