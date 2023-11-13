---
description: >-
  Learn how to construct queries to build a recommendation engine based on the
  social follows of the user(s).
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

# 🎉 Social Follows

## Get Recommendation Follows For User(s) Based on Followers

### Fetching

You can fetch follow recommendations for user(s) by simply showing them all the accounts that followed the given user(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/GEk75j6Clw" %}
Show me all followers of vitalik.eth and their account details
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["vitalik.eth"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
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
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee",
              "0x3e21146008f07e27745918b71357f1b3abc30ca8"
            ],
            "socials": [
              {
                "dappName": "farcaster",
<strong>                "profileName": "scnullois", // one of followers of vitalik.eth
</strong>                "profileTokenId": "6119",
                "profileTokenIdHex": "0x017e7",
                "userId": "6119",
                "userAssociatedAddresses": [
                  "0x99d80e209b4904aa2ef25a6014e1a7cf3ed6d5ee",
                  "0x3e21146008f07e27745918b71357f1b3abc30ca8"
                ]
              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
              "0xadd746be46ff36f10c81d6e3ba282537f4c68077"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth"
              },
              {
                "name": "7860000.eth"
              },
              {
                "name": "offchainexample.eth"
              },
              {
                "name": "brianshaw.eth"
              },
              {
                "name": "vbuterin.stateofus.eth"
              },
              {
                "name": "quantumsmartcontracts.eth"
              },
              {
                "name": "Vitalik.eth"
              },
              {
                "name": "openegp.eth"
              },
              {
                "name": "vitalik.cannafam.eth"
              },
              {
                "name": "VITALIK.eth"
              },
              {
                "name": "quantumdapps.eth"
              },
              {
                "name": "vitalik.soy.eth"
              },
              {
                "name": "satoshinart.eth"
              },
              {
                "name": "vitalik.eth"
              },
              {
                "name": "eth.soy.eth"
              },
              {
                "name": "vitalik.thepeople.eth"
              },
              {
                "name": "vitalik.dev.eth"
              },
              {
                "name": "vitalik.takoyaki.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x01612",
                "userId": "5650",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077"
                ]
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          }
        },,
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

With the response, you can get all `followerAddress.socials` and compile them into an array of accounts that you can recommend for the given user(s) to follow.

## Developer Support

If you have any questions or need help regarding building a recommendation engine, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Contact Recommendation](./)
  * [Token Transfers](token-transfers.md)
  * [POAPs](poaps.md)
  * [NFTs](nfts.md)
* [SocialFollowers API](../../api-references/api-reference/socialfollowers-api.md)
* [On-Chain Graph](../onchain-graph.md)
