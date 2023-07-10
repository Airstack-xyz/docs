---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve ENS to
  other web3 identities (Farcaster, Lens, Ethereum address).
---

# 🔷 ENS

## Get ENS from a given user(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENS($address: [Identity!]) {
  Domains(input: {filter: {owner: {_in: $address}}, blockchain: ethereum}) {
    Domain {
      dappName
      name
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
    "stani.lens",
    "fc_fname:vbuterin"
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Domains": {
      "Domain": [
        {
          "dappName": "ens",
          "name": "skynft.eth"
        },
        {
          "dappName": "ens",
          "name": "quantumexchange.eth"
        },
        {
          "dappName": "ens",
          "name": "vitalik.daohall.eth"
        },
        {
          "dappName": "ens",
          "name": "brianshaw.eth"
        },
        {
          "dappName": "ens",
          "name": "vbuterin.eth"
        },
        {
          "dappName": "ens",
          "name": "klock.eth"
        },
        {
          "dappName": "ens",
          "name": "quantumsmartcontracts.eth"
        },
        {
          "dappName": "ens",
          "name": "legendgames.eth"
        },
        {
          "dappName": "ens",
          "name": "vitalik.ethid.eth"
        },
        {
          "dappName": "ens",
          "name": "elddem.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum address, Lens, and Farcaster from a given ENS name(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENS($ens: [Identity!]) {
  Socials(input: {filter: {identity: {_in: $ens}}, blockchain: ethereum}) {
    Social {
      userAddress
      dappName
      profileName
    }
  }
}
```
{% endtab %}
{% endtabs %}
