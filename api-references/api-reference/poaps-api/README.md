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

# Poaps API

{% hint style="info" %}
The Poaps API allows users to query POAPs held by a particular wallet, all POAP tokens from a particular event, or simply fetch details about a particular POAP Token, and also return event-specific metadata, including  <mark style="background-color:green;">**resized images**</mark>.
{% endhint %}

### Inputs & Filters

```graphql
input PoapFilter {
  createdAtBlockNumber: # Block Number when POAP was created
  dappName: # POAP Dapp Name
  dappSlug: # POAP Dapp Version
  eventId: #POAP Event ID
  owner: # Identity: blockchain address, domain name, social identity
  tokenId: # POAP Token ID
}
```

### Outputs

```graphql
type Poap {
  attendee: # **Nested query** The holder/attendee of the POAP events, includes total POAPs the attendee own
  id: # Airstack unique element identifier
  chainId: # Blockchain ID 
  blockchain: # Blockchain name
  dappName: 
  dappSlug: 
  dappVersion: 
  eventId: #POAP Event ID
  owner: Wallet! # **Nested query** allowing to retrieve address, domain names, and social profiles of the owner
  createdAtBlockTimestamp: # Time when POAP was created
  createdAtBlockNumber: # Block Number when POAP was created
  mintHash: # Transaction Hash of Mint Transaction
  mintOrder: # The order of POAP minting
  tokenId: #POAP Token ID
  tokenAddress: #POAP contract address
  tokenUri: # POAP Token URI
  transferCount: $
  poapEvent:# **Nested query** allowing to return all POAP event-related metadata including images.
}
```
