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
    visible: true
  pagination:
    visible: true
---

# Domains API

The Domain API fetches information on blockchain domains, which are user-friendly names linked to specific blockchain addresses. Example: ENS.

This API can also resolve a domain name into a wallet/contract address.

## Inputs

### filter

| Name              | Type                      | Description                                                                      |
| ----------------- | ------------------------- | -------------------------------------------------------------------------------- |
| `isPrimary`       | `Boolean_Comparator_Exp`  | allows to filter out domains which are set/not set as primary                    |
| `name`            | `String_Comparator_Exp`   | Domain name, e.g. "vitalik.eth" or a subdomain, e.g. "illionaire.illionaire.eth" |
| `owner`           | `Identity_Comparator_Exp` | Identity: blockchain address, domain name, social identity                       |
| `resolvedAddress` | `Address_Comparator_Exp`  | Blockchain address to which the domain resolves to                               |

### blockchain

{% hint style="info" %}
For **Domains** API, the `blockchain` input will fetch all ENS either onchain (Ethereum) or offchain (Namestone & cb.id).
{% endhint %}

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |

### order

| Name                        | Description                                                                                                                         |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `expiryTimestamp`           | Sort the result by the ENS expiry timestamp, either in ascending (`ASC`) or descending (`DESC`) order.                              |
| `createdAtBlockTimestamp`   | Sort the result by the creation block timestamp of the ENS domain, either in ascending (`ASC`) or descending (`DESC`) order.        |
| `lastUpdatedBlockTimestamp` | Sort the result by the last block timestamp of updates on the ENS domain, either in ascending (`ASC`) or descending (`DESC`) order. |

## Output

| Name                                     | Type                                                                           | Description                                                                                                      |
| ---------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| avatar                                   | `String`                                                                       | Link to the ENS Avatar or NFT PFP                                                                                |
| `blockchain`                             | `Blockchain!`                                                                  | Blockchain, where the domain is located                                                                          |
| `chainId`                                | `String`                                                                       | Unique blockchain identifier                                                                                     |
| `createdAtBlockNumber`                   | `Int`                                                                          | Block number when the domain was created                                                                         |
| `createdAtBlockTimestamp`                | `Time`                                                                         | Timestamp when the domain was created                                                                            |
| `dappName`                               | `DomainDappName`                                                               | DApp name associated with the domain (e.g.,ENS)                                                                  |
| `dappSlug`                               | `DomainDappSlug`                                                               | DApp slug (contract version) associated with the domain                                                          |
| `expiryTimestamp`                        | `Time`                                                                         | Timestamp when the domain registration expires; note - ENS has a 3-month grace period after the expiry timestamp |
| `formattedRegistrationCost`              | `Float`                                                                        | Registration cost for the domain                                                                                 |
| `formattedRegistrationCostInNativeToken` | `Float`                                                                        | Registration cost in the native token                                                                            |
| `formattedRegistrationCostInUSDC`        | `Float`                                                                        | Registration cost in USDC                                                                                        |
| `id`                                     | `ID`                                                                           | Airstack unique identifier for the domain                                                                        |
| isNameWrapped                            | Boolean                                                                        | Indication if the ENS name is Wrapped (ERC1155) or not (ERC721).                                                 |
| `isPrimary`                              | `Boolean`                                                                      | Indicates if the domain is set to be primary                                                                     |
| `labelHash`                              | `String`                                                                       | Airstack unique domain hash                                                                                      |
| `labelName`                              | `String`                                                                       | Domain name wihtout the domain ending, eg. 'vitalik' instead of 'vitalik.eth'                                    |
| `lastUpdatedBlockNumber`                 | `Int`                                                                          | Block number when the domain was last updated                                                                    |
| `lastUpdatedBlockTimestamp`              | `Time`                                                                         | Timestamp when the domain was last updated                                                                       |
| manager                                  | Address!                                                                       | Domain manager address                                                                                           |
| managerDetails                           | [Wallet](https://airstackxyz.slack.com/archives/C05FCDM3LTC/p1705944466794539) | Domain manager's details – domains, socials, etc.                                                                |
| `name`                                   | `String`                                                                       | Domain name                                                                                                      |
| `owner`                                  | `Address!`                                                                     | Domain owner address                                                                                             |
| `ownerDetails`                           | [`Wallet`](wallet-api.md)                                                      | Domain owner's details – domains, socials, etc.                                                                  |
| `parent`                                 | `String`                                                                       | Airstack unique hash to retrieve on-chain data to be used in filters                                             |
| `paymentToken`                           | [`Token`](broken-reference)                                                    | Nested query - can retrieve payment token data (name, symbol, etc.)                                              |
| `paymentTokenCostInNativeToken`          | `Float`                                                                        | Domain registration cost in blockchain native token                                                              |
| `paymentTokenCostInUSDC`                 | `Float`                                                                        | Domain registration cost in USDC                                                                                 |
| `registrationCost`                       | `String`                                                                       | Registration cost for the domain                                                                                 |
| `registrationCostInNativeToken`          | `String`                                                                       | Registration cost in the native token                                                                            |
| `registrationCostInUSDC`                 | `String`                                                                       | Registration cost in USDC as a string                                                                            |
| `resolvedAddress`                        | `Address`                                                                      | Address to which the domain is resolved                                                                          |
| `resolvedAddressDetails`                 | [`Wallet`](wallet-api.md)                                                      | Resolved address details – domains, socials, etc.                                                                |
| `subDomainCount`                         | `Int`                                                                          | Count of subdomains linked to the domain                                                                         |
| `subDomains`                             | [`[Domain]`](domains-api.md)                                                   | Nested query - all subdomain-related data                                                                        |
| texts                                    | \[DomainTexts!]                                                                | ENS domain additional onchain records set by the user, such as links, socials, etc.                              |
| `tokenId`                                | `String`                                                                       | Domain Token ID associated with the domain, if applicable                                                        |
| `tokenNft`                               | [`[TokenNft!]`](broken-reference)                                              | Nested query - NFT details of the ENS domain NFT                                                                 |
| `ttl`                                    | `String`                                                                       | Time-to-live value for the domain                                                                                |

{% hint style="info" %}
For ENS, the ENS domain name by default references the resolved address. In some rare cases, the owner and resolved addresses may differ (the owner set it to resolve to another wallet).
{% endhint %}
