---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Lens
  Profiles to other web3 identities (Farcaster, ENS, Ethereum address).
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

# 🌿 Lens

## Get Lens Profiles from a given user(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLens($address: [Identity!]) {
  Socials(
    input: {filter: {identity: {_in: $address}, dappSlug: {_eq: lens_polygon}}, blockchain: ethereum}
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
  "address": [
    "0x4b70d04124c2996de29e0caa050a49822faec6cc",
    "betashop.eth",
    "fc_fname:vbuterin"
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "prashantbagga.lens",
          "dappName": "lens"
        },
        {
          "profileName": "betashop9.lens",
          "dappName": "lens"
        },
        {
          "profileName": "vitalik.lens",
          "dappName": "lens"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum address, Farcaster, and ENS from a given Lens profile(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressOfLens($address: [Identity!]) {
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
