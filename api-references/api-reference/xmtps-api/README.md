# XMTPs API

{% hint style="info" %}
The XMTPs APIs indicate whether a user has XMTP.
{% endhint %}

### Inputs & Filters

```graphql
input XMTPsFilter {
  owner: # Identity: blockchain address, domain name, social identity
}
```

### Outputs

```graphql
type Social {
  blockchain: Blockchain # Blockchain associated with the social identity
  id: ID # Airstack unique identifier for this particular element
  isXMTPEnabled: Boolean # indicate whether a user has XMTP
  owner: Wallet! # **Nested query** allowing to retrieve address, domain names, and social profiles of the owner
}
```
