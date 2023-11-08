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

# SocialFollowers API

{% hint style="info" %}
The SocialFollowers APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols, such as Farcaster and Lens.
{% endhint %}

### Inputs & Filters

```graphql
input SocialFollowerFilter {
  dappName: # Social DApp name – lens, farcaster
  dappSlug: # Social DApp slug (contract version) – lens_polygon, lens_v2_polygon, farcaster_optimism, farcaster_goerli
  identity: # Identity: blockchain address, domain name, social identity
}
```

### Outputs

```graphql
type SocialFollower {
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
