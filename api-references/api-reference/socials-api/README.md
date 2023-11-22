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

# Socials API

{% hint style="info" %}
The Socials APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols such as Farcaster, and Lens.
{% endhint %}

## Inputs

\#3 Filters

```graphql
input SocialFilter {
  dappName: # Social DApp name – lens, farcaster
  dappSlug: # Social DApp slug (contract version) – lens_polygon, lens_v2_polygon, farcaster_optimism, farcaster_goerli
  followerCount: # Total number of followers
  followingCount: # Total number of followings
  identity: # Identity: blockchain address, domain name, social identity
  isDefault: # True/false if the profile is set to default on the corresponding dApp
  profileName: # Profile name on the social app (prefix not required)
  userAssociatedAddresses: # Any associated Wallet address
  userId: # user ID on the social app (prefix not required)
  profileCreatedAtBlockTimestamp: # block timestamp when Lens/Farcaster profile was created
}
```

### Sorts

| Name                             | Description                                                                                                                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `profileCreatedAtBlockTimestamp` | Sort by Lens/Farcaster profile creation block timestamp in ascending or descending order.                                    |
| `followerCount`                  | Sort by the number of users following the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order.         |
| `followingCount`                 | Sort by the number of users being followed by the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order. |

## Outputs

```graphql
type Social {
  blockchain: Blockchain # Blockchain associated with the social identity
  chainId: String # Unique blockchain identifier
  dappName: # Social DApp name
  dappSlug: # Social DApp slug (contract version)
  dappVersion: String # Airstack unique dapp version number
  fnames: # Farcaster names of a user, there could be more than one
  followerCount: # Total number of followers
  followerTokenAddresses:
  followers: # Nested query - list of all followers
  followingCount: # Total number of followings
  followings: # Nested query - list of all followings
  id: ID # Airstack unique identifier for this particular element
  identity: # Identity used in the filter is returned here
  isDefault: # Returns true/false if the profile is set to default on the corresponding dApp
  profileCreatedAtBlockNumber: Int
  profileCreatedAtBlockTimestamp: Time
  profileLastUpdatedAtBlockNumber: Int
  profileLastUpdatedAtBlockTimestamp: Time
  profileBio: String
  profileDisplayName: String
  profileImage: String
  profileUrl: String
  profileName: String
  profileTokenAddress: String
  profileTokenId: String
  profileTokenIdHex: String
  profileTokenUri: String
  tokenNft: TokenNft # Nested Query: Profile NFT details, this is particularly for Lens Profile NFT, Farcaster will return `null`
  userAddressDetails: Wallet # Nested Query: Details of the user's primary address – domains, socials, XMTP, token balances
  userAssociatedAddressDetails: [Wallet] # Nested Query: Details of the user's associated addresses to the social profile – domains, socials, XMTP, token balances
  userAddress: Address # user's primary address
  userAssociatedAddresses: # blockchain addresses associated with the social profile
  userCreatedAtBlockNumber: Int
  userCreatedAtBlockTimestamp: Time
  userHomeURL: String
  userId: String
  userLastUpdatedAtBlockNumber: Int
  userLastUpdatedAtBlockTimestamp: Time
  userRecoveryAddress: Address
  handleTokenAddress: Address
  handleTokenId: String
  metadataURI: String
  profileMetadata: Map
  coverImageURI: String
  twitterUserName: String
  website: String
  location: String
  profileImageContentValue: Media
  coverImageContentValue: Media
  profileHandle: String
  profileHandleNft: TokenNft
}
```
