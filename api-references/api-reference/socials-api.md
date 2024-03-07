---
description: >-
  Learn all the detailed references of Socials API that provide a given user's
  web3 social information, including the input filters, supported chains, and
  output fields.
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

# Socials API

The Socials APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols such as Farcaster, and Lens.

## Inputs

### filter

| Name                             | Type                            | Description                                                                                                    |
| -------------------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `dappName`                       | `SocialDappName_Comparator_Exp` | Social DApp name – lens, farcaster                                                                             |
| `dappSlug`                       | `SocialDappSlug_Comparator_Exp` | Social DApp slug (contract version) – lens\_polygon, lens\_v2\_polygon, farcaster\_optimism, farcaster\_goerli |
| `followerCount`                  | `Int_Comparator_Exp`            | Total number of followers                                                                                      |
| `followingCount`                 | `Int_Comparator_Exp`            | Total number of followings                                                                                     |
| `identity`                       | `Identity_Comparator_Exp`       | Identity: blockchain address, domain name, social identity                                                     |
| `isDefault`                      | `Boolean_Comparator_Exp`        | True/false if the profile is set to default on the corresponding dApp                                          |
| `profileName`                    | `String_Comparator_Exp`         | Profile name on the social app (prefix not required)                                                           |
| `updatedAt`                      | `Time_Comparator_Exp`           | Last timestamp when Lens/Farcaster profile was updated                                                         |
| `userAssociatedAddresses`        | `Address_Comparator_Exp`        | Any associated Wallet address                                                                                  |
| `userId`                         | `String_Comparator_Exp`         | user ID on the social app (prefix not required)                                                                |
| `profileCreatedAtBlockTimestamp` | `Time_Comparator_Exp`           | block timestamp when Lens/Farcaster profile was created                                                        |

### blockchain

{% hint style="info" %}
For **Socials** API, it will return Lens & Farcaster data from all onchain and offchain sources, not specifically Ethereum.

You just need to specify the input for the query to work.
{% endhint %}

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |

### order

| Name                             | Description                                                                                                                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `followerCount`                  | Sort by the number of users following the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order.         |
| `followingCount`                 | Sort by the number of users being followed by the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order. |
| `profileCreatedAtBlockTimestamp` | Sort by Lens/Farcaster profile creation block timestamp in ascending or descending order.                                    |
| `updatedAt`                      | Sort by Lens/Farcaster profile last update timestamp in ascending or descending order.                                       |

## Outputs

```graphql
type Social {
  blockchain: Blockchain # Blockchain associated with the social identity
  chainId: String # Unique blockchain identifier
  connectedAddresses: [ConnectedAddress!] # Provide info on connected addresses, including address and blockchain
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
  profileLastUpdatedAtBlockTimestamp: Time # Last updated timestamp for Lens. For Farcaster, this value will be equal to `profileCreatedAtBlockTimestamp`. To get last updated timestamp for Farcaster, use `updatedAt`.
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
  updatedAt: Time # Last updated timestamp for Lens or Farcaster profile. This will change whenever profile image is changed, bio is changed, or followers/followings number increase/decrease, etc.
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
