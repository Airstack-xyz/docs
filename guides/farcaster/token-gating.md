---
description: >-
  Learn how to enable users to access certain features only if they have a
  Farcaster account or a combination of Farcaster + other criteria such as a
  specific POAP or NFT.
---

# 🚪 Token Gating

## Topic

* [Gating only user(s) that have Farcaster](token-gating.md#gating-only-user-s-that-have-farcaster)
* [Gating only user(s) that have Farcaster and NFT](token-gating.md#gating-only-user-s-that-have-farcaster-and-nft)
* [Gating only user(s) that have Farcaster and POAP](token-gating.md#gating-only-user-s-that-have-farcaster-and-poap)

## Gating only user(s) that have Farcaster

You can implement token gating by checking whether users have Farcaster:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ZNKiLFepdu" %}
Show Farcaster name and ID of dwr.eth, 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045, and jayden.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenGatingFarcasters {
  Socials(
    input: {filter: {identity: {_in: ["dwr.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "jayden.lens"]}, dappName: {_eq: farcaster}}, blockchain: ethereum}
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
          "profileName": "dwr.eth",
          "userId": "3",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the length of the `data.Socials.Social` array is 0, then it implies that the user has no Farcaster.

Otherwise, the user does and can be given access to a the desired feature.

## Gating only user(s) that have Farcaster and NFT

You can implement token gating by checking whether users have both Farcaster and the given NFT:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/EBQKpjUswY" %}
Show NFT balance of 0xfaba1e9ed7f667e8c7a851c9ed15aed99aa80289 on specific NFTs
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D", "0x23581767a106ae21c074b2276D25e5C3e136a68b"]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
                "userId": "4353",
                "profileName": "yuko",
                "userAssociatedAddresses": [
                  "0x730db7436791d0baba17b4872c43980366f03e38",
                  "0xadd403e94985eee3108e16987eea30c10e2df2ef",
                  "0xfaba1e9ed7f667e8c7a851c9ed15aed99aa80289"
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

If the length of the `data.TokenBalances.TokenBalance` array is 0, then it implies that the user has no given NFT held.

Otherwise, the user have at least one of the given NFT and then can have the `owner.socials` to be checked further to confirm if the user has any Farcaster.

If `owner.socials` has length 0, then similarly the user has no Farcaster.

Otherwise, the user has Farcaster and can be given access to a the desired feature.

## Gating only user(s) that have Farcaster and POAP

You can implement token gating by checking whether users have both Farcaster and the given POAP:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/b7b76BDnX6" %}
Show if 0x4455951fa43b17bd211e0e8ae64d22fb47946ade hold some given specific POAPs
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
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
                "profileName": "megankaspar",
                "userId": "7361",
                "userAssociatedAddresses": [
                  "0xadf37a0d500c748edb1c689a1be26472e583dcb5",
                  "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
                  "0xfaedb341b0faced023099d7b0ccd23c2ec5ed7a5"
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

Otherwise, the user have at least one of the given POAP and then can have the `owner.socials` to be checked further to confirm if the user has any Farcaster.

If `owner.socials` has length 0, then similarly the user has no Farcaster.

Otherwise, the user has Farcaster and can be given access to a the desired feature.

## Developer Support

If you have any questions or need help regarding token gating, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
