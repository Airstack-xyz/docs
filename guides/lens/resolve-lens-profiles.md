---
description: >-
  Learn how to resolve Lens profiles to 0x address, ENS, Farcaster, and XMTP and
  Reverse Resolution.
---

# 🆔 Resolve Lens Profiles

## Topics

* [Get All 0x addresses of Lens profile(s)](resolve-lens-profiles.md#get-all-0x-addresses-of-lens-profile-s)
* [Get all ENS domains owned by Lens profile(s)](resolve-lens-profiles.md#get-all-ens-domains-owned-by-lens-profile-s)
* [Get All Web3 Social Accounts (Lens, Farcaster) owned by 0x address or ENS](resolve-lens-profiles.md#get-all-web3-social-accounts-lens-farcaster-owned-by-0x-address-or-ens)
* [Get Farcaster of Lens Profile(s)](resolve-lens-profiles.md#get-farcaster-of-lens-profile-s)
* [Check If XMTP is Enabled for Lens profile(s)](resolve-lens-profiles.md#check-if-xmtp-is-enabled-for-lens-profile-s)

## Get All 0x addresses of Lens profile(s)

You can resolve an array of Lens profile(s) to their 0x addresses:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Qig0cA3dEv" %}
Show 0x addresses of nader.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfLens {
  Socials(
    input: {filter: {identity: {_in: ["nader.lens"]}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      userAssociatedAddresses
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
          "userAssociatedAddresses": [
            "0xb2ebc9b3a788afb1e942ed65b59e9e49a1ee500d"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get all ENS domains owned by Lens profile(s)

You can resolve an array of Lens profile(s) to their ENS domains:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/UEnpGZrGhp" %}
Show all ENS domains owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENSOfLens {
  Domains(input: {filter: {owner: {_in: ["bradorbradley.lens"]}}, blockchain: ethereum}) {
    Domain {
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
          "name": "tiktoktopmoments.eth"
        },
        {
          "name": "crimsonchin.eth"
        },
        {
          "name": "saintevie.eth"
        },
        {
          "name": "bradorbradley.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Web3 Social Accounts (Lens, Farcaster) owned by 0x address or ENS

You can resolve any 0x address or ENS to their web3 socials, which comprise of Lens and Farcaster:

### Try Demo

{% embed url="https://app.airstack.xyz/query/L6G1XYC1Qu" %}
Show web3 social accounts (Lens, Farcaster) owned by 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetWeb3SocialsOfLens {
  Socials(input: {filter: {identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ethereum}) {
    Social {
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
          "dappName": "farcaster",
          "profileName": "vbuterin"
        },
        {
          "dappName": "lens",
          "profileName": "vitalik.lens"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster of Lens profile(s)&#x20;

You can resolve an array of Lens profile(s) to their Farcaster name and ID, if any:

### Try Demo

{% embed url="https://app.airstack.xyz/query/jbX5EYqXdq" %}
Show Farcasters owned by vitalik.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcastersOfLens {
  Socials(
    input: {filter: {identity: {_in: ["vitalik.lens"]}, dappName: {_eq: farcaster}}, blockchain: ethereum}
  ) {
    Social {
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
          "dappName": "farcaster",
          "profileName": "vbuterin"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If XMTP is Enabled for Lens profile(s)

You can check if an array of Lens profile(s) have their XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qTZ3niLV24" %}
Show if XMTP is enabled for stani.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetXMTPOfLens {
  XMTPs(
    input: {blockchain: ALL, filter: {owner: {_in: ["stani.lens"]}}}
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "stani.lens"
              },
              {
                "profileName": "lilgho.lens"
              },
              {
                "profileName": "lensofficial.lens"
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding resolving identities for Lens profile(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Resolve Identities](../resolve-identities/)
  * [Lens](../resolve-identities/lens.md)
* [Has XMTP](../has-xmtp/)
  * [Check Lens Profile](../has-xmtp/check-single-user.md#by-lens-profile)
  * [Bulk Check Lens Profiles](../has-xmtp/check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
* [Lens Resolver](../../use-cases/lens/universal-resolver.md)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
