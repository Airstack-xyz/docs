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

The Accounts API fetches the list of all [ERC6551s](https://eips.ethereum.org/EIPS/eip-6551) or TBAs (token-bound accounts) created on-chain.

### Inputs & Filters

```graphql
input AccountFilter {
  standard: AccountStandard_Comparator_Exp # The standard of the ERC6551.
  tokenAddress: String_Comparator_Exp # The token address of the ERC721 token that owns the ERC6551.
  tokenId: String_Comparator_Exp # The token ID of the ERC721 token that owns the ERC6551.
  address: Identity_Comparator_Exp # The address of an ERC6551 account.
  createdAtBlockTimestamp: Time_Comparator_Exp # The block timestamp of the ERC6551 creation transaction.
}
```

### Outputs

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
