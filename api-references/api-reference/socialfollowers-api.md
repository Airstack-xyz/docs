---
description: >-
  Learn all the detailed references of SocialFollowers API that provide a given
  user's social followers information, including the input filters, supported
  chains, and output fields.
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

# SocialFollowers API

The SocialFollowers APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols, such as Farcaster and Lens.

## Inputs

### filter

| Name                 | Type                            | Description                                                                                                    |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `blockNumber`        | `Int_Comparator_Exp`            | Blocknumber when the follows occured onchain.                                                                  |
| `dappName`           | `SocialDappName_Comparator_Exp` | Social DApp name – lens, farcaster                                                                             |
| `dappSlug`           | `SocialDappSlug_Comparator_Exp` | Social DApp slug (contract version) – lens\_polygon, lens\_v2\_polygon, farcaster\_optimism, farcaster\_goerli |
| `followerProfileId`  | `String_Comparator_Exp`         | The Lens Profile NFT Token ID of the user's follower. Farcaster is empty                                       |
| `followingProfileId` | `String_Comparator_Exp`         | The Lens Profile NFT Token ID of the user's following. Farcaster is empty                                      |
| `followerSince`      | `Time_Comparator_Exp`           | Time the follows was started                                                                                   |
| `identity`           | `Identity_Comparator_Exp`       | Identity: blockchain address, domain name, social identity                                                     |

### blockchain

{% hint style="info" %}
For **SocialFollowers** API, it will return Lens & Farcaster followers data from all onchain and offchain sources.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

## Outputs

```graphql
type SocialFollower {
  blockchain: Blockchain # Blockchain associated with the social identity
  blockNumber
  dappName: # Social DApp name
  dappSlug: # Social DApp slug (contract version)
  followerAddress: # Nested query - list of all followers
  followerProfileId:
  followerTokenId:
  followerSince
  followingAddress: # Nested query - list of all followings
  followingProfileId: 
  id: ID # Airstack unique identifier for this particular element
}
```
