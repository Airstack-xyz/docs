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

# Tokens API

{% hint style="info" %}
The Tokens API retrieves high-level information on ERC20 tokens, ERC721 tokens (also known as NFT contracts, "Collections," or "NFT Collections," containing multiple tokens), and ERC1155 tokens (also called NFT contracts, "Collections," or "NFT Collections," containing several tokens).
{% endhint %}

## Inputs

### Filters

```graphql
input TokenFilter {
  address: # Token contract address on the blockchain (ERC20, ERC721, ERC1155)
  name:  # Name of the contract (e.g. "Moonbirds"). Note that this will fetch all contracts with the name "Moonbirds"
  owner: # # Identity: blockchain address, domain name, social identity of the owner of the contract
  symbol: # Symbol of the contract (e.g. "BAYC"). Note - it will return all contracts that have the same symbol.
  type: # ERC20, ERC721, ERC1155
  isSpam: # It will return only NFTs that are spams (true) or not spams (false).
}
```

{% hint style="warning" %}
Querying by name or symbol returns all contracts with matching data. These fields are not unique.
{% endhint %}

## Outputs

```graphql
type Token {
  address: Address! # Token contract address on the blockchain
  baseURI: String # Returns the base URI
  blockchain: Blockchain # Blockchain where the token is deployed
  chainId: String # Unique blockchain identifier
  contractMetaData: ContractMetadata # Token's contract metadata object
  contractMetaDataURI: String # URI for the token's contract metadata
  decimals: Int # Number of decimal places for the token's smallest unit
  id: ID # Airstack unique identifier for the contract
  isSpam: Boolean # Indicate whether a token is a spam or not.
  lastTransferBlock: Int # Block number of the token's most recent transfer
  lastTransferHash: String # Hash of the token's most recent transfer
  lastTransferTimestamp: Time # Timestamp of the token's most recent transfer
  logo: # Logo image for the contract in various sizes (if available)
  name: # Token contract name
  owner: Wallet! # **Nested query** allowing to retrieve address, domain names, and social profiles of the contract owner
  projectDetails: # Off-chain data such as project name, description, website, Discord & Twitter
  rawContractMetaData: # Token contract metadata as it appears inside the contact
  symbol: String # Token contract symbol (ticker)
  tokenBalances: # **Nested query** - allows querying Token Balance information
  tokenNfts: # **Nested query** - individual NFT token data
  tokenTraits: Map # Returns count of all token attribute types and values
  totalSupply: String # Total supply of the token
  type: Tokentype # ERC20, ERC721, or ERC1155
}
```
