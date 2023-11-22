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

# SocialFollowings API

The SocialFollowings APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols, such as Farcaster and Lens.

## Inputs

### Filters

| Name                 | Type                            | Description                                                                                                    |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `blockNumber`        | `Int_Comparator_Exp`            | Blocknumber when the follows occured onchain.                                                                  |
| `dappName`           | `SocialDappName_Comparator_Exp` | Social DApp name – lens, farcaster                                                                             |
| `dappSlug`           | `SocialDappSlug_Comparator_Exp` | Social DApp slug (contract version) – lens\_polygon, lens\_v2\_polygon, farcaster\_optimism, farcaster\_goerli |
| `followerProfileId`  | `String_Comparator_Exp`         | The Lens Profile NFT Token ID of the user's follower. Farcaster is empty                                       |
| `followingProfileId` | `String_Comparator_Exp`         | The Lens Profile NFT Token ID of the user's following. Farcaster is empty                                      |
| `followingSince`     | `Time_Comparator_Exp`           | Time the follows was started                                                                                   |
| `identity`           | `Identity_Comparator_Exp`       | Identity: blockchain address, domain name, social identity                                                     |

## Outputs

```graphql
type SocialFollowing {
  blockchain: Blockchain # Blockchain associated with the social identity
  dappName: # Social DApp name
  dappSlug: # Social DApp slug (contract version)
  followerAddress: # Nested query - list of all followers
  followerProfileId:
  followerTokenId:
  followingAddress: # Nested query - list of all followings
  followingProfileId: 
  id: ID # Airstack unique identifier for this particular element
}
```
