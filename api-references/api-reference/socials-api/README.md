# Socials API

{% hint style="info" %}
The Socials APIs deliver on-chain and off-chain user-level data pertaining to users of web3 social protocols such as Farcaster, and Lens.
{% endhint %}

### Inputs & Filters

```graphql
input SocialFilter {
  dappName: # Social DApp name
  dappSlug: # Social DApp slug (contract version)
  identity: # Identity: blockchain address, domain name, social identity
  isDefault: # True/false if the profile is set to default on the corresponding dApp
  profileName: # Profile name on the social app (prefix not required)
  userAssociatedAddresses: # Any associated Wallet address
  userId: # user ID on the social app (prefix not required)
}
```

### Outputs

```graphql
type Social {
blockchain: Blockchain # Blockchain associated with the social identity
chainId: String # Unique blockchain identifier
dappName: # Social DApp name
dappSlug: # Social DApp slug (contract version)
dappVersion: String # Airstack unique dapp version number
id: ID # Airstack unique identifier for this particular element
identity: # Identity used in the filter is returned here
isDefault: # Returns true/false if the profile is set to default on the corresponding dApp
profileCreatedAtBlockNumber: Int
profileCreatedAtBlockTimestamp: Time
profileLastUpdatedAtBlockNumber: Int
profileLastUpdatedAtBlockTimestamp: Time
profileName: String
profileTokenAddress: String
profileTokenId: String
profileTokenIdHex: String
profileTokenUri: String
userAddress: Address # user's primary address
userAssociatedAddresses: # blockchain addresses associated with the social profile
userCreatedAtBlockNumber: Int
userCreatedAtBlockTimestamp: Time
userHomeURL: String
userId: String
userLastUpdatedAtBlockNumber: Int
userLastUpdatedAtBlockTimestamp: Time
userRecoveryAddress: Address
}
```
