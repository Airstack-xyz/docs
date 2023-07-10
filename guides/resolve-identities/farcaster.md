---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Farcaster
  name or ID to other web3 identities (Lens, ENS, Ethereum address).
---

# 🟪 Farcaster

## Get Farcaster from a given user(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcaster($address: [Identity!]) {
  Socials(
    input: {filter: {identity: {_in: $address}, dappSlug: {_eq: farcaster_goerli}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      dappName
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": ["prxshant.eth", "fc_fname:vbuterin"]
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum address, Lens, and ENS from a given Farcaster(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressOfFarcasters($address: [Identity!]) {
  Socials(input: {filter: {identity: {_in: $address}}, blockchain: ethereum}) {
    Social {
      userAddress
      dappName
      profileName
    }
  }
  Domains(input: {filter: {owner: {_in: $address}}, blockchain: ethereum}) {
    Domain {
      dappName
      name
    }
  }
}
```
{% endtab %}
{% endtabs %}
