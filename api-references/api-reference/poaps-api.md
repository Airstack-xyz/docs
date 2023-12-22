---
description: >-
  Learn all the detailed references of Poaps API that provide POAP
  balances/holders information, including the input filters, supported chains,
  and output fields.
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

The Poaps API allows users to query POAPs held by a particular wallet, all POAP tokens from a particular event, or simply fetch details about a particular POAP Token, and also return event-specific metadata, including <mark style="background-color:green;">**resized images**</mark>.

## Inputs

### Filters

| Name                   | Type                          | Description                                                                                                       |
| ---------------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `createdAtBlockNumber` | `Int_Comparator_Exp`          | Block Number when POAP was created                                                                                |
| `dappName`             | `PoapDappName_Comparator_Exp` | <p>POAP Dapp Name:<br>- <code>poap_mainnet</code>: Ethereum POAPs<br>- <code>poap_gnosis</code>: Gnosis POAPs</p> |
| `dappSlug`             | `PoapDappSlug_Comparator_Exp` | POAP Dapp Version                                                                                                 |
| `eventId`              | `String_Comparator_Exp`       | POAP Event ID                                                                                                     |
| `owner`                | `Identity_Comparator_Exp`     | Identity: blockchain address, domain name, social identity                                                        |
| `tokenId`              | `String_Comparator_Exp`       | POAP Token ID                                                                                                     |

### Blockchain

{% hint style="info" %}
For **PoapEvents** API, it will return POAPs data from both Ethereum and Gnosis mainnet.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description                                                                                                                        |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `ALL` | Both Ethereum & Gnosis Mainnet. To fetch POAPs data from individual chains, check [`dappName`](poaps-api.md#filters) filter input. |

## Outputs

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
