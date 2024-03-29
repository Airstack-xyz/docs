---
description: Learn how to recommend users based on POAPs
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

# 🎫 POAPs

You can use POAP for building a contact recommendation feature for your user to help them connect with users that attended relevant events in the past. To build such a contact recommendation feature, you will need to take two steps:

1. **Query Get POAPs from a given user(s)**
2. Use the list of POAPs held from Query #1 as an input to query **Get all addresses and ENS of the attendees of a given POAP** event(s)

In the future, [Airstack](https://www.airstack.xyz/) will provide a dedicated recommendation API that will combine the two steps into one.

## Get POAPs from a given user(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAllPOAPs($Identity: [Identity!]) {
  Poaps(input: {filter: {owner: {_in: $Identity}}, blockchain: ALL, limit: 10}) {
    Poap {
      id
      eventId
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
          }
        }
      }
      owner {
        identity
        primaryDomain {
          name
        }
        addresses
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "Identity": "dwr.eth"
}
```
{% endtab %}
{% endtabs %}

## Get all addresses, web3 socials and ENS of the attendees of a given POAP event(s)

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAllAddressesSocialsAndENSOfPOAP($eventId: [String!]) {
  Poaps(input: {filter: {eventId: {_in: $eventId}}, blockchain: ALL, limit: 10}) {
    Poap {
      owner {
        identity
        primaryDomain {
          name
        }
        domains {
          name
        }
        socials {
          profileName
          dappName
        }
      }
      poapEvent {
        eventId
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating or building recommendation engine with POAPs data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [POAPs API Reference](../../api-references/api-reference/poaps-api.md)
* [On-Chain Graph](../onchain-graph.md)
