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

# XMTPs API

The XMTPs API indicate whether a user has XMTP.

## Inputs

### Filters

| Name    | Type                      | Description                                                |
| ------- | ------------------------- | ---------------------------------------------------------- |
| `owner` | `Identity_Comparator_Exp` | Identity: blockchain address, domain name, social identity |

### Blockchain

{% hint style="info" %}
For **XMTPs** API, it will return XMTP data from offchain (XMTP network) sources.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### Sorts

## Outputs

```graphql
type Social {
  blockchain: Blockchain # Blockchain associated with the social identity
  id: ID # Airstack unique identifier for this particular element
  isXMTPEnabled: Boolean # indicate whether a user has XMTP
  owner: Wallet! # **Nested query** allowing to retrieve address, domain names, and social profiles of the owner
}
```
