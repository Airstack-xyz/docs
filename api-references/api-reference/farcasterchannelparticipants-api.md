---
description: >-
  Learn all the detailed references of FarcasaterChannelParticipants API that
  provide Farcaster channel followers and participants information, including
  the input filters and output fields.
---

# FarcasterChannelParticipants API

The FarcasaterChannelParticipants API fetches the list of all followers and participants on a given channel. In addition, it could also do the reverse, that is fetching the list of all channels a participant interacted with.

## Inputs

### filter

| Name                  | Type                                        | Description                                                                                                                                                                                                                                    |
| --------------------- | ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `channelActions`      | `FarcasterChannelActionType_Comparator_Exp` | <p>To return only participants that have either casted <code>"cast"</code> or replied to a comment <code>"reply"</code>.<br><br>If not, the participants will not be returned into the result and be filtered out.</p>                         |
| `channelId`           | `String_Comparator_Exp`                     | The channel ID, e.g. for [/warpcast](https://warpcast.com/~/channel/warpcast) channel, the ID is "warpcast".                                                                                                                                   |
| `channelName`         | `String_Comparator_Exp`                     | <p>The name of the channel, not necessarily the same as the channel ID.<br><br>It is the title that you see in the channel page, e.g. for <a href="https://warpcast.com/~/channel/warpcast">/warpcast</a> channel, the name is "Warpcast".</p> |
| `lastActionTimestamp` | `Time_Comparator_Exp`                       | Last timestamp when a cast or reply to a cast occur.                                                                                                                                                                                           |
| `participant`         | `Identity_Comparator_Exp`                   | <p>The participant's web3 identity, either 0x address, Farcaster, or ENS.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p>                                                                 |

### blockchain

{% hint style="info" %}
For **FarcasterChannelParticipants** API, it will return all Farcaster channels.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### order

| Name                       | Description                                                                                    |
| -------------------------- | ---------------------------------------------------------------------------------------------- |
| `lastActionBlockTimestamp` | Sort the result by the last timestamp when the participant either casted or replied to a cast. |

## Output

| Name                    | Type                                           | Description                                                                                                                                                                                                                                    |
| ----------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `channel`               | [`FarcasterChannel`](farcasterchannels-api.md) | **Nested Query** – associated channel details                                                                                                                                                                                                  |
| `channelActions`        | `FarcasterChannelActionType`                   | Either `"cast"` or `"reply"`.                                                                                                                                                                                                                  |
| `channelId`             | `String`                                       | The channel ID, e.g. for [/warpcast](https://warpcast.com/~/channel/warpcast) channel, the ID is "warpcast".                                                                                                                                   |
| `channelName`           | `String`                                       | <p>The name of the channel, not necessarily the same as the channel ID.<br><br>It is the title that you see in the channel page, e.g. for <a href="https://warpcast.com/~/channel/warpcast">/warpcast</a> channel, the name is "Warpcast".</p> |
| `dappName`              | `String`                                       | Dapp name. Currently, only supports `Farcaster`.                                                                                                                                                                                               |
| `dappSlug`              | `String`                                       | Dapp name. Currently, only supports `farcaster_v2_optimism`.                                                                                                                                                                                   |
| `id`                    | `ID`                                           | Airstack unique identifier for the data point.                                                                                                                                                                                                 |
| `lastActionTimestamp`   | `Time`                                         | The last timestamp when an action (either cast or reply) occur by a participant.                                                                                                                                                               |
| `lastCastedTimestamp`   | `Time`                                         | <p>The last timestamp when a cast occur in the specified channel by a participant.<br><br>If the participant never casted, then this will be returned as <code>null</code>.</p>                                                                |
| `lastFollowedTimestamp` | `Time`                                         | <p>The last timestamp when the latest user followed a specified channel by a participant.<br><br>If the no participant ever followed the channel, then this will be returned as <code>null</code>.</p>                                         |
| `lastRepliedTimestamp`  | `Time`                                         | <p>The last timestamp when a reply occur in the specified channel by a participant.<br><br>If the participant never replied any cast, then this will be returned as <code>null</code>.</p>                                                     |
| `participant`           | [`Social`](socials-api.md)                     | **Nested Query** – associated participant details.                                                                                                                                                                                             |
| `participantId`         | `String`                                       | FID of the participant.                                                                                                                                                                                                                        |
