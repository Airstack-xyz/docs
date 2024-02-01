---
description: >-
  Learn all the detailed references of Accounts API that provide ERC6551
  accounts information, including the input filters, supported chains, and
  output fields.
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

# Accounts API

The Accounts API fetches the list of all [ERC6551s](https://eips.ethereum.org/EIPS/eip-6551) or TBAs (token-bound accounts), both deployed on-chain and non-deployed (optimistic).

## Supported ERC6551 Registries

Currently, [Airstack](https://airstack.xyz) indexes all the official ERC6551 registry contracts on Ethereum, Polygon, and Base:

<table><thead><tr><th width="210">Version</th><th>Registry Address</th></tr></thead><tbody><tr><td>0.2.0</td><td><a href="https://etherscan.io/address/0x02101dfB77FDE026414827Fdc604ddAF224F0921"><code>0x02101dfB77FDE026414827Fdc604ddAF224F0921</code></a></td></tr><tr><td>0.3.0</td><td><a href="https://etherscan.io/address/0x284be69BaC8C983a749956D7320729EB24bc75f9"><code>0x284be69BaC8C983a749956D7320729EB24bc75f9</code></a></td></tr><tr><td>0.3.1</td><td><a href="https://etherscan.io/address/0x000000006551c19487814612e58fe06813775758"><code>0x000000006551c19487814612e58FE06813775758</code></a></td></tr></tbody></table>

If you are deploying ERC6551 using a custom registry contract, please reach out to us to add it to the [Airstack](https://airstack.xyz) API by joining our [Telegram](https://t.me/+1k3c2FR7z51mNDRh).

## Inputs

### Filter

| Name                      | Type                             | Description                                                                                                                                                                                                                                                                                                                     |
| ------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `standard`                | `AccountStandard_Comparator_Exp` | The standard of the ERC6551.                                                                                                                                                                                                                                                                                                    |
| `tokenAddress`            | `Address_Comparator_Exp`         | The token address of the ERC721 token that owns the ERC6551.                                                                                                                                                                                                                                                                    |
| `tokenId`                 | `String_Comparator_Exp`          | The token ID of the ERC721 token that owns the ERC6551.                                                                                                                                                                                                                                                                         |
| `address`                 | `Identity_Comparator_Exp`        | The address of an ERC6551 account.                                                                                                                                                                                                                                                                                              |
| `createdAtBlockTimestamp` | `Time_Comparator_Exp`            | The block timestamp of the ERC6551 creation transaction.                                                                                                                                                                                                                                                                        |
| `registry`                | `Address_Comparator_Exp`         | <p>The registry of the ERC6551 account. This can be used to fetch certain <a href="accounts-api.md#supported-erc6551-registries">versions</a> of ERC6551 accounts.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this defaults to registry v.0.3.1</p> |
| `implementation`          | `Address_Comparator_Exp`         | <p>The implementation address of the ERC6551 account.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this defaults to the official standard ERC6551 implementation address.</p>                                                                         |
| `salt`                    | `String_Eq_Comparator_Exp`       | <p>The salt of ERC6551 account.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this defaults to 0.</p>                                                                                                                                                  |

### Blockchain

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `polygon`  | Polygon mainnet  |
| `base`     | Base mainnet     |

### Order

| Name                      | Description                                                                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `createdAtBlockTimestamp` | Sort the result by the block timestamp of the ERC6551 creation transaction, either in ascending (`ASC`) or descending (`DESC`) order. |

### Nested Input\*

{% hint style="info" %}
The nested inputs only appear within the nested input, but not for top-level `Accounts` APIs.
{% endhint %}

| Name                    | Type      | Description                                                                                                                                                                   |
| ----------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `showOptimisticAddress` | `Boolean` | If set true, it will show all the optimistic/non-deployed TBA addresses. Otherwise, the optimistic/non-deployed TBAs will be hidden and only the deployed ones will be shown. |

## Outputs

| Name                      | Type               | Description                                                                                                                                                                                                                                                            |
| ------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                      | `ID!`              | Airstack unique identifier for the ERC6551 account.                                                                                                                                                                                                                    |
| `standard`                | `AccountStandard!` | The standard of the ERC6551.                                                                                                                                                                                                                                           |
| `blockchain`              | `Blockchain`       | The blockchain where the ERC6551 token-bound account is deployed.                                                                                                                                                                                                      |
| `tokenAddress`            | `String`           | The token address of the ERC721 token that owns the ERC6551.                                                                                                                                                                                                           |
| `tokenId`                 | `String`           | The token ID of the ERC721 token that owns the ERC6551.                                                                                                                                                                                                                |
| `address`                 | `Wallet!`          | The details of an ERC6551 account (address, socials, token balances).                                                                                                                                                                                                  |
| `registry`                | `String`           | The registry address used to deploy the smart contract wallet.                                                                                                                                                                                                         |
| `implementation`          | `String`           | The implementation address of the on-chain smart contract account.                                                                                                                                                                                                     |
| `salt`                    | `String`           | The salt for ERC6551 creation.                                                                                                                                                                                                                                         |
| `createdAtBlockNumber`    | `Int`              | <p>The block number of the ERC6551 creation transaction.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this will return -1 as the account was never deployed on-chain.</p>                    |
| `createdAtBlockTimestamp` | `Time`             | <p>The block timestamp of the ERC6551 creation transaction.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this will return <code>null</code> as the account was never deployed on-chain.</p>  |
| `creationTransactionHash` | `String`           | <p>The transaction Hash of the ERC6551 creation transaction.<br><br>For non-deployed (optimistic) accounts in <a href="tokennfts-api.md"><code>tokenNfts</code></a> nested queries, this will return <code>null</code> as the account was never deployed on-chain.</p> |
| `deployer`                | `String`           | The address of the deployer.                                                                                                                                                                                                                                           |
| `nft`                     | `TokenNft`         | The NFT that owns the ERC6551 account.                                                                                                                                                                                                                                 |
| `updatedAtBlockNumber`    | `Int`              | The block number of the latest update on the ERC6551.                                                                                                                                                                                                                  |
| `updatedAtBlockTimestamp` | `Time`             | The block timestamp of the latest update on the ERC6551.                                                                                                                                                                                                               |
