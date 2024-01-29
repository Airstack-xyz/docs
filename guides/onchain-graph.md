---
description: Learn how to use Airstack to display the Onchain Graph of users.
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

# 🕸 Onchain Graph

The Onchain Graph is the **web3 address book.** It analyzes all of a user/wallet's onchain interactions and recommends contacts based on their strengh of relationship. It currently brings together all of the user's onchain interactions & tokens in common across POAPs, NFTs, token transfers, and Lens and Farcaster.

Developers are utilizing Onchain Graph for recommendation engines, pre-populating friends lists, address books, spam filters, product enhancements, and more.

### Live Demo

We have integrated onchain graph into the [Airstack Explorer ](https://explorer.airstack.xyz)as you can see below. In Airstack Explorer you can enter any 0x address, Lens, Farcaster, or ENS and get the user's onchain graph.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Onchain Graph integration on Airstack Explorer</p></figcaption></figure>

To try it out yourself, click here:

{% embed url="https://explorer.airstack.xyz/onchain-graph?identity=betashop.eth" %}
betashop.eth's onchain graph
{% endembed %}

## Table Of Contents

In this tutorial, you'll learn how to build an onchain graph for your web3 social application using either JavaScript or Python.

{% hint style="info" %}
Currently, Airstack Explorer's onchain graph implementation has no backend and hence it takes time to scan and fetch all the data.

For **backend integrations**, it is best practice that you take the following approach for the best user experience:

1. fetch your users' onchain graph data periodically (e.g. once a day) as a cronjob
2. store your user's onchain graph data into your preferred database
3. Fetched the data from your frontend and cache it

With this approach, your user shall receive their onchain graph data almost instantaneously instead of calling the API on-demand which could take minutes.

In the future, we shall provide webhooks and a dedicated Onchain Graph API for lighter-weight integrations.
{% endhint %}

The algorithm for building onchain graph will be as follows:

1. [Fetch All Onchain Graph Data](onchain-graph.md#step-1-fetch-all-onchain-graph-data)
   * [Fetch Common POAP Holders Data](onchain-graph.md#step-1.1-fetch-common-poap-holders-data)
   * [Fetch Farcaster Followings Data](onchain-graph.md#step-1.2-fetch-farcaster-followings-data)
   * [Fetch Lens Followings Data](onchain-graph.md#step-1.3-fetch-lens-followings-data)
   * [Fetch Farcaster Followers Data](onchain-graph.md#step-1.4-fetch-farcaster-followers-data)
   * [Fetch Lens Followers Data](onchain-graph.md#step-1.5-fetch-lens-followers-data)
   * [Fetch Token Transfers Sent Data](onchain-graph.md#step-1.7-fetch-token-transfers-received-data)
   * [Fetch Token Transfers Received Data](onchain-graph.md#step-1.7-fetch-token-transfers-received-data)
   * [Fetch Common Ethereum Token Holders Data](onchain-graph.md#step-1.8-fetch-common-ethereum-nft-holders-data)
   * [Fetch Common Polygon Token Holders Data](onchain-graph.md#step-1.9-fetch-common-polygon-nft-holders-data)
   * [Fetch Common Base Token Holders Data](onchain-graph.md#step-1.10-fetch-common-polygon-nft-holders-data)
2. [Aggregate All Data By User Identities](onchain-graph.md#step-2-aggregate-all-data-by-user-identities)
3. [Scoring & Sorting](onchain-graph.md#step-3-scoring-and-sorting)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

### Get Started

To get started, install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
**React**

```sh
npm install @airstack/airstack-react
```

**Node**

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
**React**

```sh
yarn add @airstack/airstack-react
```

**Node**

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
**React**

```sh
pnpm install @airstack/airstack-react
```

**Node**

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack
```
{% endtab %}
{% endtabs %}

### Step 1: Fetch All Onchain Graph Data

In order to build a comprehensive onchain graph of a user, it'll require various kinds of data to analyze. Those data comprises of:

* **common POAP holders** that are also attended by the user
* Lens and Farcaster **social followers and following** of the given user
* Token transfers **senders and receivers**
* **common Ethereum, Polygon, and Base NFT holders** that are also held by the user

In this step, you'll learn to fetch all the data that you need to build the onchain graph of a user.

#### Step 1.1: Fetch Common POAP Holders Data

In order to fetch the common POAP holders that hold the POAPs attended by a given user, it will require 2 steps:

1. [Fetch all non-virtual POAPs' event IDs owned by a user](onchain-graph.md#fetch-all-non-virtual-poaps-event-ids-owned-by-a-user)
2. [Fetch all POAP holders of an array of POAP event IDs](onchain-graph.md#fetch-all-poap-holders-of-an-array-of-poap-event-ids)

**Fetch all non-virtual POAPs' event IDs owned by a user**

You can use Airstack to fetch all the POAPs that are hold by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), and check if the events are non-virtual or not:

**Demo**

{% embed url="https://app.airstack.xyz/query/t6zJ93uJ3A" %}
Show me all POAPs owned by vitalik.eth with their event IDs and whether they are virtual or not
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  Poaps(input: { filter: { owner: { _eq: $user } }, blockchain: ALL }) {
    Poap {
      eventId
      poapEvent {
        isVirtualEvent
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
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
          "eventId": "80393",
          "poapEvent": {
            "isVirtualEvent": false
          }
        },
        {
          "eventId": "79011",
          "poapEvent": {
            "isVirtualEvent": false
          }
        },
        {
          "eventId": "15678",
          "poapEvent": {
            "isVirtualEvent": false
          }
        }
        // other POAPs owned by vitalik.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, the response can be filtered to only non-virtual POAPs and be formatted into an array of event IDs to be used in the next step:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const eventIds =
  data?.Poaps.Poap?.filter((poap) => !poap?.poapEvent?.isVirtualEvent).map(
    (poap) => poap?.eventId
  ) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
event_ids = [
  poap.get('eventId')
  for poap in data.get('Poaps', {}).get('Poap', [])
  if not poap.get('poapEvent', {}).get('isVirtualEvent')
] if data and 'Poaps' in data and 'Poap' in data['Poaps'] else []
```
{% endtab %}
{% endtabs %}

where `data` is the response from the API. The formatted result, will be an array of event IDs of the non-virtual POAPs owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

```json
[
  "80393",
  "79011",
  "15678",
  "76134",
  "149333"
  // other non-virtual POAP event IDs held by vitalik.eth
]
```

**Fetch all POAP holders of an array of POAP event IDs**

Using the array of event IDs from the first step, you can fetch all POAP holders that hold any of the POAPs that the given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), owned/attended:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/UMgiOv8Uwk" %}
show me POAP holders of an array of POAP event IDs
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($eventIds: [String!]) {
  Poaps(input: { filter: { eventId: { _in: $eventIds } }, blockchain: ALL }) {
    Poap {
      eventId
      poapEvent {
        eventName
        contentValue {
          image {
            extraSmall
          }
        }
      }
      attendee {
        owner {
          addresses
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "eventIds": [
    "80393",
    "79011",
    "15678",
    "76134",
    "149333",
    "117166",
    "74916",
    "69822",
    "68648",
    "84",
    "74803",
    "11",
    "3606",
    "74216",
    "4",
    "129645",
    "65",
    "129619",
    "107435",
    "98262",
    "124221",
    "145196",
    "67256",
    "74441",
    "7426",
    "100475",
    "71115",
    "38",
    "129422",
    "69787",
    "102610",
    "36",
    "92705",
    "48474",
    "34123",
    "153310",
    "101153",
    "74699",
    "67650"
  ]
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
          "eventId": "79011",
          "poapEvent": {
            "eventName": "ITU Blockchain - Devcon Satellite",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/Llu3dveWH3HHEYsATZWtJQ==/extra_small.png"
              }
            }
          },
          "attendee": {
            "owner": {
              "addresses": ["0x3f27512a67f663c31522a6dd81ee739ddc44f0ea"],
              "domains": null,
              "socials": [
                {
                  "dappName": "lens",
                  "blockchain": "polygon",
                  "profileName": "lens/@egeagus",
                  "profileImage": "",
                  "profileTokenId": "66698",
                  "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
                }
              ],
              "xmtp": null
            }
          }
        }
        // Other POAP holders
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that has common POAPs with the given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatPoapsData.js" %}
```javascript
function formatPoapsData(poaps, exitingUser = []) {
  const recommendedUsers = [...exitingUser];
  for (const poap of poaps ?? []) {
    const { attendee, poapEvent, eventId } = poap ?? {};
    const { eventName: name, contentValue } = poapEvent ?? {};
    const { addresses } = attendee?.owner ?? {};
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );
    if (existingUserIndex !== -1) {
      recommendedUsers[existingUserIndex].addresses = [
        ...(recommendedUsers?.[existingUserIndex]?.addresses ?? []),
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      const _poaps = recommendedUsers?.[existingUserIndex]?.poaps || [];
      const poapExists = _poaps.some((poap) => poap.eventId === eventId);
      if (!poapExists) {
        _poaps?.push({ name, image: contentValue?.image?.extraSmall, eventId });
        recommendedUsers[existingUserIndex].poaps = [..._poaps];
      }
    } else {
      recommendedUsers.push({
        ...(attendee?.owner ?? {}),
        poaps: [{ name, image: contentValue?.image?.extraSmall, eventId }],
      });
    }
  }
  return recommendedUsers;
}

export default formatPoapsData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/poaps.py" %}
```python
def format_poaps_data(poaps, existing_user=None):
    if existing_user is None:
        existing_user = []

    recommended_users = existing_user.copy()
    for poap in poaps or []:
        attendee = poap.get('attendee', {})
        poap_event = poap.get('poapEvent', {})
        event_id = poap.get('eventId')

        name = poap_event.get('eventName')
        content_value = poap_event.get('contentValue', {})
        addresses = attendee.get('owner', {}).get('addresses', [])

        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(addr in recommended_user_addresses for addr in addresses):
                existing_user_index = index
                break

        image = content_value.get('image', {}).get(
            'extraSmall') if content_value.get('image') else None

        new_poap = {
            'name': name,
            'image': image,
            'eventId': event_id
        }

        if existing_user_index != -1:
            recommended_user = recommended_users[existing_user_index]
            _addresses = set(recommended_user.get('addresses', []))
            _addresses.update(addresses)
            recommended_user['addresses'] = list(_addresses)

            _poaps = recommended_user.get('poaps', [])
            if event_id and all(poap['eventId'] != event_id for poap in _poaps):
                _poaps.append(new_poap)
            recommended_user['poaps'] = _poaps
        else:
            new_user = attendee.get('owner', {})
            new_user['poaps'] = [new_poap]
            recommended_users.append(new_user)

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xd35f7c2f23fdc341aa8c7534f0e521679206a036"],
    "domains": [ { "name": "taoliu.eth", "isPrimary": true } ],
    "socials": [
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@colinlt",
        "profileImage": "",
        "profileTokenId": "33481",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      }
    ],
    "xmtp": null,
<strong>    "poaps": [ // show all common POAPs also owned by vitalik.eth
</strong>      {
        "name": "Rocket Pool Bot Catcher POAP",
        "image": undefined,
        "eventId": "7426"
      }
    ]
  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all common POAP holders data**

With the queries for fetching common POAP holders established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchPoapsData.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatPoapsData from "../utils/formatPoapsData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const userPoapsEventIdsQuery = `
query MyQuery {
  Poaps(input: {filter: {owner: {_eq: "vitalik.eth"}}, blockchain: ALL}) {
    Poap {
      eventId
      poapEvent {
        isVirtualEvent
      }
    }
  }
}
`;

const poapsByEventIdsQuery = `
query MyQuery($eventIds: [String!]) {
  Poaps(input: {filter: {eventId: {_in: $eventIds}}, blockchain: ALL}) {
    Poap {
      eventId
      poapEvent {
        eventName
        contentValue {
          image {
            extraSmall
          }
        }
      }
      attendee {
        owner {
          addresses
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
  }
}
`;

const fetchPoapsData = async (address, existingUsers = []) => {
  let poapsDataResponse;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!poapsDataResponse) {
      // Paagination #1: Fetch All POAPs
      poapsDataResponse = await fetchQueryWithPagination(
        userPoapsEventIdsQuery,
        {
          user: address,
        }
      );
    }
    const {
      data: poapsData,
      error: poapsError,
      hasNextPage: poapsHasNextPage,
      getNextPage: poapsGetNextPage,
    } = poapsDataResponse ?? {};
    if (!poapsError) {
      const eventIds =
        poapsData?.Poaps.Poap?.filter(
          (poap) => !poap?.poapEvent?.isVirtualEvent
        ).map((poap) => poap?.eventId) ?? [];
      let poapHoldersDataResponse;
      while (true) {
        if (eventIds.length === 0) break;
        if (!poapHoldersDataResponse) {
          // Pagination #2: Fetch All POAP holders
<strong>          poapHoldersDataResponse = await fetchQueryWithPagination(
</strong>            poapsByEventIdsQuery,
            {
              eventIds,
            }
          );
        }
        const {
          data: poapHoldersData,
          error: poapHoldersError,
          hasNextPage: poapHoldersHasNextPage,
          getNextPage: poapHoldersGetNextPage,
        } = poapHoldersDataResponse;
        if (!poapHoldersError) {
          recommendedUsers = [
            ...formatPoapsData(poapHoldersData?.Poaps?.Poap, recommendedUsers),
          ];
          if (!poapHoldersHasNextPage) {
            break;
          } else {
            poapHoldersDataResponse = await poapHoldersGetNextPage();
          }
        } else {
          console.error("Error: ", poapHoldersError);
          break;
        }
      }
      if (!poapsHasNextPage) {
        break;
      } else {
        poapsDataResponse = await poapsGetNextPage();
      }
    } else {
      console.error("Error: ", poapsError);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchPoapsData;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/poaps.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.poaps import format_poaps_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

user_poaps_event_ids_query = """
query MyQuery($user: Identity!) {
  Poaps(input: {filter: {owner: {_eq: $user}}, blockchain: ALL}) {
    Poap {
      eventId
      poapEvent {
        isVirtualEvent
      }
    }
  }
}
"""

poaps_by_event_ids_query = """
query MyQuery($eventIds: [String!]) {
  Poaps(input: {filter: {eventId: {_in: $eventIds}}, blockchain: ALL}) {
    Poap {
      eventId
      poapEvent {
        eventName
        contentValue {
          image {
            extraSmall
          }
        }
      }
      attendee {
        owner {
          addresses
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
  }
}
"""

async def fetch_poaps_data(address, existing_users=[]):
    poaps_data_response = None
    recommended_users = existing_users.copy()
    while True:
        if poaps_data_response is None:
            execute_query_client = api_client.create_execute_query_object(
                query=user_poaps_event_ids_query, variables={'user': address})
                # Pagination #1: Fetch All POAPs
<strong>            poaps_data_response = await execute_query_client.execute_paginated_query()
</strong>
        if poaps_data_response.error is None:
            event_ids = [
                poap.get('eventId')
                for poap in poaps_data_response.data.get('Poaps', {}).get('Poap', [])
                if not poap.get('poapEvent', {}).get('isVirtualEvent')
            ] if poaps_data_response.data and 'Poaps' in poaps_data_response.data and 'Poap' in poaps_data_response.data['Poaps'] else []
            poap_holders_data_response = None
            while True:
                if poap_holders_data_response is None:
                    execute_query_client = api_client.create_execute_query_object(
                        query=poaps_by_event_ids_query, variables={'eventIds': event_ids})
                        # Pagination 2: Fetch all POAP Holders
<strong>                    poap_holders_data_response = await execute_query_client.execute_paginated_query()
</strong>
                if poap_holders_data_response.error is None:
                    recommended_users = format_poaps_data(
                        poap_holders_data_response.data.get(
                            'Poaps', {}).get('Poap', []),
                        recommended_users
                    )

                    if not poap_holders_data_response.has_next_page:
                        break
                    else:
                        poap_holders_data_response = await poap_holders_data_response.get_next_page
                else:
                    print("Error: ", poap_holders_data_response.error)
                    break

            if not poaps_data_response.has_next_page:
                break
            else:
                poaps_data_response = await poaps_data_response.get_next_page
        else:
            print("Error: ", poaps_data_response.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.2: Fetch Farcaster Followings Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that is being followed on Farcaster by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/B2gIbVbPXh" %}
Show all Farcaster followings of vitalik.eth and check if they're mutual followings
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {
      filter: { identity: { _eq: $user }, dappName: { _eq: farcaster } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {
            filter: { identity: { _eq: $user }, dappName: { _eq: farcaster } }
          }
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```
// Some code
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that is being followed on Farcaster by the given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatFarcasterFollowingsData.js" %}
```javascript
function formatFarcasterFollowingsData(followings, existingUser = []) {
  const recommendedUsers = [...existingUser];
  for (const following of followings) {
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          following.addresses?.includes?.(address)
        )
    );

    const followsBack = Boolean(following?.mutualFollower?.Follower?.[0]);
    if (existingUserIndex !== -1) {
      const follows = recommendedUsers?.[existingUserIndex]?.follows ?? {};
      recommendedUsers[existingUserIndex] = {
        ...following,
        ...recommendedUsers[existingUserIndex],
        follows: {
          ...follows,
          followingOnFarcaster: true,
          followedOnFarcaster: followsBack,
        },
      };
    } else {
      recommendedUsers.push({
        ...following,
        follows: {
          followingOnFarcaster: true,
          followedOnFarcaster: followsBack,
        },
      });
    }
  }
  return recommendedUsers;
}

export default formatFarcasterFollowingsData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/farcaster_followings.py" %}
```python
def format_farcaster_followings_data(followings, existing_user=None):
    if existing_user is None:
        existing_user = []

    recommended_users = existing_user.copy()
    for following in followings:
        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(addr in recommended_user_addresses for addr in following.get('addresses', [])):
                existing_user_index = index
                break

        mutual_follower = following.get('mutualFollower', {})
        follower = mutual_follower.get(
            'Follower') if mutual_follower is not None else []
        follows_back = bool(follower[0]) if follower else False

        if existing_user_index != -1:
            follows = recommended_users[existing_user_index].get('follows', {})
            recommended_users[existing_user_index] = {
                **following,
                **recommended_users[existing_user_index],
                'follows': {
                    **follows,
                    'followingOnFarcaster': True,
                    'followedOnFarcaster': follows_back
                }
            }
        else:
            recommended_users.append({
                **following,
                'follows': {
                    'followingOnFarcaster': True,
                    'followedOnFarcaster': follows_back
                }
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": [
      "0xf6fd7deec77d7b1061435585df1d7fdfd4682577",
      "0x925afeb19355e289ed1346ede709633ca8788b25",
      "0x18b7511938fbe2ee08adf3d4a24edb00a5c9b783"
    ],
    "domains": [
      { "name": "phil.brightmoments.eth", "isPrimary": true },
      { "name": "centerforonchainstudies.eth", "isPrimary": false },
      { "name": "purple.philm.eth", "isPrimary": true },
      { "name": "centerforonchainstructure.eth", "isPrimary": false },
      { "name": "lorensbags.eth", "isPrimary": false },
      { "name": "onchainstudies.eth", "isPrimary": false }
    ],
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "phil",
        "profileImage": "https://i.imgur.com/sx6qqM7.jpg",
        "profileTokenId": "129",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      }
    ],
    "xmtp": [{ "isXMTPEnabled": true }],
    "mutualFollower": { "Follower": null },
    // show vitalik.eth following this user on Faracster and being followed back
<strong>    "follows": { "followingOnFarcaster": true, "followedOnFarcaster": true }
</strong>  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all users being followed on Farcaster**

With the queries for fetching all the users being followed on Farcaster of a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchFarcasterFollowings.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatFarcasterFollowingsData from "../utils/formatFarcasterFollowingsData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const socialFollowingsQuery = `
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}}
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
`;

const fetchFarcasterFollowings = async (address, existingUsers = []) => {
  let farcasterFollowingsDataResponse;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!farcasterFollowingsDataResponse) {
<strong>      farcasterFollowingsDataResponse = await fetchQueryWithPagination(
</strong>        socialFollowingsQuery,
        {
          user: address,
        }
      );
    }
    const {
      data: farcasterFollowingsData,
      error: farcasterFollowingsError,
      hasNextPage: farcasterFollowingsHasNextPage,
      getNextPage: farcasterFollowingsGetNextPage,
    } = farcasterFollowingsDataResponse ?? {};
    if (!farcasterFollowingsError) {
      const followings =
        farcasterFollowingsData?.SocialFollowings?.Following?.map(
          following => following.followingAddress
        ) ?? [];
      recommendedUsers = [
        ...formatFarcasterFollowingsData(followings, recommendedUsers),
      ];
      if (!farcasterFollowingsHasNextPage) {
        break;
      } else {
        farcasterFollowingsDataResponse = await farcasterFollowingsGetNextPage();
      }
    } else {
      console.error("Error: ", farcasterFollowingsError);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchFarcasterFollowings;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/farcaster_followings.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.farcaster_followings import format_farcaster_followings_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

social_followings_query = """
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}}
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
"""


async def fetch_farcaster_followings(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=social_followings_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            followings = [following['followingAddress'] for following in (res.data.get(
                'SocialFollowings', {}).get('Following', []) or []) if 'followingAddress' in following]
            recommended_users = format_farcaster_followings_data(
                followings,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.3: Fetch Lens Followings Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that is being followed on Lens by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/r4oSCxfvbr" %}
Show all Lens followings of vitalik.eth and check if they're mutual followings
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {
      filter: { identity: { _eq: $user }, dappName: { _eq: lens } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {
            filter: { identity: { _eq: $user }, dappName: { _eq: lens } }
          }
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": ["0x648aa14e4424e0825a5ce739c8c68610e143fb79"],
            "domains": [
              {
                "name": "sassal.isstackingsats.eth",
                "isPrimary": false
              },
              {
                "name": "[2b95ffa321895f770d6cf4f5a0a28b503775a5791b879f7fd7dc8be4d2119539].ethmojis.eth",
                "isPrimary": false
              },
              {
                "name": "thedailygwei.eth",
                "isPrimary": false
              },
              {
                "name": "[77a1a3bec9ef5f3ae2bb016067fb690ea5db004f01c5628c015b15c0c2954b1c].ethmojis.eth",
                "isPrimary": false
              },
              {
                "name": "[799a91224d75d9f60ff17c9704dff211ac00d58d9c0f929f9bd0c972dc1d9e1b].ethmojis.eth",
                "isPrimary": false
              },
              {
                "name": "sassal.eth",
                "isPrimary": true
              },
              {
                "name": "[61a23a96d60aa46f53bd6ea7db88aa1ccca962bf18d4f3fbeef46aa611783032].ethmojis.eth",
                "isPrimary": false
              },
              {
                "name": "thedailygwei.mirror.xyz",
                "isPrimary": false
              },
              {
                "name": "sassal.ismoney.eth",
                "isPrimary": false
              },
              {
                "name": "sassal.defi⚡.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "blockchain": "polygon",
                "profileName": "lens/@sassal",
                "profileImage": "https://ipfs.infura.io/ipfs/QmbDWyw6b1YfpTkPhWySzfr7zrdwT1TNTA8Hbk1S9JoRrp",
                "profileTokenId": "13464",
                "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
              },
              {
                "dappName": "farcaster",
                "blockchain": "optimism",
                "profileName": "",
                "profileImage": "",
                "profileTokenId": "21566",
                "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b"
              },
              {
                "dappName": "farcaster",
                "blockchain": "optimism",
                "profileName": "sassal.eth",
                "profileImage": "https://i.imgur.com/J81m7He.jpg",
                "profileTokenId": "4036",
                "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b"
              }
            ],
            "xmtp": null,
            "mutualFollower": {
              "Follower": null
            }
          }
        }
        // more Lens following data
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that is being followed on Lens by the given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatLensFollowingsData.js" overflow="wrap" %}
```javascript
function formatLensFollowingsData(followings, existingUser = []) {
  const recommendedUsers = [...existingUser];
  for (const following of followings) {
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          following.addresses?.includes?.(address)
        )
    );

    const followsBack = Boolean(following?.mutualFollower?.Follower?.[0]);
    if (existingUserIndex !== -1) {
      const follows = recommendedUsers?.[existingUserIndex]?.follows ?? {};
      recommendedUsers[existingUserIndex] = {
        ...following,
        ...recommendedUsers[existingUserIndex],
        follows: {
          ...follows,
          followingOnLens: true,
          followedOnLens: followsBack,
        },
      };
    } else {
      recommendedUsers.push({
        ...following,
        follows: {
          followingOnLens: true,
          followedOnLens: followsBack,
        },
      });
    }
  }
  return recommendedUsers;
}

export default formatLensFollowingsData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/lens_followings.py" %}
```python
def format_lens_followings_data(followings, existing_user=None):
    if existing_user is None:
        existing_user = []

    recommended_users = existing_user.copy()
    for following in followings:
        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(address in recommended_user_addresses for address in following.get('addresses', [])):
                existing_user_index = index
                break

        mutual_follower = following.get('mutualFollower', {})
        follower = mutual_follower.get('Follower', []) if mutual_follower is not None else []
        follows_back = bool(follower[0]) if follower else False

        if existing_user_index != -1:
            follows = recommended_users[existing_user_index].get('follows', {})
            recommended_users[existing_user_index].update({
                **following,
                'follows': {
                    **follows,
                    'followingOnLens': True,
                    'followedOnLens': follows_back
                }
            })
        else:
            recommended_users.append({
                **following,
                'follows': {
                    'followingOnLens': True,
                    'followedOnLens': follows_back
                }
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0x648aa14e4424e0825a5ce739c8c68610e143fb79"],
    "domains": [
      { "name": "sassal.isstackingsats.eth", "isPrimary": false },
      {
        "name": "[2b95ffa321895f770d6cf4f5a0a28b503775a5791b879f7fd7dc8be4d2119539].ethmojis.eth",
        "isPrimary": false
      },
      { "name": "thedailygwei.eth", "isPrimary": false },
      {
        "name": "[77a1a3bec9ef5f3ae2bb016067fb690ea5db004f01c5628c015b15c0c2954b1c].ethmojis.eth",
        "isPrimary": false
      },
      {
        "name": "[799a91224d75d9f60ff17c9704dff211ac00d58d9c0f929f9bd0c972dc1d9e1b].ethmojis.eth",
        "isPrimary": false
      },
      { "name": "sassal.eth", "isPrimary": true },
      {
        "name": "[61a23a96d60aa46f53bd6ea7db88aa1ccca962bf18d4f3fbeef46aa611783032].ethmojis.eth",
        "isPrimary": false
      },
      { "name": "thedailygwei.mirror.xyz", "isPrimary": false },
      { "name": "sassal.ismoney.eth", "isPrimary": false },
      { "name": "sassal.defi⚡.eth", "isPrimary": false }
    ],
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "",
        "profileImage": "",
        "profileTokenId": "21566",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      },
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@sassal",
        "profileImage": "",
        "profileTokenId": "13464",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      },
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "sassal.eth",
        "profileImage": "https://i.imgur.com/J81m7He.jpg",
        "profileTokenId": "4036",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      }
    ],
    "xmtp": null,
    "mutualFollower": { "Follower": null },
    // shows vitalik.eth following this user, but not followed back on Lens
<strong>    "follows": { "followingOnLens": true, "followedOnLens": false }
</strong>  }
]
</code></pre>

**Iterate to fetch all users being followed on Lens**

With the queries for fetching all the users being followed on Lens of a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize [`fetchQueryWithPagination`](../nodejs-sdk-reference/fetchquery.md) and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="functions/fetchLensFollowings.js" %}
```javascript
import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatLensFollowingsData from "../utils/formatLensFollowingsData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const socialFollowingsQuery = `
query MyQuery($user: Identity!) {
    SocialFollowings(
      input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
    ) {
      Following {
        followingAddress {
          addresses
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
          mutualFollower: socialFollowers(
            input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}}
          ) {
            Follower {
              followerAddress {
                socials {
                  profileName
                }
              }
            }
          }
        }
      }
    }
  }
`;

const fetchLensFollowings = async (address, existingUsers = []) => {
  let res;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!res) {
      res = await fetchQueryWithPagination(socialFollowingsQuery, {
        user: address,
      });
    }
    const { data, error, hasNextPage, getNextPage } = res ?? {};
    if (!error) {
      const followings =
        data?.SocialFollowings?.Following?.map(
          (following) => following.followingAddress
        ) ?? [];
      recommendedUsers = [
        ...formatLensFollowingsData(followings, recommendedUsers),
      ];
      if (!hasNextPage) {
        break;
      } else {
        res = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchLensFollowings;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/lens_followings.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.lens_followings import format_lens_followings_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

social_followings_query = """
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}}
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
"""


async def fetch_lens_followings(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=social_followings_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            followings = [following['followingAddress'] for following in (res.data.get(
                'SocialFollowings', {}).get('Following', []) or []) if 'followingAddress' in following]
            recommended_users = format_lens_followings_data(
                followings,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.4: Fetch Farcaster Followers Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that is following a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), on Farcaster and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/Rv0kUYb0ks" %}
Show all Farcaster followers of vitalik.eth and check if they're mutual followers
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: $user }, dappName: { _eq: farcaster } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollowing: socialFollowings(
          input: {
            filter: { identity: { _eq: $user }, dappName: { _eq: farcaster } }
          }
        ) {
          Following {
            followingAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xa298b2a89621f9a43b75efae8c7411deeeedea59",
              "0x02d64289bbe12d96e53957fe33a9b1373aa0da40"
            ],
            "domains": [
              {
                "name": "gaoa.eth",
                "isPrimary": true
              },
              {
                "name": "accordplace.eth",
                "isPrimary": false
              },
              {
                "name": "knowledgeworker.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "blockchain": "optimism",
                "profileName": "gaoa.eth",
                "profileImage": "https://i.imgur.com/KRZPQnq.png",
                "profileTokenId": "9582",
                "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b"
              }
            ],
            "xmtp": null,
            "mutualFollowing": {
              "Following": null
            }
          }
        }
        // more Farcaster followers
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that is following a given user on Farcaster:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatFarcasterFollowersData.js" %}
```javascript
function formatFarcasterFollowersData(followers, existingUser = []) {
  const recommendedUsers = [...existingUser];

  for (const follower of followers) {
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          follower.addresses?.includes?.(address)
        )
    );

    const following = Boolean(follower?.mutualFollowing?.Following?.length);

    if (existingUserIndex !== -1) {
      const follows = recommendedUsers?.[existingUserIndex]?.follows ?? {};

      follows.followedOnFarcaster = true;
      follows.followingOnFarcaster = follows.followingOnFarcaster || following;

      recommendedUsers[existingUserIndex] = {
        ...follower,
        ...recommendedUsers[existingUserIndex],
        follows,
      };
    } else {
      recommendedUsers.push({
        ...follower,
        follows: {
          followingOnFarcaster: following,
          followedOnFarcaster: true,
        },
      });
    }
  }
  return recommendedUsers;
}

export default formatFarcasterFollowersData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/farcaster_followers.py" %}
```python
def format_farcaster_followers_data(followers, existing_user=None):
    if existing_user is None:
        existing_user = []

    recommended_users = existing_user.copy()

    for follower in followers:
        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(address in follower.get('addresses', []) for address in recommended_user_addresses):
                existing_user_index = index
                break

        following = bool(follower.get('mutualFollower', {}).get('Following'))

        if existing_user_index != -1:
            follows = recommended_users[existing_user_index].get('follows', {})

            follows['followedOnFarcaster'] = True
            follows['followingOnFarcaster'] = follows.get(
                'followingOnFarcaster', False) or following

            recommended_users[existing_user_index].update({
                **follower,
                'follows': follows
            })
        else:
            recommended_users.append({
                **follower,
                'follows': {
                    'followingOnFarcaster': following,
                    'followedOnFarcaster': True
                }
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xca9af28a4e0a73a74fd481a00d1145130f17586d"],
    "domains": null,
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "thessy",
        "profileImage": "https://i.imgur.com/7xIpCuE.jpg",
        "profileTokenId": "5910",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      }
    ],
    "xmtp": null,
    "mutualFollowing": { "Following": null },
    // show vitalik.eth being followed by this user, but not following back on Farcaster
<strong>    "follows": { "followingOnFarcaster": false, "followedOnFarcaster": true }
</strong>  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all users following on Farcaster**

With the queries for fetching all the users following on Farcaster of a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize [`fetchQueryWithPagination`](../nodejs-sdk-reference/fetchquerywithpagination.md) and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchFarcasterFollowers.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatFarcasterFollowersData from "./utils/formatFarcasterFollowersData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const socialFollowersQuery = `
query MyQuery($user: Identity!) {
  SocialFollowers(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollowing: socialFollowings(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}}
        ) {
          Following {
            followingAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
`;

const fetchFarcasterFollowers = async (address, existingUsers = []) => {
  let res;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!res) {
<strong>      res = await fetchQueryWithPagination(socialFollowersQuery, {
</strong>        user: address,
      });
    }
    const { data, error, hasNextPage, getNextPage } = res ?? {};
    if (!error) {
      const followings =
        data?.SocialFollowers?.Follower?.map(
          (follower) => follower.followerAddress
        ) ?? [];
      recommendedUsers = [
        ...formatFarcasterFollowersData(followings, recommendedUsers),
      ];
      if (!hasNextPage) {
        break;
      } else {
        res = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchFarcasterFollowers;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/farcaster_followers.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.farcaster_followers import format_farcaster_followers_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

social_followers_query = """
query MyQuery($user: Identity!) {
  SocialFollowers(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollowing: socialFollowings(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: farcaster}}}
        ) {
          Following {
            followingAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
"""


async def fetch_farcaster_followers(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=social_followers_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            followings = [following['followerAddress'] for following in (res.data.get(
                'SocialFollowers', {}).get('Follower', []) or []) if 'followerAddress' in following]
            recommended_users = format_farcaster_followers_data(
                followings,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.5: Fetch Lens Followers Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that is following a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), on Lens and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/0NZU9Mrb8m" %}
Show all Lens followers of vitalik.eth and check if they're mutual followers
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  SocialFollowers(
    input: {
      filter: { identity: { _eq: $user }, dappName: { _eq: lens } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollowing: socialFollowings(
          input: {
            filter: { identity: { _eq: $user }, dappName: { _eq: lens } }
          }
        ) {
          Following {
            followingAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that is following a given user on Lens:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatLensFollowersData.js" %}
```javascript
function formatLensFollowersData(followers, existingUser = []) {
  const recommendedUsers = [...existingUser];

  for (const follower of followers) {
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          follower.addresses?.includes?.(address)
        )
    );

    const following = Boolean(follower?.mutualFollower?.Following?.length);

    if (existingUserIndex !== -1) {
      const follows = recommendedUsers?.[existingUserIndex]?.follows ?? {};

      follows.followedOnLens = true;
      follows.followingOnLens = follows.followingOnLens || following;

      recommendedUsers[existingUserIndex] = {
        ...follower,
        ...recommendedUsers[existingUserIndex],
        follows,
      };
    } else {
      recommendedUsers.push({
        ...follower,
        follows: {
          followingOnLens: following,
          followedOnLens: true,
        },
      });
    }
  }
  return recommendedUsers;
}

export default formatLensFollowersData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/lens_followers.py" %}
```python
def format_lens_followers_data(followers, existing_user=None):
    if existing_user is None:
        existing_user = []

    recommended_users = existing_user.copy()

    for follower in followers:
        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(address in follower.get('addresses', []) for address in recommended_user_addresses):
                existing_user_index = index
                break

        following = bool(follower.get('mutualFollower', {}).get('Following'))

        if existing_user_index != -1:
            follows = recommended_users[existing_user_index].get('follows', {})

            follows['followedOnLens'] = True
            follows['followingOnLens'] = follows.get(
                'followingOnLens', False) or following

            recommended_users[existing_user_index].update({
                **follower,
                'follows': follows
            })
        else:
            recommended_users.append({
                **follower,
                'follows': {
                    'followingOnLens': following,
                    'followedOnLens': True
                }
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0x55b0c2662fe08f265e658ce235151b689d5e120c"],
    "domains": null,
    "socials": [
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@huxley_warner",
        "profileImage": "",
        "profileTokenId": "8818",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      }
    ],
    "xmtp": [{ "isXMTPEnabled": true }],
    "mutualFollowing": { "Following": null },
    // show vitalik.eth followed by this user on Lens, but not following back
<strong>    "follows": { "followingOnLens": false, "followedOnLens": true }
</strong>  },
  // more onchain graph users
]
</code></pre>

**Iterate to fetch all users following on Lens**

With the queries for fetching all the users following on Lens of a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize [`fetchQueryWithPagination`](../nodejs-sdk-reference/fetchquerywithpagination.md) and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="function/fetchLensFollowers.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatLensFollowersData from "./utils/formatLensFollowersData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const socialFollowingsQuery = `
query MyQuery($user: Identity!) {
  SocialFollowers(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollowing: socialFollowings(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}}
        ) {
          Following {
            followingAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
`;

const fetchLensFollowers = async (address, existingUsers = []) => {
  let res;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!res) {
<strong>      res = await fetchQueryWithPagination(socialFollowingsQuery, {
</strong>        user: address,
      });
    }
    const { data, error, hasNextPage, getNextPage } = res ?? {};
    if (!error) {
      const followings =
        data?.SocialFollowers?.Follower?.map(
          (follower) => follower.followerAddress
        ) ?? [];
      recommendedUsers = [
        ...formatLensFollowersData(followings, recommendedUsers),
      ];
      if (!hasNextPage) {
        break;
      } else {
        res = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchLensFollowers;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/lens_followers.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.lens_followings import format_lens_followings_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="fdbab79bfe44463497a990d62d90495a")

social_followings_query = """
query MyQuery($user: Identity!) {
  SocialFollowings(
    input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
        mutualFollower: socialFollowers(
          input: {filter: {identity: {_eq: $user}, dappName: {_eq: lens}}}
        ) {
          Follower {
            followerAddress {
              socials {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
"""


async def fetch_lens_followers(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=social_followings_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            followings = [following['followingAddress'] for following in (res.data.get(
                'SocialFollowings', {}).get('Following', []) or []) if 'followingAddress' in following]
            recommended_users = format_lens_followings_data(
                followings,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.6: Fetch Token Transfers Sent Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that received token transfers **sent from a given user,** e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/IhG0IQGdBv" %}
Show me token transfers from vitalik.eth on Ethereum, Polygon, and Base
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  Ethereum: TokenTransfers(
    input: {
      filter: { from: { _eq: $user } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenTransfer {
      account: from {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: {
      filter: { from: { _eq: $user } }
      blockchain: polygon
      limit: 200
    }
  ) {
    TokenTransfer {
      account: from {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
    input: {
      filter: { from: { _eq: $user } }
      blockchain: base
      limit: 200
    }
  ) {
    TokenTransfer {
      account: from {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "account": {
            "addresses": ["0xd8b75eb7bd778ac0b3f5ffad69bcc2e25bccac95"],
            "domains": [
              {
                "name": "toastmybread.eth",
                "isPrimary": true
              },
              {
                "name": "daerbymtsaot.eth",
                "isPrimary": false
              }
            ],
            "socials": null,
            "xmtp": null
          }
        }
        // more Ethereum token transfers from vitalik.eth
      ]
    },
    "Polygon": {
      "TokenTransfer": [
        {
          "account": {
            "addresses": ["0xd8b75eb7bd778ac0b3f5ffad69bcc2e25bccac95"],
            "domains": [
              {
                "name": "toastmybread.eth",
                "isPrimary": true
              },
              {
                "name": "daerbymtsaot.eth",
                "isPrimary": false
              }
            ],
            "socials": null,
            "xmtp": null
          }
        }
        // more Polygon token transfers from vitalik.eth
      ],
    }
    "Base": {
      "TokenTransfer": [
        {
          "account": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              {
                "name": "7860000.eth",
                "isPrimary": false
              },
              {
                "name": "offchainexample.eth",
                "isPrimary": false
              },
              {
                "name": "brianshaw.eth",
                "isPrimary": false
              },
              {
                "name": "vbuterin.stateofus.eth",
                "isPrimary": false
              },
              {
                "name": "quantumsmartcontracts.eth",
                "isPrimary": false
              },
              {
                "name": "Vitalik.eth",
                "isPrimary": false
              },
              {
                "name": "openegp.eth",
                "isPrimary": false
              },
              {
                "name": "vitalik.cannafam.eth",
                "isPrimary": false
              },
              {
                "name": "VITALIK.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "blockchain": "optimism",
                "profileName": "vitalik.eth",
                "profileImage": "https://i.imgur.com/gF9Yaeg.jpg",
                "profileTokenId": "5650",
                "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b"
              },
              {
                "dappName": "lens",
                "blockchain": "polygon",
                "profileName": "lens/@vitalik",
                "profileImage": "ipfs://QmQP1DyNH8upeBxKJYtfCDdUj3mRcZep8zhJTLe3ePXB7M",
                "profileTokenId": "100275",
                "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // more Base token transfers from vitalik.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that received token transfers sent from a given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatTokenSentData.js" %}
```javascript
const formatTokenSentData = (data, _recommendedUsers = []) => {
  const recommendedUsers = [..._recommendedUsers];

  for (const transfer of data) {
    const { addresses = [] } = transfer || {};
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );
    const _tokenTransfers = {
      sent: true,
    };
    if (existingUserIndex !== -1) {
      const _addresses = recommendedUsers?.[existingUserIndex]?.addresses || [];
      recommendedUsers[existingUserIndex].addresses = [
        ..._addresses,
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      recommendedUsers[existingUserIndex].tokenTransfers = {
        ...(recommendedUsers?.[existingUserIndex]?.tokenTransfers ?? {}),
        ..._tokenTransfers,
      };
    } else {
      recommendedUsers.push({
        ...transfer,
        tokenTransfers: {
          ..._tokenTransfers,
        },
      });
    }
  }

  return recommendedUsers;
};

export default formatTokenSentData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/token_sent.py" %}
```python
def format_token_sent_data(data, recommended_users=None):
    if recommended_users is None:
        recommended_users = []

    for transfer in data:
        addresses = transfer.get('addresses', [])
        existing_user_index = next((index for index, recommended_user in enumerate(recommended_users)
                                    if any(address in recommended_user.get('addresses', []) for address in addresses)), -1)

        token_transfers = {'sent': True}

        if existing_user_index != -1:
            existing_addresses = recommended_users[existing_user_index].get(
                'addresses', [])
            unique_addresses = list(set(existing_addresses + addresses))
            recommended_users[existing_user_index]['addresses'] = unique_addresses
            existing_token_transfers = recommended_users[existing_user_index].get(
                'tokenTransfers', {})
            recommended_users[existing_user_index]['tokenTransfers'] = {
                **existing_token_transfers, **token_transfers}
        else:
            recommended_users.append(
                {**transfer, 'tokenTransfers': token_transfers})

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">{
  "addresses": ["0xd8b75eb7bd778ac0b3f5ffad69bcc2e25bccac95"],
  "primaryDomain": { "name": "toastmybread.eth" },
  "domains": [
    { "name": "toastmybread.eth", "isPrimary": true },
    { "name": "daerbymtsaot.eth", "isPrimary": false }
  ],
  "socials": null,
  "xmtp": null,
  // This user receive tokens sent by vitalik.eth
<strong>  "tokenTransfers": { "sent": true }
</strong>}
</code></pre>

**Iterate to fetch all users that received token transfers sent from a given user**

With the queries for fetching all the users that received token transfers sent from a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchTokenSent.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatTokenSentData from "./utils/formatTokenSentData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const tokenSentQuery = `
query TokenSent($user: Identity!) {
    Ethereum: TokenTransfers(
      input: {filter: {from: {_eq: $user}}, blockchain: ethereum, limit: 200}
    ) {
      TokenTransfer {
        account: to {
          addresses
          primaryDomain {
            name
          }
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
    Polygon: TokenTransfers(
      input: {filter: {from: {_eq: $user}}, blockchain: polygon, limit: 200}
    ) {
      TokenTransfer {
        account: to {
          addresses
          primaryDomain {
            name
          }
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
    Base: TokenTransfers(
      input: {filter: {from: {_eq: $user}}, blockchain: base, limit: 200}
    ) {
      TokenTransfer {
        account: to {
          addresses
          primaryDomain {
            name
          }
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
  }
`;

const fetchTokenSent = async (address, existingUsers = []) => {
  let res;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!res) {
<strong>      res = await fetchQueryWithPagination(tokenSentQuery, {
</strong>        user: address,
      });
    }
    const { data, error, hasNextPage, getNextPage } = res ?? {};
    if (!error) {
      const ethData = (data?.Ethereum?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );
      const polygonData = (data?.Polygon?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );
      const baseData = (data?.Base?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );

      const tokenTransfer = [...ethData, ...polygonData, ...baseData];
      recommendedUsers = [
        ...formatTokenSentData(tokenTransfer, recommendedUsers),
      ];
      if (!hasNextPage) {
        break;
      } else {
        res = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchTokenSent;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/token_sent.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.token_sent import format_token_sent_data

# t your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

token_sent_query = """
query MyQuery($user: Identity!) {
  Ethereum: TokenTransfers(
    input: {filter: {from: {_eq: $user}}, blockchain: ethereum, limit: 200}
  ) {
    TokenTransfer {
      account: from {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: {filter: {from: {_eq: $user}}, blockchain: ethereum, limit: 200}
  ) {
    TokenTransfer {
      account: from {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
      input: {filter: {from: {_eq: $user}}, blockchain: base, limit: 200}
    ) {
      TokenTransfer {
        account: to {
          addresses
          primaryDomain {
            name
          }
          domains {
            name
            isPrimary
          }
          socials {
            dappName
            blockchain
            profileName
            profileImage
            profileTokenId
            profileTokenAddress
          }
          xmtp {
            isXMTPEnabled
          }
        }
      }
    }
}
"""


async def fetch_token_sent(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=token_sent_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            eth_data = [transfer['account'] for transfer in (res.data.get('Ethereum', {}).get(
                'TokenTransfer', []) if res.data and 'Ethereum' in res.data and 'TokenTransfer' in res.data['Ethereum'] else [])]
            polygon_data = [transfer['account'] for transfer in (res.data.get('Polygon', {}).get(
                'TokenTransfer', []) if res.data and 'Polygon' in res.data and 'TokenTransfer' in res.data['Polygon'] else [])]
            base_data = [transfer['account'] for transfer in (res.data.get('Base', {}).get(
                'TokenTransfer', []) if res.data and 'Base' in res.data and 'TokenTransfer' in res.data['Base'] else [])]
            token_transfer = eth_data + polygon_data + base_data
            recommended_users = format_token_sent_data(
                token_transfer,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.7: Fetch Token Transfers Received Data

You can use [Airstack](https://airstack.xyz) to easily fetch all the users that sent token transfers to and **received by a given user**, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), and get their 0x addresses, ENS domains, Lens, Farcaster, and XMTP:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/a3EjaA49Gd" %}
Show me token transfers received by vitalik.eth on Ethereum, Polygon, and Base
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  Ethereum: TokenTransfers(
    input: { filter: { to: { _eq: $user } }, blockchain: ethereum, limit: 200 }
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: { filter: { to: { _eq: $user } }, blockchain: polygon, limit: 200 }
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
    input: { filter: { to: { _eq: $user } }, blockchain: base, limit: 200 }
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "account": {
            "addresses": ["0x92c6b6b4a1817e76b56eb3e1724f9df6026dd63c"],
            "domains": [
              {
                "name": "iamonlyanartist.eth",
                "isPrimary": true
              }
            ],
            "socials": null,
            "xmtp": null
          }
        }
        // more tokens received by vitalik.eth on Ethereum
      ],
      "Polygon": [
        {
          "account": {
            "addresses": ["0x0aa843796ff888f06f5d609c9d6339364d138752"],
            "domains": [
              {
                "name": "abc-d.eth",
                "isPrimary": false
              },
              {
                "name": "orent.eth",
                "isPrimary": false
              },
              {
                "name": "0xstranger.eth",
                "isPrimary": true
              }
            ],
            "socials": null,
            "xmtp": null
          }
        }
        // more tokens received by vitalik.eth on Polygon
      ]
    },
    "Base": {
      "TokenTransfer": [
        {
          "account": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              {
                "name": "7860000.eth",
                "isPrimary": false
              },
              {
                "name": "offchainexample.eth",
                "isPrimary": false
              },
              {
                "name": "brianshaw.eth",
                "isPrimary": false
              },
              {
                "name": "vbuterin.stateofus.eth",
                "isPrimary": false
              },
              {
                "name": "quantumsmartcontracts.eth",
                "isPrimary": false
              },
              {
                "name": "Vitalik.eth",
                "isPrimary": false
              },
              {
                "name": "openegp.eth",
                "isPrimary": false
              },
              {
                "name": "vitalik.cannafam.eth",
                "isPrimary": false
              },
              {
                "name": "VITALIK.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "blockchain": "optimism",
                "profileName": "vitalik.eth",
                "profileImage": "https://i.imgur.com/gF9Yaeg.jpg",
                "profileTokenId": "5650",
                "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b"
              },
              {
                "dappName": "lens",
                "blockchain": "polygon",
                "profileName": "lens/@vitalik",
                "profileImage": "ipfs://QmQP1DyNH8upeBxKJYtfCDdUj3mRcZep8zhJTLe3ePXB7M",
                "profileTokenId": "100275",
                "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // more tokens received by vitalik.eth on Base
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that sent token transfers to a given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatTokenReceivedData.js" %}
```javascript
const formatTokenReceivedData = (data, _recommendedUsers = []) => {
  const recommendedUsers = [..._recommendedUsers];

  for (const transfer of data) {
    const { addresses = [] } = transfer || {};
    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );
    const _tokenTransfers = {
      received: true,
    };
    if (existingUserIndex !== -1) {
      const _addresses = recommendedUsers?.[existingUserIndex]?.addresses || [];
      recommendedUsers[existingUserIndex].addresses = [
        ..._addresses,
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      recommendedUsers[existingUserIndex].tokenTransfers = {
        ...(recommendedUsers?.[existingUserIndex]?.tokenTransfers ?? {}),
        ..._tokenTransfers,
      };
    } else {
      recommendedUsers.push({
        ...transfer,
        tokenTransfers: {
          ..._tokenTransfers,
        },
      });
    }
  }

  return recommendedUsers;
};

export default formatTokenReceivedData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/token_received.py" %}
```python
def format_token_received_data(data, _recommended_users=None):
    if _recommended_users is None:
        _recommended_users = []

    recommended_users = _recommended_users.copy()

    for transfer in data:
        addresses = transfer.get('addresses', []) if transfer else []
        existing_user_index = -1

        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(address in recommended_user_addresses for address in addresses):
                existing_user_index = index
                break

        _token_transfers = {'received': True}

        if existing_user_index != -1:
            _addresses = recommended_users[existing_user_index].get(
                'addresses', [])
            new_addresses = list(set(_addresses + addresses))
            recommended_users[existing_user_index]['addresses'] = new_addresses
            existing_token_transfers = recommended_users[existing_user_index].get(
                'tokenTransfers', {})
            recommended_users[existing_user_index]['tokenTransfers'] = {
                **existing_token_transfers, **_token_transfers}
        else:
            new_user = transfer.copy() if transfer else {}
            new_user['tokenTransfers'] = _token_transfers
            recommended_users.append(new_user)

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
    "domains": [
      { "name": "quantumexchange.eth", "isPrimary": false },
      { "name": "7860000.eth", "isPrimary": false },
      { "name": "offchainexample.eth", "isPrimary": false },
      { "name": "brianshaw.eth", "isPrimary": false },
      { "name": "vbuterin.stateofus.eth", "isPrimary": false },
      { "name": "quantumsmartcontracts.eth", "isPrimary": false },
      { "name": "Vitalik.eth", "isPrimary": false },
      { "name": "openegp.eth", "isPrimary": false },
      { "name": "vitalik.cannafam.eth", "isPrimary": false },
      { "name": "VITALIK.eth", "isPrimary": false }
    ],
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "vitalik.eth",
        "profileImage": "https://i.imgur.com/gF9Yaeg.jpg",
        "profileTokenId": "5650",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      },
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@vitalik",
        "profileImage": "",
        "profileTokenId": "100275",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      }
    ],
    "xmtp": [{ "isXMTPEnabled": true }],
    // show that vitalik.eth received token transfers sent by this user
<strong>    "tokenTransfers": { "received": true }
</strong>  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all users that sent token transfers to a given user**

With the queries for fetching all the users that sent token transfers to a given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchTokenReceived.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatTokenReceivedData from "./utils/formatTokenReceivedData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const tokenReceivedQuery = `
query MyQuery($user: Identity!) {
  Ethereum: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: ethereum, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: polygon, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: base, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
`;

const fetchTokenReceived = async (address, existingUsers = []) => {
  let res;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!res) {
<strong>      res = await fetchQueryWithPagination(tokenReceivedQuery, {
</strong>        user: address,
      });
    }
    const { data, error, hasNextPage, getNextPage } = res ?? {};
    if (!error) {
      const ethData = (data?.Ethereum?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );
      const polygonData = (data?.Polygon?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );
      const baseData = (data?.Base?.TokenTransfer ?? []).map(
        (transfer) => transfer.account
      );

      const tokenTransfer = [...ethData, ...polygonData, ...baseData];
      recommendedUsers = [
        ...formatTokenReceivedData(tokenTransfer, recommendedUsers),
      ];
      if (!hasNextPage) {
        break;
      } else {
        res = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchTokenReceived;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/token_received.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.token_received import format_token_received_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

token_received_query = """
query MyQuery($user: Identity!) {
  Ethereum: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: ethereum, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: polygon, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
    input: {filter: {to: {_eq: $user}}, blockchain: base, limit: 200}
  ) {
    TokenTransfer {
      account: to {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
"""


async def fetch_token_received(address, existing_users=[]):
    res = None
    recommended_users = existing_users.copy()
    while True:
        if res is None:
            execute_query_client = api_client.create_execute_query_object(
                query=token_received_query, variables={'user': address})
<strong>            res = await execute_query_client.execute_paginated_query()
</strong>
        if res.error is None:
            eth_data = [transfer['account'] for transfer in (res.data.get('Ethereum', {}).get(
                'TokenTransfer', []) if res.data and 'Ethereum' in res.data and 'TokenTransfer' in res.data['Ethereum'] else [])]
            polygon_data = [transfer['account'] for transfer in (res.data.get('Polygon', {}).get(
                'TokenTransfer', []) if res.data and 'Polygon' in res.data and 'TokenTransfer' in res.data['Polygon'] else [])]
            base_data = [transfer['account'] for transfer in (res.data.get('Base', {}).get(
                'TokenTransfer', []) if res.data and 'Base' in res.data and 'TokenTransfer' in res.data['Base'] else [])]
            token_transfer = eth_data + polygon_data + base_data
            recommended_users = format_token_received_data(
                token_transfer,
                recommended_users
            )

            if not res.has_next_page:
                break
            else:
                res = await res.get_next_page
        else:
            print("Error: ", res.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.8: Fetch Common Ethereum NFT Holders Data

In order to fetch the common Ethereum holders that hold the Ethereum NFTs hold by a given user, it will require 2 steps:

1. [Fetch all Ethereum NFTs owned by a user](onchain-graph.md#fetch-all-ethereum-nfts-owned-by-a-user)
2. [Fetch all Ethereum NFT owners](onchain-graph.md#fetch-all-ethereum-nft-owners)

**Fetch all Ethereum NFTs owned by a user**

You can use Airstack to fetch all the NFTs that are hold by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), on Ethereum:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/IeNR6V1SoE" %}
Show me all Ethereum NFT address owned by vitalik.eth
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  TokenBalances(
    input: {
      filter: { tokenType: { _in: [ERC721] }, owner: { _eq: $user } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
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
          "tokenAddress": "0x2f1d6bba8d2ce2f1b6aed78f797096a6cc9e9fc9"
        },
        {
          "tokenAddress": "0x2f1d6bba8d2ce2f1b6aed78f797096a6cc9e9fc9"
        },
        {
          "tokenAddress": "0x9251dec8df720c2adf3b6f46d968107cbbadf4d4"
        }
        // Other Ethereum NFTs
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, the response can be filtered to only Ethereum NFTs and be formatted into an array of token addresses to be used in the next step:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const tokenAddresses =
  data?.TokenBalances?.TokenBalance?.map((token) => token.tokenAddress) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
token_addresses = [token['tokenAddress'] for token in data.get('TokenBalances', {}).get('TokenBalance', [])] if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances'] else []
```
{% endtab %}
{% endtabs %}

where `data` is the response from the API. The formatted result will have a format as follows:

```json
[
  "0x2f1d6bba8d2ce2f1b6aed78f797096a6cc9e9fc9",
  "0x9251dec8df720c2adf3b6f46d968107cbbadf4d4",
  "0xcde7c3d9629f6bf247b4a4601260bd8fb7554ec6",
  "0x77e2545d1d63856e22ce82e3d6f2a3b2077232bf",
  "0xfbbddd98640eb24732f3c65a0348825055d2d651",
  "0x932261f9fc8da46c4a22e31b45c4de60623848bf",
  "0x160da290a6b1923257705cb05c322ae44ca86ebb",
  "0x0a1e7f376c586e272a3070632c8297c91b1d1b32",
  "0x343f999eaacdfa1f201fb8e43ebb35c99d9ae0c1",
  "0x9fa184c43b00da59b06f2296d509fbb465fb362e",
  "0xf18224ab6479bb1ecb908d4fe7d2c366d49df0fc",
  "0xb365e53b64655476e3c3b7a3e225d8bf2e95f71d",
  "0x7d89b4c0e85634f0587946b0c8370f477c645a80",
  "0x60f80121c31a0d46b5279700f9df786054aa5ee5",
  "0x765c1d9b32bb20c143aeebfe56e6e7f15d2e8af0",
  "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
  "0x8d0802559775c70fb505f22988a4fd4a4f6d3b62",
  "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
  "0x02e9b2389156ee8ed963b1341a69d5f54ada4d82",
  "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
  "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1",
  "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676",
  "0x044ec6ce7e87859eb9d3ca966cadfb7926d0c482",
  "0xd9e4f99ff4582c710686e30efff39776a055039b",
  "0x172750a992eeee819394dcbab0c86dab5f94b557"
  // other Ethereum NFT addresses
]
```

**Fetch all Ethereum NFT owners**

Using the array of token addresses from the first step, you can fetch all Ethereum NFT holders that hold any of the NFTs that the given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), owned on Ethereum:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/6lE9hiUTgD" %}
Show me Ethereum NFT holders of an array of Ethereum NFT addresses
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _in: $tokenAddresses }
        tokenType: { _in: [ERC721] }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "tokenAddresses": [
    "0x2f1d6bba8d2ce2f1b6aed78f797096a6cc9e9fc9",
    "0x9251dec8df720c2adf3b6f46d968107cbbadf4d4",
    "0xcde7c3d9629f6bf247b4a4601260bd8fb7554ec6",
    "0x77e2545d1d63856e22ce82e3d6f2a3b2077232bf",
    "0xfbbddd98640eb24732f3c65a0348825055d2d651",
    "0x932261f9fc8da46c4a22e31b45c4de60623848bf",
    "0x160da290a6b1923257705cb05c322ae44ca86ebb",
    "0x0a1e7f376c586e272a3070632c8297c91b1d1b32",
    "0x343f999eaacdfa1f201fb8e43ebb35c99d9ae0c1",
    "0x9fa184c43b00da59b06f2296d509fbb465fb362e",
    "0xf18224ab6479bb1ecb908d4fe7d2c366d49df0fc",
    "0xb365e53b64655476e3c3b7a3e225d8bf2e95f71d",
    "0x7d89b4c0e85634f0587946b0c8370f477c645a80",
    "0x60f80121c31a0d46b5279700f9df786054aa5ee5",
    "0x765c1d9b32bb20c143aeebfe56e6e7f15d2e8af0",
    "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    "0x8d0802559775c70fb505f22988a4fd4a4f6d3b62",
    "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    "0x02e9b2389156ee8ed963b1341a69d5f54ada4d82",
    "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    "0xb93ee8cdab36199c6debf5bbec53e5908fd8e4e1",
    "0x0a1bbd57033f57e7b6743621b79fcb9eb2ce3676",
    "0x044ec6ce7e87859eb9d3ca966cadfb7926d0c482",
    "0xd9e4f99ff4582c710686e30efff39776a055039b",
    "0x172750a992eeee819394dcbab0c86dab5f94b557"
    // other Ethereum NFT addresses
  ]
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
          "token": {
            "name": "Ethereum Name Service",
            "address": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
            "tokenNfts": [
              {
                "tokenId": "0"
              },
              {
                "tokenId": "1"
              },
              {
                "tokenId": "10"
              },
              {
                "tokenId": "100000033565963492184780382747956449779597849692908939064034980301724777286916"
              },
              {
                "tokenId": "100000053429447445684944795499020314613632708924380550113564120013762803052833"
              },
              {
                "tokenId": "100000096360781137727618610464236537987424530373051124849292369811849840753483"
              },
              {
                "tokenId": "100000114843936327888441444535452908724595399506693879401652018232791665965273"
              },
              {
                "tokenId": "100000125947529619364344205670868761012554267569373365034925463196683339911935"
              },
              {
                "tokenId": "100000139284959947654389656434545974129063100884388571533302285098261179508579"
              },
              {
                "tokenId": "100000171317110418138203615791561215890402521171131044888763796972880825791068"
              }
            ],
            "blockchain": "ethereum",
            "logo": {
              "small": "https://assets.airstack.xyz/image/logo/BQrUBoUPz7YtP+f8AdOKeXhU5q9k47EfLHh6VHoZnGcoMtWouWirBO3gxIG42YCJ/small.png"
            }
          },
          "owner": {
            "addresses": [
              "0xfc1fa39b72b8b83336bac1a3475e5f9f06ebe77f"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        // Other Ethereum NFT holders
      ]
    }
  }
}Try
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that has common Ethereum NFTs with the given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatEthNftData.js" %}
```javascript
const formatEthNftData = (data, _recommendedUsers = []) => {
  const recommendedUsers = [..._recommendedUsers];

  for (const nft of data) {
    const { owner, token } = nft ?? {};
    const { name, logo, address, tokenNfts = [] } = token ?? {};
    const { addresses } = owner ?? {};
    const tokenNft = tokenNfts?.[0];

    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );

    if (existingUserIndex !== -1) {
      const _addresses = recommendedUsers?.[existingUserIndex]?.addresses || [];
      recommendedUsers[existingUserIndex].addresses = [
        ..._addresses,
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      const _nfts = recommendedUsers?.[existingUserIndex]?.nfts || [];
      const nftExists = _nfts.some((nft) => nft.address === address);
      if (!nftExists) {
        _nfts?.push({
          name,
          image: logo?.small,
          blockchain: "ethereum",
          address,
          tokenNfts: tokenNft,
        });
      }
      recommendedUsers[existingUserIndex].nfts = [..._nfts];
    } else {
      recommendedUsers.push({
        ...owner,
        nfts: [
          {
            name,
            image: logo?.small,
            blockchain: "ethereum",
            address,
            tokenNfts: tokenNft,
          },
        ],
      });
    }
  }
  return recommendedUsers;
};

export default formatEthNftData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/ethereum_nft.py" %}
```python
def format_eth_nft_data(data, _recommended_users=None):
    if _recommended_users is None:
        _recommended_users = []

    recommended_users = _recommended_users.copy()

    for nft in data or []:
        owner = nft.get('owner') if nft else {}
        token = nft.get('token') if nft else {}

        name = token.get('name')
        logo = token.get('logo', {})
        address = token.get('address')
        token_nfts = token.get('tokenNfts', [])
        addresses = owner.get('addresses', [])
        token_nft = token_nfts[0] if len(token_nfts) > 0 else None

        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(addr in addresses for addr in recommended_user_addresses):
                existing_user_index = index
                break

        if existing_user_index != -1:
            _addresses = recommended_users[existing_user_index].get('addresses', [])
            _addresses.extend(addresses)
            _addresses = list(set(_addresses))  # Remove duplicates
            recommended_users[existing_user_index]['addresses'] = _addresses

            _nfts = recommended_users[existing_user_index].get('nfts', [])
            nft_exists = any(nft['address'] == address for nft in _nfts)
            if not nft_exists:
                _nfts.append({
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'ethereum',
                    'address': address,
                    'tokenNfts': token_nft
                })
            recommended_users[existing_user_index]['nfts'] = _nfts
        else:
            recommended_users.append({
                **owner,
                'nfts': [{
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'ethereum',
                    'address': address,
                    'tokenNfts': token_nft
                }]
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401"],
    "domains": [
      { "name": "namewrapper.eth", "isPrimary": false },
      { "name": "🐸.lovespepe.eth", "isPrimary": false },
      { "name": "wrapper.ens.eth", "isPrimary": true }
    ],
    "socials": null,
    "xmtp": null,
    // show all common Ethereum NFTs that is also owned by vitalik.eth
<strong>    "nfts": [
</strong>      {
        "name": "Ethereum Name Service",
        "image": "https://assets.airstack.xyz/image/logo/BQrUBoUPz7YtP+f8AdOKeXhU5q9k47EfLHh6VHoZnGcoMtWouWirBO3gxIG42YCJ/small.png",
        "blockchain": "ethereum",
        "address": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
        "tokenNfts": { "tokenId": "0" }
      },
      // other NFTs
    ]
  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all common Ethereum NFT holders**

With the queries for fetching all the common Ethereum NFT holders that holds the same Ethereum NFTs as the given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize [`fetchQueryWithPagination`](../nodejs-sdk-reference/fetchquerywithpagination.md) and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/fetchEthNft.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatEthNftData from "./utils/formatEthNftData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const nftAddressesQuery = `
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
`;

const nftQuery = `
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
`;

const fetchEthNft = async (address, existingUsers = []) => {
  let ethNftDataResponse;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!ethNftDataResponse) {
      // Pagination #1: Fetch Ethereum NFTs
<strong>      ethNftDataResponse = await fetchQueryWithPagination(nftAddressesQuery, {
</strong>        user: address,
      });
    }
    const {
      data: ethNftData,
      error: ethNftError,
      hasNextPage: ethNftHasNextPage,
      getNextPage: ethNftGetNextPage,
    } = ethNftDataResponse ?? {};
    if (!ethNftError) {
      const tokenAddresses =
        ethNftData?.TokenBalances?.TokenBalance?.map(
          (token) => token.tokenAddress
        ) ?? [];
      let ethNftHoldersDataResponse;
      while (true) {
        if (tokenAddresses.length === 0) break;
        if (!ethNftHoldersDataResponse) {
          // Pagination #2: Fetch Ethereum NFT Holders
<strong>          ethNftHoldersDataResponse = await fetchQueryWithPagination(nftQuery, {
</strong>            tokenAddresses,
          });
        }
        const {
          data: ethNftHoldersData,
          error: ethNftHoldersError,
          hasNextPage: ethNftHoldersHasNextPage,
          getNextPage: ethNftHoldersGetNextPage,
        } = ethNftHoldersDataResponse;
        if (!ethNftHoldersError) {
          recommendedUsers = [
            ...formatEthNftData(
              ethNftHoldersData?.TokenBalances?.TokenBalance,
              recommendedUsers
            ),
          ];
          if (!ethNftHoldersHasNextPage) {
            break;
          } else {
            ethNftHoldersDataResponse = await ethNftHoldersGetNextPage();
          }
        } else {
          console.error("Error: ", ethNftHoldersError);
          break;
        }
      }
      if (!ethNftHasNextPage) {
        break;
      } else {
        ethNftDataResponse = await ethNftGetNextPage();
      }
    } else {
      console.error("Error: ", ethNftError);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchEthNft;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/ethereum_nfts.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.ethereum_nft import format_eth_nft_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

nft_addresses_query = """
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
"""

nft_query = """
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
"""


async def fetch_eth_nft(address, existing_users=[]):
    eth_nft_response = None
    recommended_users = existing_users.copy()
    while True:
        if eth_nft_response is None:
            execute_query_client = api_client.create_execute_query_object(
                query=nft_addresses_query, variables={'user': address})
            # Pagination #1: Fetch Ethereum NFTs
<strong>            eth_nft_response = await execute_query_client.execute_paginated_query()
</strong>
        if eth_nft_response.error is None:
            token_addresses = [token['tokenAddress'] for token in eth_nft_response.data.get('TokenBalances', {}).get(
                'TokenBalance', [])] if eth_nft_response.data and 'TokenBalances' in eth_nft_response.data and 'TokenBalance' in eth_nft_response.data['TokenBalances'] else []
            eth_nft_holders_response = None
            while True:
                if eth_nft_holders_response is None:
                    execute_query_client = api_client.create_execute_query_object(
                        query=nft_query, variables={'tokenAddresses': token_addresses})
                    # Pagination #2: Fetch Ethereum NFT Holders
<strong>                    eth_nft_holders_response = await execute_query_client.execute_paginated_query()
</strong>
                if eth_nft_holders_response.error is None:
                    recommended_users = format_eth_nft_data(
                        eth_nft_holders_response.data.get(
                            'TokenBalances', {}).get('TokenBalance', []),
                        recommended_users
                    )

                    if not eth_nft_holders_response.has_next_page:
                        break
                    else:
                        eth_nft_holders_response = await eth_nft_holders_response.get_next_page
                else:
                    print("Error: ", eth_nft_holders_response.error)
                    break

            if not eth_nft_response.has_next_page:
                break
            else:
                eth_nft_response = await eth_nft_response.get_next_page
        else:
            print("Error: ", eth_nft_response.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.9: Fetch Common Polygon NFT Holders Data

You can use Airstack to fetch all the NFTs that are hold by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), on Polygon:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/G6o3Bo8lgM" %}
Show me all Polygon NFT address owned by vitalik.eth
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  TokenBalances(
    input: {
      filter: { tokenType: { _in: [ERC721] }, owner: { _eq: $user } }
      blockchain: polygon
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
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
          "tokenAddress": "0x6f4394d3a3f3e6a264bfbeef4ef73458d3a71cb7"
        },
        {
          "tokenAddress": "0x55909bb8cd8276887aae35118d60b19755201c68"
        },
        {
          "tokenAddress": "0x01647f5eb4e469843bcfa7b43f1f26300f2a0f1d"
        }
        // other Polygon NFTs hold by vitalik.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, the response can be filtered to only Polygon NFTs and be formatted into an array of token addresses to be used in the next step:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const tokenAddresses =
  data?.TokenBalances?.TokenBalance?.map((token) => token.tokenAddress) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
token_addresses = [token['tokenAddress'] for token in data.get('TokenBalances', {}).get('TokenBalance', [])] if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances'] else []
```
{% endtab %}
{% endtabs %}

where `data` is the response from the API. The formatted result will have a format as follows:

```json
[
  "0x6f4394d3a3f3e6a264bfbeef4ef73458d3a71cb7",
  "0x55909bb8cd8276887aae35118d60b19755201c68",
  "0x01647f5eb4e469843bcfa7b43f1f26300f2a0f1d",
  "0x898534c01d3533d43433392f51be07bdacb9a3d7",
  "0x772087591891f6d55f9943f36d2583e11717f692",
  "0x18da13a311d9b1c1dc993bc02bb64dec48686a60",
  "0xc690dd5f1b29b91b8e25a6eba9b45d5efefeb6d8",
  "0x5d666f215a85b87cb042d59662a7ecd2c8cc44e6",
  "0xfabb1b73f27a2e5d8c8cde58572cfb485d9b01e4",
  "0xb2af63208ac7173474b92200dd9f2b623e2e6171",
  "0x3aec78547f92357238851e615ac7cc7614d87584",
  "0xa6511c9c07ea84955530f52cd8be009557478261",
  "0x95728feaf3e9ec79651822b1ae5df30cd075e09e",
  "0x9494879fca4a69afdc230f00907d1f83779408ea",
  "0x7c6ab04b66a47d65b469dc12286401a188b2fd8e",
  "0x7c6ab04b66a47d65b469dc12286401a188b2fd8e",
  "0x56d49c7e00945ecb2ee7747d7e56936651c6bd6a",
  "0xdf230dc0d5e4c6bba9f1570a58be8120a33dc50f",
  "0x43bcad6eff1fa2e75a66b975668a9a5edc077cba",
  "0xd6739ecf05c00d1041e6426e234dda2df49a6a10",
  "0xbe032a91c511621458836760b23b0ba008d523d4",
  "0xbbb9a43df595b502be75d09345460098538d2870"
  // other Polygon NFT addresses
]
```

**Fetch all Polygon NFT owners**

Using the array of token addresses from the first step, you can fetch all Polygon NFT holders that hold any of the NFTs that the given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), owned on Polygon:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/vgOlSFjQhH" %}
Show me Polygon NFT holders of an array of Polygon NFT addresses
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _in: $tokenAddresses }
        tokenType: { _in: [ERC721] }
      }
      blockchain: polygon
      limit: 200
    }
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "tokenAddresses": [
    "0x6f4394d3a3f3e6a264bfbeef4ef73458d3a71cb7",
    "0x55909bb8cd8276887aae35118d60b19755201c68",
    "0x01647f5eb4e469843bcfa7b43f1f26300f2a0f1d",
    "0x898534c01d3533d43433392f51be07bdacb9a3d7",
    "0x772087591891f6d55f9943f36d2583e11717f692",
    "0x18da13a311d9b1c1dc993bc02bb64dec48686a60",
    "0xc690dd5f1b29b91b8e25a6eba9b45d5efefeb6d8",
    "0x5d666f215a85b87cb042d59662a7ecd2c8cc44e6",
    "0xfabb1b73f27a2e5d8c8cde58572cfb485d9b01e4",
    "0xb2af63208ac7173474b92200dd9f2b623e2e6171",
    "0x3aec78547f92357238851e615ac7cc7614d87584",
    "0xa6511c9c07ea84955530f52cd8be009557478261",
    "0x95728feaf3e9ec79651822b1ae5df30cd075e09e",
    "0x9494879fca4a69afdc230f00907d1f83779408ea",
    "0x7c6ab04b66a47d65b469dc12286401a188b2fd8e",
    "0x7c6ab04b66a47d65b469dc12286401a188b2fd8e",
    "0x56d49c7e00945ecb2ee7747d7e56936651c6bd6a",
    "0xdf230dc0d5e4c6bba9f1570a58be8120a33dc50f",
    "0x43bcad6eff1fa2e75a66b975668a9a5edc077cba",
    "0xd6739ecf05c00d1041e6426e234dda2df49a6a10",
    "0xbe032a91c511621458836760b23b0ba008d523d4",
    "0xbbb9a43df595b502be75d09345460098538d2870"
    // other Polygon NFT addresses
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenBalances": {
      "TokenBalance": null
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that has common Polygon NFTs with a given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatPolygonNftData.js" %}
```javascript
const formatPolygonNftData = (data, _recommendedUsers = []) => {
  const recommendedUsers = [..._recommendedUsers];

  for (const nft of data) {
    const { owner, token } = nft ?? {};
    const { name, logo, address, tokenNfts = [] } = token ?? {};
    const { addresses } = owner ?? {};
    const tokenNft = tokenNfts?.[0];

    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );

    if (existingUserIndex !== -1) {
      const _addresses = recommendedUsers?.[existingUserIndex]?.addresses || [];
      recommendedUsers[existingUserIndex].addresses = [
        ..._addresses,
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      const _nfts = recommendedUsers?.[existingUserIndex]?.nfts || [];
      const nftExists = _nfts.some((nft) => nft.address === address);
      if (!nftExists) {
        _nfts?.push({
          name,
          image: logo?.small,
          blockchain: "polygon",
          address,
          tokenNfts: tokenNft,
        });
      }
      recommendedUsers[existingUserIndex].nfts = [..._nfts];
    } else {
      recommendedUsers.push({
        ...owner,
        nfts: [
          {
            name,
            image: logo?.small,
            blockchain: "polygon",
            address,
            tokenNfts: tokenNft,
          },
        ],
      });
    }
  }
  return recommendedUsers;
};

export default formatPolygonNftData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/polygon_nft.py" %}
```python
def format_polygon_nft_data(data, _recommended_users=None):
    if _recommended_users is None:
        _recommended_users = []

    recommended_users = _recommended_users.copy()

    for nft in data or []:
        owner = nft.get('owner', {})
        token = nft.get('token', {})

        name = token.get('name')
        logo = token.get('logo', {})
        address = token.get('address')
        token_nfts = token.get('tokenNfts', [])
        addresses = owner.get('addresses', [])
        token_nft = token_nfts[0] if len(token_nfts) > 0 else None

        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(addr in recommended_user_addresses for addr in addresses):
                existing_user_index = index
                break

        if existing_user_index != -1:
            _addresses = recommended_users[existing_user_index].get('addresses', [])
            _addresses.extend(addresses)
            _addresses = list(set(_addresses))  # Remove duplicates
            recommended_users[existing_user_index]['addresses'] = _addresses

            _nfts = recommended_users[existing_user_index].get('nfts', [])
            nft_exists = any(nft['address'] == address for nft in _nfts)
            if not nft_exists:
                _nfts.append({
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'polygon',
                    'address': address,
                    'tokenNfts': token_nfts
                })
            recommended_users[existing_user_index]['nfts'] = _nfts
        else:
            recommended_users.append({
                **owner,
                'nfts': [{
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'polygon',
                    'address': address,
                    'tokenNfts': token_nfts
                }]
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0x0000000000000000000000000000000000000000"],
    "domains": [
      { "name": "rake.eth", "isPrimary": false },
      { "name": "21505.eth", "isPrimary": false },
      { "name": "👑420👑.eth", "isPrimary": false },
      // other domains
    ],
    "socials": [
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@alebaffa",
        "profileImage": "",
        "profileTokenId": "81453",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      },
      // other web3 socials
    ],
    "xmtp": null,
    // show all common Polygon NFTs that is also owned by vitalik.eth
<strong>    "nfts": [
</strong>      {
        "name": "Constant Inflow NFT",
        "image": null,
        "blockchain": "polygon",
        "address": "0x55909bb8cd8276887aae35118d60b19755201c68",
        "tokenNfts": {
          "tokenId": "69405188863775283014867442726397310916844721196293623142708245765342049376515"
        }
      }
    ]
  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all common Polygon NFT holders**

With the queries for fetching all the common Polygon NFT holders that holds the same Polygon NFTs as the given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/formatPolygonNftData.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatPolygonNftData from "./utils/formatPolygonNftData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const nftAddressesQuery = `
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: polygon, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
`;

const nftQuery = `
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: polygon, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
`;

const formatPolygonNftData = async (address, existingUsers = []) => {
  let polygonNftDataResponse;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!polygonNftDataResponse) {
      // Pagination #1: Fetch Polygon NFTs
<strong>      polygonNftDataResponse = await fetchQueryWithPagination(
</strong>        nftAddressesQuery,
        {
          user: address,
        }
      );
    }
    const {
      data: polygonNftData,
      error: polygonNftError,
      hasNextPage: polygonNftHasNextPage,
      getNextPage: polygonNftGetNextPage,
    } = polygonNftDataResponse ?? {};
    if (!polygonNftError) {
      const tokenAddresses =
        polygonNftData?.TokenBalances?.TokenBalance?.map(
          (token) => token.tokenAddress
        ) ?? [];
      let polygonNftHoldersDataResponse;
      while (true) {
        if (tokenAddresses.length === 0) break;
        if (!polygonNftHoldersDataResponse) {
          // Pagination #2: Fetch Polygon NFT Holders
<strong>          polygonNftHoldersDataResponse = await fetchQueryWithPagination(
</strong>            nftQuery,
            {
              tokenAddresses,
            }
          );
        }
        const {
          data: polygonNftHoldersData,
          error: polygonNftHoldersError,
          hasNextPage: polygonNftHoldersHasNextPage,
          getNextPage: polygonNftHoldersGetNextPage,
        } = polygonNftHoldersDataResponse;
        if (!polygonNftHoldersError) {
          recommendedUsers = [
            ...formatPolygonNftData(
              polygonNftHoldersData?.TokenBalances?.TokenBalance,
              recommendedUsers
            ),
          ];
          if (!polygonNftHoldersHasNextPage) {
            break;
          } else {
            polygonNftHoldersDataResponse =
              await polygonNftHoldersGetNextPage();
          }
        } else {
          console.error("Error: ", polygonNftHoldersError);
          break;
        }
      }
      if (!polygonNftHasNextPage) {
        break;
      } else {
        polygonNftDataResponse = await polygonNftGetNextPage();
      }
    } else {
      console.error("Error: ", polygonNftError);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchPolygonNft;
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="functions/polygon_nft.py"><code class="lang-python">from airstack.execute_query import AirstackClient
from utils.polygon_nft import format_polygon_nft_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

nft_addresses_query = """
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: polygon, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
"""

nft_query = """
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: polygon, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
"""


async def fetch_polygon_nft(address, existing_users=[]):
    polygon_nft_response = None
    recommended_users = existing_users.copy()
    while True:
        if polygon_nft_response is None:
            execute_query_client = api_client.create_execute_query_object(
                query=nft_addresses_query, variables={'user': address})
            # Pagination #1: Fetch Polygon NFTs
<strong>            polygon_nft_response = await execute_query_client.execute_paginated_query()
</strong>
        if polygon_nft_response.error is None:
            token_addresses = [token['tokenAddress'] for token in polygon_nft_response.data.get('TokenBalances', {}).get(
                'TokenBalance', [])] if polygon_nft_response.data and 'TokenBalances' in polygon_nft_response.data and 'TokenBalance' in polygon_nft_response.data['TokenBalances'] else []
            polygon_nft_holders_response = None
            while True:
                if polygon_nft_holders_response is None:
                    execute_query_client = api_client.create_execute_query_object(
                        query=nft_query, variables={'tokenAddresses': token_addresses})
                    # Pagination #2: Fetch Polygon NFT Holders
<strong>                    polygon_nft_holders_response = await execute_query_client.execute_paginated_query()
</strong>
                if polygon_nft_holders_response.error is None:
                    recommended_users = format_polygon_nft_data(
                        polygon_nft_holders_response.data.get(
                            'TokenBalances', {}).get('TokenBalance', []),
                        recommended_users
                    )

                    if not polygon_nft_holders_response.has_next_page:
                        break
                    else:
                        polygon_nft_holders_response = await polygon_nft_holders_response.get_next_page
                else:
                    print("Error: ", polygon_nft_holders_response.error)
                    break

            if not polygon_nft_response.has_next_page:
                break
            else:
                polygon_nft_response = await polygon_nft_response.get_next_page
        else:
            print("Error: ", polygon_nft_response.error)
            break

    return recommended_users
</code></pre>
{% endtab %}
{% endtabs %}

#### Step 1.10: Fetch Common Polygon NFT Holders Data

You can use Airstack to fetch all the NFTs that are hold by a given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), on Base:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/mH2WZD8WHm" %}
Show me all Base NFT address owned by vitalik.eth
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($user: Identity!) {
  TokenBalances(
    input: {
      filter: { tokenType: { _in: [ERC721] }, owner: { _eq: $user } }
      blockchain: base
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "vitalik.eth"
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
          "tokenAddress": "0x273db54929d8392c1997be361da89d41af202a49"
        },
        {
          "tokenAddress": "0x3325c30baf2c97a7b8f28d4418e803104ad9e5b9"
        },
        {
          "tokenAddress": "0x344bd884b47dfc988f7e47851d576e0ac083a16f"
        }
        // other Base NFTs hold by vitalik.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, the response can be filtered to only Base NFTs and be formatted into an array of token addresses to be used in the next step:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const tokenAddresses =
  data?.TokenBalances?.TokenBalance?.map((token) => token.tokenAddress) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
token_addresses = [token['tokenAddress'] for token in data.get('TokenBalances', {}).get('TokenBalance', [])] if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances'] else []
```
{% endtab %}
{% endtabs %}

where `data` is the response from the API. The formatted result will have a format as follows:

```json
[
  "0x273db54929d8392c1997be361da89d41af202a49",
  "0x3325c30baf2c97a7b8f28d4418e803104ad9e5b9",
  "0x344bd884b47dfc988f7e47851d576e0ac083a16f",
  "0x1c6fbcf5a97c2e95af33086aad269972450365b6",
  "0xc2c543d39426bfd1db66bbde2dd9e4a5c7212876",
  "0x3cc896f6761253d105737867e784cf2ffd8ca11e",
  "0x0171b64518477b66e4f7069a66585eac513d1d9a",
  "0xde35a0595a53676babf4f4cb0d4efa0b8db46e77",
  "0x2513271b9c0b5131f3e1a949179d53285cae2b23",
  "0xc6db514244f75c55688ecf2a379443551353ac4c",
  "0xc6db514244f75c55688ecf2a379443551353ac4c"
  // other Base NFT addresses
]
```

**Fetch all Base NFT owners**

Using the array of token addresses from the first step, you can fetch all Base NFT holders that hold any of the NFTs that the given user, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), owned on Base:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/zs7NiKbwwa" %}
Show me Base NFT holders of an array of Base NFT addresses
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: { _in: $tokenAddresses }
        tokenType: { _in: [ERC721] }
      }
      blockchain: base
      limit: 200
    }
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "tokenAddresses": [
    "0x273db54929d8392c1997be361da89d41af202a49",
    "0x3325c30baf2c97a7b8f28d4418e803104ad9e5b9",
    "0x344bd884b47dfc988f7e47851d576e0ac083a16f",
    "0x1c6fbcf5a97c2e95af33086aad269972450365b6",
    "0xc2c543d39426bfd1db66bbde2dd9e4a5c7212876",
    "0x3cc896f6761253d105737867e784cf2ffd8ca11e",
    "0x0171b64518477b66e4f7069a66585eac513d1d9a",
    "0xde35a0595a53676babf4f4cb0d4efa0b8db46e77",
    "0x2513271b9c0b5131f3e1a949179d53285cae2b23",
    "0xc6db514244f75c55688ecf2a379443551353ac4c",
    "0xc6db514244f75c55688ecf2a379443551353ac4c"
    // other Base NFT addresses
  ]
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
          "token": {
            "name": ".basepunk",
            "address": "0xc2c543d39426bfd1db66bbde2dd9e4a5c7212876",
            "tokenNfts": [
              {
                "tokenId": "220"
              },
              {
                "tokenId": "74"
              },
              {
                "tokenId": "199"
              },
              {
                "tokenId": "282"
              },
              {
                "tokenId": "1"
              },
              {
                "tokenId": "322"
              },
              {
                "tokenId": "142"
              },
              {
                "tokenId": "7"
              },
              {
                "tokenId": "268"
              },
              {
                "tokenId": "333"
              }
            ],
            "blockchain": "base",
            "logo": {
              "small": null
            }
          },
          "owner": {
            "addresses": ["0xc0acf511babc340fd0ff969e112c4c45e31c6c7c"],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        }
        // Other Base NFT owners
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The response then can be formatted further with the following formatting function to extract all the recommended users that has common Base NFTs with a given user:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="utils/formatBaseNftData.js" %}
```javascript
const formatBaseNftData = (data, _recommendedUsers = []) => {
  const recommendedUsers = [..._recommendedUsers];

  for (const nft of data) {
    const { owner, token } = nft ?? {};
    const { name, logo, address, tokenNfts = [] } = token ?? {};
    const { addresses } = owner ?? {};
    const tokenNft = tokenNfts?.[0];

    const existingUserIndex = recommendedUsers.findIndex(
      ({ addresses: recommendedUsersAddresses }) =>
        recommendedUsersAddresses?.some?.((address) =>
          addresses?.includes?.(address)
        )
    );

    if (existingUserIndex !== -1) {
      const _addresses = recommendedUsers?.[existingUserIndex]?.addresses || [];
      recommendedUsers[existingUserIndex].addresses = [
        ..._addresses,
        ...addresses,
      ]?.filter((address, index, array) => array.indexOf(address) === index);
      const _nfts = recommendedUsers?.[existingUserIndex]?.nfts || [];
      const nftExists = _nfts.some((nft) => nft.address === address);
      if (!nftExists) {
        _nfts?.push({
          name,
          image: logo?.small,
          blockchain: "base",
          address,
          tokenNfts: tokenNft,
        });
      }
      recommendedUsers[existingUserIndex].nfts = [..._nfts];
    } else {
      recommendedUsers.push({
        ...owner,
        nfts: [
          {
            name,
            image: logo?.small,
            blockchain: "base",
            address,
            tokenNfts: tokenNft,
          },
        ],
      });
    }
  }
  return recommendedUsers;
};

export default formatBaseNftData;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/base_nft.py" %}
```python
def format_base_nft_data(data, _recommended_users=None):
    if _recommended_users is None:
        _recommended_users = []

    recommended_users = _recommended_users.copy()

    for nft in data or []:
        owner = nft.get('owner', {})
        token = nft.get('token', {})

        name = token.get('name')
        logo = token.get('logo', {})
        address = token.get('address')
        token_nfts = token.get('tokenNfts', [])
        addresses = owner.get('addresses', [])
        token_nft = token_nfts[0] if len(token_nfts) > 0 else None

        existing_user_index = -1
        for index, recommended_user in enumerate(recommended_users):
            recommended_user_addresses = recommended_user.get('addresses', [])
            if any(addr in recommended_user_addresses for addr in addresses):
                existing_user_index = index
                break

        if existing_user_index != -1:
            _addresses = recommended_users[existing_user_index].get('addresses', [])
            _addresses.extend(addresses)
            _addresses = list(set(_addresses))  # Remove duplicates
            recommended_users[existing_user_index]['addresses'] = _addresses

            _nfts = recommended_users[existing_user_index].get('nfts', [])
            nft_exists = any(nft['address'] == address for nft in _nfts)
            if not nft_exists:
                _nfts.append({
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'base',
                    'address': address,
                    'tokenNfts': token_nfts
                })
            recommended_users[existing_user_index]['nfts'] = _nfts
        else:
            recommended_users.append({
                **owner,
                'nfts': [{
                    'name': name,
                    'image': logo.get('small'),
                    'blockchain': 'base',
                    'address': address,
                    'tokenNfts': token_nfts
                }]
            })

    return recommended_users
```
{% endcode %}
{% endtab %}
{% endtabs %}

The formatted result will have a format as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xc0acf511babc340fd0ff969e112c4c45e31c6c7c"],
    "domains": null,
    "socials": null,
    "xmtp": null,
    // show all common Base NFTs that is also owned by vitalik.eth
<strong>    "nfts": [
</strong>      {
        "name": ".basepunk"",
        "image": null,
        "blockchain": "base",
        "address": "0xc2c543d39426bfd1db66bbde2dd9e4a5c7212876",
        "tokenNfts": {
          "tokenId": "220"
        }
      }
    ]
  },
  // other onchain graph users
]
</code></pre>

**Iterate to fetch all common Base NFT holders**

With the queries for fetching all the common Base NFT holders that holds the same Base NFTs as the given user established, it will be essential to fetch all the data using paginations.

In order to paginate through all the data, you can utilize `fetchQueryWithPagination` and `execute_paginated_query` from the JavaScript (React & Node) and Python SDKs, respectively. The full code implementation for this will be as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="functions/formatBaseNftData.js"><code class="lang-javascript">import { init, fetchQueryWithPagination } from "@airstack/node"; // or @airstack/airstack-react for frontend javascript
import formatPolygonNftData from "./utils/formatPolygonNftData";

// get your API key at https://app.airstack.xyz/profile-settings/api-keys
init("YOUR_AIRSTACK_API_KEY");

const nftAddressesQuery = `
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: base, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
`;

const nftQuery = `
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: base, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
`;

const formatBaseNftData = async (address, existingUsers = []) => {
  let baseNftDataResponse;
  let recommendedUsers = [...existingUsers];
  while (true) {
    if (!baseNftDataResponse) {
      // Pagination #1: Fetch Base NFTs
<strong>      baseNftDataResponse = await fetchQueryWithPagination(
</strong>        nftAddressesQuery,
        {
          user: address,
        }
      );
    }
    const {
      data: baseNftData,
      error: baseNftError,
      hasNextPage: baseNftHasNextPage,
      getNextPage: baseNftGetNextPage,
    } = baseNftDataResponse ?? {};
    if (!baseNftError) {
      const tokenAddresses =
        baseNftData?.TokenBalances?.TokenBalance?.map(
          (token) => token.tokenAddress
        ) ?? [];
      let baseNftHoldersDataResponse;
      while (true) {
        if (tokenAddresses.length === 0) break;
        if (!pbaseNftHoldersDataResponse) {
          // Pagination #2: Fetch Base NFT Holders
<strong>          baseNftHoldersDataResponse = await fetchQueryWithPagination(
</strong>            nftQuery,
            {
              tokenAddresses,
            }
          );
        }
        const {
          data: baseNftHoldersData,
          error: baseNftHoldersError,
          hasNextPage: baseNftHoldersHasNextPage,
          getNextPage: baseNftHoldersGetNextPage,
        } = baseNftHoldersDataResponse;
        if (!baseNftHoldersError) {
          recommendedUsers = [
            ...formatBaseNftData(
              baseNftHoldersData?.TokenBalances?.TokenBalance,
              recommendedUsers
            ),
          ];
          if (!baseNftHoldersHasNextPage) {
            break;
          } else {
            baseNftHoldersDataResponse =
              await baseNftHoldersGetNextPage();
          }
        } else {
          console.error("Error: ", baseNftHoldersError);
          break;
        }
      }
      if (!baseNftHasNextPage) {
        break;
      } else {
        baseNftDataResponse = await baseNftGetNextPage();
      }
    } else {
      console.error("Error: ", baseNftError);
      break;
    }
  }
  return recommendedUsers;
};

export default fetchBaseNft;
</code></pre>
{% endtab %}

{% tab title="Python" %}
```python
from airstack.execute_query import AirstackClient
from utils.base_nft import format_base_nft_data

# get your API key at https://app.airstack.xyz/profile-settings/api-keys
api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

nft_addresses_query = """
query MyQuery($user: Identity!) {
  TokenBalances(input: {filter: {tokenType: {_in: [ERC721]}, owner: {_eq: $user}}, blockchain: base, limit: 200}) {
    TokenBalance {
      tokenAddress
    }
  }
}
"""

nft_query = """
query MyQuery($tokenAddresses: [Address!]) {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: $tokenAddresses}, tokenType: {_in: [ERC721]}}, blockchain: base, limit: 200}
  ) {
    TokenBalance {
      token {
        name
        address
        tokenNfts {
          tokenId
        }
        blockchain
        logo {
          small
        }
      }
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          dappName
          blockchain
          profileName
          profileImage
          profileTokenId
          profileTokenAddress
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
"""


async def fetch_base_nft(address, existing_users=[]):
    base_nft_response = None
    recommended_users = existing_users.copy()
    while True:
        if base_nft_response is None:
            execute_query_client = api_client.create_execute_query_object(
                query=nft_addresses_query, variables={'user': address})
            # Pagination #1: Fetch Base NFTs
            base_nft_response = await execute_query_client.execute_paginated_query()

        if base_nft_response.error is None:
            token_addresses = [token['tokenAddress'] for token in base_nft_response.data.get('TokenBalances', {}).get(
                'TokenBalance', [])] if base_nft_response.data and 'TokenBalances' in base_nft_response.data and 'TokenBalance' in base_nft_response.data['TokenBalances'] else []
            base_nft_holders_response = None
            while True:
                if base_nft_holders_response is None:
                    execute_query_client = api_client.create_execute_query_object(
                        query=nft_query, variables={'tokenAddresses': token_addresses})
                    # Pagination #2: Fetch Base NFT Holders
                    base_nft_holders_response = await execute_query_client.execute_paginated_query()

                if base_nft_holders_response.error is None:
                    recommended_users = format_base_nft_data(
                        base_nft_holders_response.data.get(
                            'TokenBalances', {}).get('TokenBalance', []),
                        recommended_users
                    )

                    if not base_nft_holders_response.has_next_page:
                        break
                    else:
                        base_nft_holders_response = await base_nft_holders_response.get_next_page
                else:
                    print("Error: ", base_nft_holders_response.error)
                    break

            if not base_nft_response.has_next_page:
                break
            else:
                base_nft_response = await base_nft_response.get_next_page
        else:
            print("Error: ", base_nft_response.error)
            break

    return recommended_users
```
{% endtab %}
{% endtabs %}

### Step 2: Aggregate All Data By User Identities

In the previous step, you have successfully create multiple functions to fetch a user's onchain and off-chain data, from POAPs to Lens and Farcasters followers.

In this step, you'll use the data from [Step 1](onchain-graph.md#step-1-fetch-all-on-chain-graph-data) to aggregate all the data fetched and compile it into the given user's **onchain graph**.

Utilizing the data fetching functions that we have defined, we can easily import them into a single file and do an iterative call on every function step-by-step as shown below:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import fetchPoapsData from "./functions/fetchPoapsData";
import fetchFarcasterFollowings from "./functions/fetchFarcasterFollowings";
import fetchLensFollowings from "./functions/fetchLensFollowings";
import fetchFarcasterFollowers from "./functions/fetchFarcasterFollowers";
import fetchLensFollowers from "./functions/fetchLensFollowers";
import fetchTokenSent from "./functions/fetchTokenSent";
import fetchTokenReceived from "./functions/fetchTokenReceived";
import fetchEthNft from "./functions/fetchEthNft";
import fetchPolygonNft from "./functions/fetchPolygonNft";
import fetchBaseNft from "./functions/fetchBaseNft";

const fetchOnChainGraphData = async (address) => {
  let recommendedUsers = [];
  const fetchFunctions = [
    fetchPoapsData,
    fetchFarcasterFollowings,
    fetchLensFollowings,
    fetchFarcasterFollowers,
    fetchLensFollowers,
    fetchTokenSent,
    fetchTokenReceived,
    fetchEthNft,
    fetchPolygonNft,
    fetchBaseNft,
  ];
  for (const func of fetchFunctions) {
    recommendedUsers = await func(address, recommendedUsers);
  }
  return recommendedUsers;
};

const onChainGraphUsers = await fetchOnChainGraphData("vitalik.eth");
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="index.py" %}
```python
import asyncio
from functions.poaps import fetch_poaps_data
from functions.farcaster_followings import fetch_farcaster_followings
from functions.lens_followings import fetch_lens_followings
from functions.farcaster_followers import fetch_farcaster_followers
from functions.lens_followers import fetch_lens_followers
from functions.token_sent import fetch_token_sent
from functions.token_received import fetch_token_received
from functions.ethereum_nft import fetch_eth_nft
from functions.polygon_nft import fetch_polygon_nft
from functions.base_nft import fetch_base_nft

async def fetch_on_chain_graph_data(address):
    recommended_users = []
    fetch_functions = [
        fetch_poaps_data,
        fetch_farcaster_followings,
        fetch_lens_followings,
        fetch_farcaster_followers,
        fetch_lens_followers,
        fetch_token_sent,
        fetch_token_received,
        fetch_eth_nft,
        fetch_polygon_nft,
        fetch_base_nft,
    ]
    for func in fetch_functions:
        recommended_users = await func(address, recommended_users)
    return recommended_users

if __name__ == "__main__":
    onchain_graph_results = asyncio.run(fetch_on_chain_graph_data("vitalik.eth"))
```
{% endcode %}
{% endtab %}
{% endtabs %}

Through the for loops, the `recommendedUsers`(JavaScript) and `recommended_users`(Python) variable will be storing onchain graph users and have their data updated whenever new data is fetched.

### Step 3: Scoring & Sorting

Now that you have all the data aggregated, you might notice that some of those users from the onchain graph might have higher relevancies to the given user, such as having more POAPs in common than the given user.

Thus, for a better user experience, it will make more sense to score individual user profiles and with the scoring system established, sort them in descending order (from the highest score/most relevant to the lowest score/least relevant).

#### Scoring

In this tutorial, let's establish a scoring function that will calculate the total score of individual users on the onchain graph as follows:

$$score(x) = \sum (points * weight)$$

Each data has different methods to calculate **points** and has their **individual weights:**

| Data                   | Points                       | Weight (Default) |
| ---------------------- | ---------------------------- | ---------------- |
| Token sent             | 1                            | 10               |
| Token received         | 1                            | 0                |
| Followed on Lens       | 1                            | 5                |
| Following on Lens      | 1                            | 7                |
| Followed on Farcaster  | 1                            | 5                |
| Following on Farcaster | 1                            | 5                |
| Common POAPs           | number of POAPs hold         | 7                |
| Common Ethereum NFTs   | number of Ethereum NFTs hold | 5                |
| Common Polygon NFTs    | number of Polygon NFTs hold  | 0                |
| Common Base NFTs       | number of Base NFTs hold     | 3                |

Thus, translating this into code, the score calculation function will look as follows:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="score.js" %}
```javascript
const defaultScoreMap = {
  tokenSent: 10,
  tokenReceived: 0,
  followedByOnLens: 5,
  followingOnLens: 7,
  followedByOnFarcaster: 5,
  followingOnFarcaster: 5,
  commonPoaps: 7,
  commonEthNfts: 5,
  commonPolygonNfts: 0,
  commonBaseNfts: 3,
};

const identityMap = (identities) =>
  identities.reduce((acc, identity) => {
    acc[identity] = true;
    return acc;
  }, {});

const isBurnedAddress = (address) => {
  if (!address) {
    return false;
  }
  address = address.toLowerCase();
  return (
    address === "0x0000000000000000000000000000000000000000" ||
    address === "0x000000000000000000000000000000000000dead"
  );
};

const calculatingScore = (user, scoreMap = defaultScoreMap) => {
  const identities = [user];
  if (
    user.addresses?.some((address) => identityMap(identities)[address]) ||
    user.domains?.some(({ name }) => identityMap(identities)[name]) ||
    user.addresses?.some((address) => isBurnedAddress(address))
  ) {
    return;
  }

  let score = 0;
  if (user.follows?.followingOnLens) {
    score += scoreMap.followingOnLens;
  }
  if (user.follows?.followedOnLens) {
    score += scoreMap.followedByOnLens;
  }
  if (user.follows?.followingOnFarcaster) {
    score += scoreMap.followingOnFarcaster;
  }
  if (user.follows?.followedOnFarcaster) {
    score += scoreMap.followedByOnFarcaster;
  }
  if (user.tokenTransfers?.sent) {
    score += scoreMap.tokenSent;
  }
  if (user.tokenTransfers?.received) {
    score += scoreMap.tokenReceived;
  }
  let uniqueNfts = [];
  if (user.nfts) {
    const existingNFT = {};
    uniqueNfts = user.nfts.filter((nft) => {
      const key = `${nft.address}-${nft.tokenNfts?.tokenId}`;
      if (existingNFT[key] || isBurnedAddress(nft.address)) {
        return false;
      }
      existingNFT[key] = true;
      return true;
    });

    const ethNftCount = uniqueNfts.filter(
      (nft) => nft.blockchain === "ethereum"
    ).length;
    const polygonNftCount = uniqueNfts.filter(
      (nft) => nft.blockchain === "polygon"
    ).length;
    const baseNftCount = uniqueNfts.filter(
      (nft) => nft.blockchain === "base"
    ).length;
    score +=
      scoreMap.commonEthNfts * ethNftCount +
      scoreMap.commonPolygonNfts * polygonNftCount +
      scoreMap.commonBaseNfts * baseNftCount;
  }
  if (user.poaps) {
    score += scoreMap.commonPoaps * user.poaps.length;
  }

  return {
    ...user,
    _score: score,
  };
};

export default calculatingScore;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="score.py" %}
```python
default_score_map = {
    'tokenSent': 10,
    'tokenReceived': 0,
    'followedByOnLens': 5,
    'followingOnLens': 7,
    'followedByOnFarcaster': 5,
    'followingOnFarcaster': 5,
    'commonPoaps': 7,
    'commonEthNfts': 5,
    'commonPolygonNfts': 0,
}


def identity_map(users):
    identity_dict = {}
    for user in users:
        # Assuming user is a dictionary and has an 'id' field of a hashable type (e.g., string or int)
        user_id = user.get('id')
        if user_id is not None:
            identity_dict[user_id] = True
    return identity_dict


def is_burned_address(address):
    if not address:
        return False
    address = address.lower()
    return address in ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dead"]


def calculating_score(user, score_map=None):
    if score_map is None:
        score_map = default_score_map

    identities = [user]
    identity_dict = identity_map(identities)

    addresses = user.get('addresses', [])
    domains = user.get('domains', [])

    # Ensure addresses is a list
    if not isinstance(addresses, list):
        addresses = []

    # Ensure domains is a list of dictionaries
    if domains is not None or not isinstance(domains, list) or not all(isinstance(domain, dict) for domain in domains):
        domains = []

    if any(address in identity_dict for address in addresses if address is not None) or \
       any(domain.get('name') in identity_dict for domain in domains if domain is not None) or \
       any(is_burned_address(address) for address in addresses if address is not None):
        return None

    score = 0
    follows = user.get('follows', {})
    token_transfers = user.get('tokenTransfers', {})

    for key in ['followingOnLens', 'followedOnLens', 'followingOnFarcaster', 'followedOnFarcaster']:
        score += follows.get(key, 0) * score_map.get(key, 0)

    for key in ['sent', 'received']:
        score += token_transfers.get(key, 0) * \
            score_map.get('token' + key.capitalize(), 0)

    unique_nfts = {f"{nft['address']}-{nft.get('tokenNfts', {}).get('tokenId')}" for nft in user.get(
        'nfts', []) if not is_burned_address(nft['address'])}
    eth_nft_count = sum(1 for nft in unique_nfts if 'ethereum' in nft)
    polygon_nft_count = sum(1 for nft in unique_nfts if 'polygon' in nft)

    score += (score_map['commonEthNfts'] * eth_nft_count) + \
        (score_map['commonPolygonNfts'] * polygon_nft_count)

    poaps = user.get('poaps', [])
    score += score_map['commonPoaps'] * len(poaps)

    user['_score'] = score
    return user
```
{% endcode %}
{% endtab %}
{% endtabs %}

To assign score to each user, you can simply use the following code:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="index.js"><code class="lang-javascript">import calculatingScore from "score";

const onChainGraphUsers = await fetchOnChainGraphData("vitalik.eth");
<strong>const onChainGraphUsersWithScore = recommendUsers.map(user => calculatingScore(user));
</strong>console.log(onChainGraphUsersWithScore);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="index.py"><code class="lang-python">from score import calculating_score

if __name__ == "__main__":
    onchain_graph_results = asyncio.run(fetch_on_chain_graph_data("vitalik.eth"))
<strong>    on_chain_graph_users_with_score = [calculating_score(user) for user in on_chain_graph_users]
</strong>    print(on_chain_graph_users_with_score)
</code></pre>
{% endtab %}
{% endtabs %}

and the modified JSON will have a new `_score` field as follows:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xd35f7c2f23fdc341aa8c7534f0e521679206a036"],
    "domains": [{ "name": "taoliu.eth", "isPrimary": true }],
    "socials": [
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@colinlt",
        "profileImage": "",
        "profileTokenId": "33481",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      }
    ],
    "xmtp": null,
    "poaps": [
      {
        "name": "Rocket Pool Bot Catcher POAP",
        "image": null,
        "eventId": "7426"
      }
    ],
<strong>    "_score": 7 // onchain graph score of the user `taoliu.eth`
</strong>  },
  {
    "addresses": ["0x263af7a0ba6f8432e7861b9d92a44639c768d17f"],
    "domains": null,
    "socials": null,
    "xmtp": null,
    "poaps": [
      {
        "name": "Messi Win FWC QATAR 2022",
        "image": "https://assets.airstack.xyz/image/poap/qSZIZX20XcTPh6qr55uhXw==/extra_small.png",
        "eventId": "92705"
      }
    ],
<strong>    "_score": 7 // onchain graph score of the user `0x263af7a0ba6f8432e7861b9d92a44639c768d17f`
</strong>  },
  {
    "addresses": ["0xd5aec8ceb2a5dee7914d1c5d07db7e3391253f31"],
    "domains": null,
    "socials": null,
    "xmtp": null,
    "poaps": [
      {
        "name": "Rocket Pool Bot Catcher POAP",
        "image": null,
        "eventId": "7426"
      }
    ],
<strong>    "_score": 7 // onchain graph score of the user `0xd5aec8ceb2a5dee7914d1c5d07db7e3391253f31`
</strong>  }
]
</code></pre>

#### Sorting

Lastly, once you have all the recommended users' score calculated, you can use the following sorting function that will return the sorted result of the onchain graph:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="sort.js" %}
```javascript
const sortByScore = (recommendations) => {
  return recommendations.sort((a, b) => {
    return (b._score || 0) - (a._score || 0);
  });
};

export default sortByScore;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="sort.py" %}
```python
def sort_by_score(recommendations):
    return sorted(recommendations, key=lambda x: x.get('_score', 0), reverse=True)
```
{% endcode %}
{% endtab %}
{% endtabs %}

Import it to the index file as follows:

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript" data-title="index.js"><code class="lang-javascript">import sortByScore from "sort";

const onChainGraphUsers = await fetchOnChainGraphData("vitalik.eth");
const onChainGraphUsersWithScore = recommendUsers.map(user => calculatingScore(user));
// finalOnChainGraphUsers can be stored in database
<strong>const finalOnChainGraphUsers = sortByScore(onChainGraphUsersWithScore);
</strong>console.log(finalOnChainGraphUsers);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python" data-title="index.py"><code class="lang-python">from score import calculating_score

if __name__ == "__main__":
    onchain_graph_results = asyncio.run(fetch_on_chain_graph_data("vitalik.eth"))
    on_chain_graph_users_with_score = [calculating_score(user) for user in on_chain_graph_users]
    # final_on_chain_graph_users can be stored on database
<strong>    final_on_chain_graph_users = sort_by_score(on_chain_graph_users_with_score)
</strong>    print(final_on_chain_graph_users)
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you are doing **backend integration**, you can store the `finalOnChainGraphUsers`(JavaScript) or `final_on_chain_graph_users`(Python) that contains the fully-processed onchain graph data into your database.
{% endhint %}

The sorted final result will look as shown below:

<pre class="language-json"><code class="lang-json">[
  {
    "addresses": ["0xf6b6f07862a02c85628b3a9688beae07fea9c863"],
    "domains": [
      { "name": "juliuspreite.eth", "isPrimary": false },
      { "name": "poap.mirror.xyz", "isPrimary": false },
      { "name": "poap.sismo.eth", "isPrimary": false },
      // other ENS domains
    ],
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "worthalter",
        "profileImage": "https://i.imgur.com/5ywjJJD.jpg",
        "profileTokenId": "9456",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      }
    ],
    "xmtp": [{ "isXMTPEnabled": true }],
    "poaps": [
      {
        "name": "ETHWaterloo 2019",
        "image": "https://assets.airstack.xyz/image/poap/jJBtpGTUG7nxFlXa6q+pvQ==/extra_small.png",
        "eventId": "84"
      },
      {
        "name": "TEL AVIV BLOCKCHAIN WEEK 2019",
        "image": "https://assets.airstack.xyz/image/poap/DCDoHTJADUNCSJKdWXTang==/extra_small.png",
        "eventId": "65"
      },
      {
        "name": "Zcon0",
        "image": "https://assets.airstack.xyz/image/poap/UEKJYWIB9mwSux2YThOnZw==/extra_small.png",
        "eventId": "36"
      },
      // other POAPs
    ],
<strong>    "_score": 56 // The highest score comes first
</strong>  },
  {
    "addresses": ["0x225f137127d9067788314bc7fcc1f36746a3c3b5"],
    "domains": [
      { "name": "stopspammingyourname.eth", "isPrimary": false },
      { "name": "conferencewifi.eth", "isPrimary": false },
      { "name": "bestfuckever.eth", "isPrimary": false },
      // other ENS domains
    ],
    "socials": [
      {
        "dappName": "farcaster",
        "blockchain": "optimism",
        "profileName": "lucemans",
        "profileImage": "https://i.imgur.com/GOu2pEH.jpg",
        "profileTokenId": "20737",
        "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a"
      },
      {
        "dappName": "lens",
        "blockchain": "polygon",
        "profileName": "lens/@lucemans",
        "profileImage": "",
        "profileTokenId": "12083",
        "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d"
      }
    ],
    "xmtp": [{ "isXMTPEnabled": true }],
    "poaps": [
      {
        "name": "I met tjais.eth at Devcon 6",
        "image": "https://assets.airstack.xyz/image/poap/1O7dHBavxRVTxH12cq+RoA==/extra_small.png",
        "eventId": "71115"
      },
      {
        "name": "I met nicogallardo.eth at Devcon 6",
        "image": "https://assets.airstack.xyz/image/poap/o5X7XVS3H3umU7iRKeoaPw==/extra_small.png",
        "eventId": "69822"
      },
      {
        "name": "I met helenag.eth at Devcon 6",
        "image": "https://assets.airstack.xyz/image/poap/pX4g6cy3zHCyDIqioCbY4g==/extra_small.png",
        "eventId": "74216"
      },
      // other POAPs
    ],
<strong>    "_score": 49 // lowers score comes after
</strong>  },
  // more onchain graph recommended users
]
</code></pre>

And done! 🎉 :partying\_face: Congratulations you've just built an onchain graph!

### Developer Support

If you have any questions or need help regarding integrating or building onchain graph into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Poaps API Reference](../api-references/api-reference/poaps-api.md)
* [SocialFollowers API Reference](../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API Reference](../api-references/api-reference/socialfollowings-api.md)
* [TokenTransfers API Reference](../api-references/api-reference/tokentransfers-api.md)
* [TokenBalances API Reference](../api-references/api-reference/tokenbalances-api.md)
* [Recommendation Engine Tutorial](recommendation-engine/)
