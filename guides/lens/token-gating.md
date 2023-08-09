---
description: >-
  Learn how to enable users to access certain features only if they have a Lens
  profile or a combination of Lens + other criteria such as a specific POAP or
  NFT.
---

# 🚪 Token Gating

## Topic

* [Gating only user(s) that have Lens Profile](token-gating.md#gating-only-user-s-that-have-lens-profile)
* [Gating only user(s) that have Lens Profile and NFT](token-gating.md#gating-only-user-s-that-have-lens-profile-and-nft)
* [Gating only user(s) that have Lens Profile and POAP](token-gating.md#gating-only-user-s-that-have-lens-profile-and-poap)

## Gating only user(s) that have Lens Profile

You can implement token gating by checking whether users have Lens profile:

### Try Demo



{% embed url="https://app.airstack.xyz/query/KoOyThItEi" %}
Show Lens profile of bradorbradley.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenGatingLens {
  Socials(
    input: {filter: {identity: {_eq: "bradorbradley.eth"}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      userId
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
          "profileName": "westlakevillage.lens",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "brad.lens",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "bradorbradley.lens",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "hanimourra.lens",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the length of the `data.Socials.Social` array is 0, then it implies that the user has no Lens profile.

Otherwise, the user does and can be given access to a the desired feature.

## Gating only user(s) that have Lens Profile and NFT

You can implement token gating by checking whether users have both Lens profile and the given NFT:

### Try Demo

{% embed url="https://app.airstack.xyz/query/DLaOAsiLyj" %}
Show NFT balance of 0x8eC94086A724cbEC4D37097b8792cE99CaDCd520 on specific NFTs
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: ["0x977e43ab3eb8c0aece1230ba187740342865ee78", "0x9d90669665607f08005cae4a7098143f554c59ef"]}, owner: {_eq: "0x8eC94086A724cbEC4D37097b8792cE99CaDCd520"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          userId
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "westlakevillage.lens"
              },
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "brad.lens"
              },
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "bradorbradley.lens"
              },
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "hanimourra.lens"
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

If the length of the `data.TokenBalances.TokenBalance` array is 0, then it implies that the user has no given NFT held.

Otherwise, the user have at least one of the given NFT and then can have the `owner.socials` to be checked further to confirm if the user has any Lens profile.

If `owner.socials` has length 0, then similarly the user has no Lens profile.

Otherwise, the user has Lens profile and can be given access to a the desired feature.

## Gating only user(s) that have Lens Profile and POAP

You can implement token gating by checking whether users have both Lens profile and the given POAP:

### Try Demo

{% embed url="https://app.airstack.xyz/query/95WPOVPJTA" %}
Show if 0x4455951fa43b17bd211e0e8ae64d22fb47946ade hold some given specific POAPs and have Lens profile
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(
    input: {filter: {eventId: {_in: ["127462", "141910"]}, owner: {_eq: "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"}}, blockchain: ALL}
  ) {
    Poap {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
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
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "0x131.lens",
                "userId": "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
                "userAssociatedAddresses": [
                  "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"
                ]
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

If the length of the `data.Poaps.Poap` array is 0, then it implies that the user has no given POAP held.

Otherwise, the user have at least one of the given POAP and then can have the `owner.socials` to be checked further to confirm if the user has any Lens profile.

If `owner.socials` has length 0, then similarly the user has no Lens profile.

Otherwise, the user has that Lens profile and can be given access to a the desired feature.

## Developer Support

If you have any questions or need help regarding token gating, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)