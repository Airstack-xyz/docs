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
query GetENS {
  Domains(
    input: {filter: {owner: {_in: ["0x4b70d04124c2996de29e0caa050a49822faec6cc", "stani.lens", "fc_fname:vbuterin"]}}, blockchain: ethereum}
  ) {
    Domain {
      dappName
      name
    }
  }
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
query GetENS {
  Socials(
    input: {filter: {identity: {_in: ["vitalik.eth", "yosephks.cb.id"]}}, blockchain: ethereum}
  ) {
    Social {
      userAddress
      dappName
      profileName
    }
  }
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
          "userAddress": "0xc6582cd12debdc9cbe4d972615589aba586550e7",
          "dappName": "farcaster",
          "profileName": "yosephks.eth"
        },
        {
          "userAddress": "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
          "dappName": "farcaster",
          "profileName": "vitalik.eth"
        },
        {
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "dappName": "lens",
          "profileName": "vitalik.lens"
        },
        {
          "userAddress": "0xc7486219881c780b676499868716b27095317416",
          "dappName": "lens",
          "profileName": "yosephks.lens"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}
