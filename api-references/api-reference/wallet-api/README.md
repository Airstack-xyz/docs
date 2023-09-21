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

# Wallet API

{% hint style="info" %}
The Wallet APIs deliver comprehensive data about a specific wallet address, including the tokens they currently possess, the domain names they own, their token transfer and sale history, and their on-chain social profiles (like ENS, Farcaster).

From the Wallet API it’s possible to create complex queries using nested queries, such as: Get the primary ENS, the Farcaster user name, the Farcaster connected Addresses, all tokens held by the wallet, all NFT Sales by the wallet, all token transfers.
{% endhint %}

### Inputs & Filters

```graphql
input WalletInput {
  blockchain: Blockchain # Blockchain where the token is deployed
  identity:# Identity: blockchain address, domain name, social identity
}
```

### Outputs

```graphql
type Wallet {
  accounts: # Nested query – return ERC6551 detail information on the account (if any) 
  addresses: Addresses! # return addresses associated with the identity input
  domains: # Nested query - allows querying domains owned by the address
  identity: # return identity passed from the input
  nftSaleTransactions: # Nested query - allows querying NFT Sales by the address
  primaryDomain: # Nested query - allows returning primary domains, if applicable
  socialFollowers: # Nested query - return all followers of the wallet address (Lens & Farcaster)
  socialFollowings: # Nested query - return all followings of the wallet address (Lens & Farcaster)
  socials: # returns social profile information related to the address
  tokenBalances: # Nested query - allows returning token balances
  tokenTransfers: # Nested query - allows returning token transfers and related information
  tokenNfts: # Nested query – allows returning all ERC721, ERC1155, and ERC6551 (token bound accounts)
}
```
