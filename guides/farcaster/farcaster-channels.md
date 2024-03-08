---
description: >-
  Learn how to fetch data from Farcaster channels, including their original host
  and participants who have interacted (either by casting or replying) within
  the channels.
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

# 📺 Farcaster Channels

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Get Channel Details](farcaster-channels.md#get-channel-details)
* [Get Participants Of A Channel](farcaster-channels.md#get-participants-of-a-channel)
* [Get The Original Host Of A Channel](farcaster-channels.md#get-the-original-host-of-a-channel)
* [Get All The Channels Of A Farcaster User Participates In](farcaster-channels.md#get-all-the-channels-of-a-farcaster-user-participates-in)
* [Get All Farcaster Users Who Casted In Certain Channel](farcaster-channels.md#get-all-farcaster-users-who-casted-in-certain-channel)
* [Get All Farcaster Users Who Casted In Certain Channel Since Certain Timestamp](farcaster-channels.md#get-all-farcaster-users-who-casted-in-certain-channel-since-certain-timestamp)
* [Get All Farcaster Users Who Participates In A Channel And Following The Host](farcaster-channels.md#get-all-farcaster-users-who-participates-in-a-channel-and-following-the-host)
* [Check If Farcaster User Has Casted In A Given Channel](farcaster-channels.md#check-if-farcaster-user-has-casted-in-a-given-channel)
* [Search All Farcaster Channels Whose Names Start With Certain Terms (auto-complete)](farcaster-channels.md#search-all-farcaster-channels-whose-names-start-with-certain-terms-auto-complete)
* [Search All Farcaster Channels Whose Names Contain With Certain Terms (auto-complete)](farcaster-channels.md#search-all-farcaster-channels-whose-names-contain-certain-terms-auto-complete)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

### Get Started

**JavaScript/TypeScript/Python**

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

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

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

  if (data) {
    return <p>Data: {JSON.stringify(data)}</p>;
  }

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }
};
```
{% endtab %}

{% tab title="Node" %}
```javascript
import { init, fetchQuery } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const { data, error } = await fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """YOUR_QUERY""" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Channel Details

You can fetch a certain channel details by using the [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) and providing the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/rVa7S9sPmm" %}
Get /farcaster Farcaster channel details
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannels(
    input: {
      blockchain: ALL,
      filter: {
<strong>        channelId: {_eq: "farcaster"} # input 'farcaster' for /farcaster channel
</strong>      }
    }
  ) {
    FarcasterChannel {
      channelId
      dappName
      createdAtTimestamp
      dappSlug
      description
      name
      url
      imageUrl
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "channelId": "farcaster",
          "dappName": "Farcaster",
          "createdAtTimestamp": "2023-08-02T22:33:26Z",
          "dappSlug": "farcaster_v2_optimism",
          "description": "Discussions about Farcaster on Farcaster (meta!)",
          "name": "Farcaster",
          "url": "chain://eip155:7777777/erc721:0x4f86113fc3e9783cf3ec9a552cbb566716a57628",
          "imageUrl": "https://ipfs.decentralized-content.com/ipfs/bafkreialf5usxssf2eu3e5ct37zzdd553d7lg7oywvdszmrg5p2zpkta7u"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Participants Of A Channel

You can fetch all the participants of a channel by using the [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) and providing the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qPY13mDKpQ" %}
Show me all participants of the /farcaster channel on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannels(
    input: {
      blockchain: ALL,
      filter: {
<strong>        channelId: {_eq: "farcaster"} # input 'farcaster' for /farcaster channel
</strong>      }
    }
  ) {
    FarcasterChannel {
      participants {
        participant {
          userAddress
          profileName
          fid: userId
          userAssociatedAddresses
          followerCount
          followingCount
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "participants": [
            {
              "participant": {
                "userAddress": "0x01a7cd990253915391454a111d704c22a62a1e4d",
                "profileName": "voor03",
                "fid": "250359",
                "userAssociatedAddresses": [
                  "0x01a7cd990253915391454a111d704c22a62a1e4d"
                ],
                "followerCount": 434,
                "followingCount": 617
              }
            }
            // Other /warpcast channel participants
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Original Host Of A Channel

You can fetch the original host of a Farcaster channel by using the [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) and providing the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Xa4oRpkT6D" %}
Get the host of /warpcast Farcaster channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannels(
    input: {
      blockchain: ALL,
      filter: {
<strong>        channelId: {_eq: "farcaster"} # input 'farcaster' for /farcaster channel
</strong>      }
    }
  ) {
    FarcasterChannel {
      leadProfiles {
        userAddress
        profileName
        fid: userId
        userAssociatedAddresses
        followerCount
        followingCount
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "leadProfiles": [
            {
              "userAddress": "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "profileName": "v",
              "fid": "2",
              "userAssociatedAddresses": [
                "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                "0x91031dcfdea024b4d51e775486111d2b2a715871",
                "0x182327170fc284caaa5b1bc3e3878233f529d741",
                "0xf86a7a5b7c703b1fd8d93c500ac4cc75b67477f0"
              ],
              "followerCount": 141368,
              "followingCount": 1122
            }
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All The Channels Of A Farcaster User Participates In

You can fetch all the channel a given Farcaster user participates in (either have casted or replied to a cast) by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) and providing the participant's FID to `$participant` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/rAunfvJ3nx" %}
Show me all the channels Farcaster user fid 602 participated in
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
<strong>        participant: {_eq: "fc_fid:602"} # Participant's FID
</strong>      },
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      channelName
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "channelName": "Founders"
        },
        {
          "channelName": "Farcaster Devs"
        },
        {
          "channelName": "Farcaster"
        }
        // Other channels Farcaster user FID 602 participated in
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Who Casted In Certain Channel

You can fetch all Farcaster users who casted in a given channel by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) and providing the `$channelActions` variable with "cast" value and the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LbwKDXEDuG" %}
Show me all the Farcaster user who casted on /warpcast channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
<strong>        channelActions: {_eq: cast}, # Filter only for those who casted
</strong><strong>        channelId: {_eq: "warpcast"}, # Search in /warpcast channel
</strong>      },
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      participant {
        userAddress
        profileName
        fid: userId
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "participant": {
            "userAddress": "0x371eef6d990a7f841e2363452f1e275c783a5b15",
            "profileName": "sterschuyler",
            "fid": "9470"
          }
        },
        {
          "participant": {
            "userAddress": "0x6e30699e7b9b8e4e42b30b0cfd6c5dfd63d4df59",
            "profileName": "got",
            "fid": "279506"
          }
        },
        {
          "participant": {
            "userAddress": "0x77e6a3c5df4c6bf73024e7897dc33356c5ec2600",
            "profileName": "baconsandwich",
            "fid": "288411"
          }
        }
        // Other /warpcaster channel participant
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Who Casted In Certain Channel Since Certain Timestamp

You can fetch all Farcaster users who casted in a given channel since a certain time by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) and providing:

* the "cast" value to the `$channelActions` variable,
* the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable, and
* the timestamp to the `$lastActionTimestamp` variable, e.g. 2024-02-01T00:00:00Z for Feb 1, 2024 at 00:00.

### Try Demo

{% embed url="https://app.airstack.xyz/query/gJ6Yc5atYy" %}
Show me all the Farcaster user who casted on /warpcast channel since Feb 1, 2024
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
<strong>        channelActions: {_eq: cast}, # Filter only for those who casted
</strong><strong>        channelId: {_eq: "warpcast"}, # Search in /warpcast channel
</strong>        # Filter last timestamp to be "greater than or equal to" Feb 1st, 2024
<strong>        lastActionTimestamp: {_gte: "2024-02-01T00:00:00Z"}
</strong>      },
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      participant {
        userAddress
        profileName
        fid: userId
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "participant": {
            "userAddress": "0xff9715817e8b4011ca564f6dcca74a051aaa15a7",
            "profileName": "cepi21",
            "fid": "250243"
          }
        },
        {
          "participant": {
            "userAddress": "0x36ab2c5251b3282f023c2718e78f7e75924eb781",
            "profileName": "itsmwamad",
            "fid": "341677"
          }
        },
        {
          "participant": {
            "userAddress": "0x6b75881c4319c25af8ab843dc071aa5f24557098",
            "profileName": "sh68sana",
            "fid": "320271"
          }
        }
        // Other users casted in /warpcast channel since Feb 1, 2024
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Who Participates In A Channel And Following The Host

To fetch all Farcaster users who participates in a channel and following the host, you'll first need to fetch the original host of the channel:

### Try Demo

{% embed url="https://app.airstack.xyz/query/IjXdzaJLOa" %}
Show the original host of /airstack channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannels(
    input: { blockchain: ALL, filter: { channelId: { _eq: "airstack" } } }
  ) {
    FarcasterChannel {
      leadIds
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "leadIds": ["602"]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Once you have the original host's FID, you can then use it as an input variable to check on each participant if they're following the original host or not:

### Try Demo

{% embed url="https://app.airstack.xyz/query/sP3U4VNq4S" %}
Show all the participants of /airstack channel and if they are following the original host
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($originalHost: Identity!) {
  FarcasterChannelParticipants(
    input: { filter: { channelId: { _eq: "airstack" } }, blockchain: ALL }
  ) {
    FarcasterChannelParticipant {
      participant {
        userAddressDetails {
          socialFollowers(
            input: { filter: { identity: { _eq: $originalHost } } }
          ) {
            Follower {
              followerAddress {
                addresses
                socials(input: { filter: { dappName: { _eq: farcaster } } }) {
                  profileName
                  fid: userId
                  userAssociatedAddresses
                  followerCount
                  followingCount
                }
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
  "originalHost": "fc_fid:602" // The original host's FID
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "participant": {
            "userAddressDetails": {
              "socialFollowers": {
                "Follower": [
                  {
                    "followerAddress": {
                      "addresses": [
                        "0x44e2c9d7a7fc841b9929f0f980acfe784bac82e0",
                        "0x0cf68416279c2a9bcdd0fa6cc4e0347ac3d55d7d"
                      ],
                      "socials": [
                        {
                          // This user participates in /airstack channel
                          // and follows the original host
<strong>                          "profileName": "chuckstock",
</strong>                          "fid": "16405",
                          "userAssociatedAddresses": [
                            "0x44e2c9d7a7fc841b9929f0f980acfe784bac82e0",
                            "0x0cf68416279c2a9bcdd0fa6cc4e0347ac3d55d7d"
                          ],
                          "followerCount": 1957,
                          "followingCount": 136
                        }
                      ]
                    }
                  }
                ]
              }
            }
          }
        },
        {
          "participant": {
            "userAddressDetails": {
              "socialFollowers": {
                // This user does not follow the original host and can be filtered out
<strong>                "Follower": null
</strong>              }
            }
          }
        },
        // Other /airstack channel participants
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

Here, you can simply filter out those who return `null` as they are a participant of the /airstack channel, but does not follow the original host.

## Check If Farcaster User Has Casted In A Given Channel

You can check if Farcaster user has casted in a given channel by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) API and providing:

* the "cast" value to the `$channelActions` variable,
* the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable, and
* the FID to the `$participant` variable

### Try Demo

{% embed url="https://app.airstack.xyz/query/PkFu8vdw9o" %}
Check if Farcaster user FID 602 is participating in airstack channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
        participant: { _eq: "fc_fid:602" }
        channelId: { _eq: "airstack" }
        channelActions: { _eq: cast }
      }
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      lastActionTimestamp
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "lastActionTimestamp": "2024-02-23T17:12:13Z"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Search All Farcaster Channels Whose Names Start With Certain Terms (auto-complete)

You can fetch all Farcaster channels that starts given words by providing `"^<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OYXxn7sAP6" %}
Show me all Farcaster channels name that starts with "air"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannels(
    input: { blockchain: ALL, filter: { name: { _regex: "^air" } } }
  ) {
    FarcasterChannel {
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
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "name": "airdrop-dao"
        },
        {
          "name": "airdropkazet"
        },
        {
          "name": "airdrop1-deutsch"
        }
        // Other channels starting with "air"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Search All Farcaster Channels Whose Names Contain Certain Terms (auto-complete)

You can fetch all Farcaster channels that contains given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Fv5fIwtAH8" %}
Show me all Farcaster channels name that contains with "air"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannels(
    input: { blockchain: ALL, filter: { name: { _regex: "air" } } }
  ) {
    FarcasterChannel {
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
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "name": "airdrop-dao"
        },
        {
          "name": "kazetairdrop"
        },
        {
          "name": "airdropkazet"
        }
        // Other Farcaster channel name containing "air"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetch any data related to Farcaster channels, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterChannels API Reference](../../api-references/api-reference/farcasterchannels-api.md)
* [FarcasterChannelParticipants API Reference](../../api-references/api-reference/farcasterchannelparticipants-api.md)
* [Airstack Onchain Kit for Farcaster Frames](airstack-onchain-kit-for-farcaster-frames.md)
* [Allow Lists for Farcaster Frames](allow-lists-for-farcaster-frames.md)
