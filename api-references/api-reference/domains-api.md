---
description: >-
  Learn all the detailed references of Domains API that provide ENS domains
  onchain and offchain (cb.id & Namestone) information, including the input
  filters, supported chains, and output fields.
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

# Domains API

The Domain API fetches information on blockchain domains, which are user-friendly names linked to specific blockchain addresses. Example: ENS.

This API can also resolve a domain name into a wallet/contract address.

## Inputs

### Filters

| Name              | Type                      | Description                                                                      |
| ----------------- | ------------------------- | -------------------------------------------------------------------------------- |
| `isPrimary`       | `Boolean_Comparator_Exp`  | allows to filter out domains which are set/not set as primary                    |
| `name`            | `String_Comparator_Exp`   | Domain name, e.g. "vitalik.eth" or a subdomain, e.g. "illionaire.illionaire.eth" |
| `owner`           | `Identity_Comparator_Exp` | Identity: blockchain address, domain name, social identity                       |
| `parent`          | `String_Comparator_Exp`   | Airstack unique hash to retrieve on-chain data to be used in filters             |
| `resolvedAddress` | `Address_Comparator_Exp`  | Blockchain address to which the domain resolves to                               |

### Blockchain

{% hint style="info" %}
For **Domains** API, the `blockchain` input will fetch all ENS either onchain (Ethereum) or offchain (Namestone & cb.id).
{% endhint %}

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |

## Output

```graphql
type Domain {
  blockchain: Blockchain! # Blockchain, where the domain is located
  chainId: String # Unique blockchain identifier
  createdAtBlockNumber: Int # Block number when the domain was created
  createdAtBlockTimestamp: Time #Timestamp when the domain was created
  dappName:  # DApp name associated with the domain (e.g.,ENS)
  dappSlug:  # DApp slug (contract version) associated with the domain
  expiryTimestamp: # Timestamp when the domain registration expires; note - ENS has a 3-month grace period after the expiry timestamp
  formattedRegistrationCost: Float # Registration cost for the domain
  formattedRegistrationCostInNativeToken: Float # Registration cost in the native token
  formattedRegistrationCostInUSDC: Float # Registration cost in USDC
  id: ID # Airstack unique identifier for the domain
  isPrimary: Boolean # Indicates if the domain is set to be primary
  labelHash: String # Airstack unique domain hash
  labelName: String # Domain name wihtout the domain ending, eg. 'vitalik' instead of 'vitalik.eth'
  lastUpdatedBlockNumber: Int # Block number when the domain was last updated
  lastUpdatedBlockTimestamp: Time # Timestamp when the domain was last updated
  name: String # Domain name
  owner: Address! # Domain owner's address
  ownerDetails: Wallet # Domain owner's details – domains, socials, XMTP, token balances, etc.
  parent: String # Airstack unique hash to retrieve on-chain data to be used in filters
  paymentToken: Token # Nested query - can retrieve payment token data (name, symbol, etc.)
  paymentTokenCostInNativeToken: Float # Domain registration cost in blockchain native token
  paymentTokenCostInUSDC: Float # Domain registration cost in USDC
  registrationCost: String # Registration cost for the domain
  registrationCostInNativeToken: String # Registration cost in the native token
  registrationCostInUSDC: String # Registration cost in USDC as a string
  resolvedAddress: Address # Address to which the domain is resolved
  resolvedAddressDetails: Wallet # Resolved address details – domains, socials, XMTP, token balances, etc.
  subDomainCount: Int # Count of subdomains linked to the domain
  subDomains: [Domain] # Nested query - all subdomain-related data
  tokenId: String # Domain Token ID associated with the domain, if applicable
  tokenNft: [TokenNft] # Nested query - NFT details of the ENS domain NFT 
  ttl: String # Time-to-live value for the domain
}
```

{% hint style="info" %}
For ENS, the ENS domain name by default references the resolved address. In some rare cases, the owner and resolved addresses may differ (the owner set it to resolve to another wallet).
{% endhint %}
