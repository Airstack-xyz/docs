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
  Farcaster: SocialFollowers(
    input: {filter: {identity: {_in: ["vitalik.eth"]}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          dappName
          profileName
          userId
          profileImage
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
      hasNextPage
      hasPrevPage
    }
  }
    Lens: SocialFollowers(
    input: {filter: {identity: {_in: ["vitalik.eth"]}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          dappName
          profileName
          userId
          profileImage
          profileImageContentValue{
            image{
              medium
            }
          }
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
      hasNextPage
      hasPrevPage
    }
  }
}
```
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
