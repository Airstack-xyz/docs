---
description: >-
  Learn all the detailed references of FarcasterChannels API that provide
  Farcaster channel participants information, including the input filters and
  output fields.
---

# FarcasterChannels API

The FarcasaterChannels API fetches the details of Farcaster channels, which include data such as channel name, channel ID, description, image URI, participants, and more.

## Inputs

### Filters

| Name                 | Type                          | Description                                                                                                                                                                                                                                    |
| -------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `channelId`          | `String_Comparator_Exp`       | The channel ID, e.g. for [/warpcast](https://warpcast.com/\~/channel/warpcast) channel, the ID is "warpcast".                                                                                                                                  |
| `createdAtTimestamp` | `Time_Comparator_Exp`         | Creation timestamp of the channel.                                                                                                                                                                                                             |
| `leadId`             | `Boolean_Comparator_Exp`      | The channel original host's FIDs.                                                                                                                                                                                                              |
| `leadIdentity`       | `Identity_Comparator_Exp`     | <p>The channel original host's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p>                                               |
| `name`               | `Regex_String_Comparator_Exp` | <p>The name of the channel, not necessarily the same as the channel ID.<br><br>It is the title that you see in the channel page, e.g. for <a href="https://warpcast.com/~/channel/warpcast">/warpcast</a> channel, the name is "Warpcast".</p> |

### Blockchain

{% hint style="info" %}
For **FarcasterChannelParticipants** API, it will return all Farcaster channels.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### Order

| Name                 | Description                                        |
| -------------------- | -------------------------------------------------- |
| `createdAtTimestamp` | Sort the result by the channel creation timestamp. |

## Output

| Name                 | Type                                                                  | Description                                                                                                                                                                                                                                    |
| -------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `channelId`          | `String`                                                              | The channel ID, e.g. for [/warpcast](https://warpcast.com/\~/channel/warpcast) channel, the ID is "warpcast".                                                                                                                                  |
| `createdAtTimestamp` | `Time`                                                                | The last timestamp when an action (either cast or reply) occur by a participant.                                                                                                                                                               |
| `dappName`           | `String`                                                              | Dapp name. Currently, only supports `Farcaster`.                                                                                                                                                                                               |
| `dappSlug`           | `String`                                                              | Dapp name. Currently, only supports `farcaster_v2_optimism`.                                                                                                                                                                                   |
| `id`                 | `ID`                                                                  | Airstack unique identifier for the data point.                                                                                                                                                                                                 |
| `description`        | `String`                                                              | The channel description.                                                                                                                                                                                                                       |
| `imageUrl`           | `String`                                                              | The channel image URL.                                                                                                                                                                                                                         |
| `leadIds`            | `[String]`                                                            | The original host's FIDs.                                                                                                                                                                                                                      |
| `leadProfiles`       | `[Socials]`                                                           | **Nested Query** – The original host's profile details.                                                                                                                                                                                        |
| `name`               | `String`                                                              | <p>The name of the channel, not necessarily the same as the channel ID.<br><br>It is the title that you see in the channel page, e.g. for <a href="https://warpcast.com/~/channel/warpcast">/warpcast</a> channel, the name is "Warpcast".</p> |
| `participants`       | [`FarcasterChannelParticipants`](farcasterchannelparticipants-api.md) | **Nested Query** – associated participants details.                                                                                                                                                                                            |
| `url`                | `String`                                                              | Warpcast URL to the channel.                                                                                                                                                                                                                   |
