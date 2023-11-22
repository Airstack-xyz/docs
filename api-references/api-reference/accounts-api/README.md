---
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

The Accounts API fetches the list of all [ERC6551s](https://eips.ethereum.org/EIPS/eip-6551) or TBAs (token-bound accounts) deployed on-chain.

[Optimistic indexing](#user-content-fn-1)[^1] will soon be supported as well.

## Supported ERC6551 Registries

Currently, [Airstack](https://airstack.xyz) indexes all the official ERC6551 registry contract on both Ethereum and Polygon:

<table><thead><tr><th width="210">Version</th><th>Registry Address</th></tr></thead><tbody><tr><td>0.2.0</td><td><a href="https://etherscan.io/address/0x02101dfB77FDE026414827Fdc604ddAF224F0921"><code>0x02101dfB77FDE026414827Fdc604ddAF224F0921</code></a></td></tr><tr><td>0.3.0</td><td><a href="https://etherscan.io/address/0x284be69BaC8C983a749956D7320729EB24bc75f9"><code>0x284be69BaC8C983a749956D7320729EB24bc75f9</code></a></td></tr><tr><td>0.3.1</td><td><a href="https://etherscan.io/address/0x000000006551c19487814612e58fe06813775758"><code>0x000000006551c19487814612e58FE06813775758</code></a></td></tr></tbody></table>

If you are deploying ERC6551 using a custom registry contract, please reach out to us to add it to the [Airstack](https://airstack.xyz) API by joining our [Telegram](https://t.me/+1k3c2FR7z51mNDRh).

## Inputs

### Filters

| Name                      | Type                             | Description                                                  |
| ------------------------- | -------------------------------- | ------------------------------------------------------------ |
| `standard`                | `AccountStandard_Comparator_Exp` | The standard of the ERC6551.                                 |
| `tokenAddress`            | `String_Comparator_Exp`          | The token address of the ERC721 token that owns the ERC6551. |
| `tokenId`                 | `String_Comparator_Exp`          | The token ID of the ERC721 token that owns the ERC6551.      |
| `address`                 | `Identity_Comparator_Exp`        | The address of an ERC6551 account.                           |
| `createdAtBlockTimestamp` | `Time_Comparator_Exp`            | The block timestamp of the ERC6551 creation transaction.     |

## Outputs

```graphql
type Account {
  id: ID! # Airstack unique identifier for the ERC6551 account.
  standard: AccountStandard! # The standard of the ERC6551.
  blockchain: Blockchain # The blockchain where the ERC6551 token-bound account is created.
  tokenAddress: String # The token address of the ERC721 token that owns the ERC6551.
  tokenId: String # The token ID of the ERC721 token that owns the ERC6551.
  address: Wallet! # The details of an ERC6551 account (address, socials, token balances).
  registry: String # The registry address used to deploy the smart contract wallet.
  implementation: String # The implementation address of the on-chain smart contract account.
  salt: String # The salt for ERC6551 creation.
  createdAtBlockNumber: Int # The block number of the ERC6551 creation transaction.
  createdAtBlockTimestamp: Time # The block timestamp of the ERC6551 creation transaction.
  creationTransactionHash: String # The transaction Hash of the ERC6551 creation transaction.
  deployer: String # The address of the deployer.
  nft: TokenNft # The NFT that owns the ERC6551 account.
  updatedAtBlockNumber: Int # The block number of the latest update on the ERC6551.
  updatedAtBlockTimestamp: Time # The block timestamp of the latest update on the ERC6551.
}
```

[^1]: indexingnon-deployed [ERC6551s](https://eips.ethereum.org/EIPS/eip-6551) or TBAs (token-bound accounts)
