---
description: >-
  Learn all the detailed references of FarcasterMoxieEarningStats API that
  provide Moxie earnings of Farcaster user, channel, or network information,
  including the input filters and output fields.
---

# FarcasterMoxieEarningStats

This API will help you calculate the Moxie Earnings that is received by an entity, that could either be a user, channel, or network (Farcaster) type.

## Inputs

### filters

| Name         | Type                                                                                       | Description                                       |
| ------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `entityId`   | `String`                                                                                   | The name of the user or channel.                  |
| `entityType` | [`FarcasterMoxieEarningStatsEntityType!`](../enum/farcastermoxieearningstatsentitytype.md) | The type of fan token, either `USER` or `CHANNEL` |
| `timeFrame`  | [`FarcasterMoxieEarningStatsTimeFrame!`](../enum/farcastermoxieearningstatstimeframe.md)   | either `TODAY`, `WEEKLY`, or `LIFETIME`.          |

### blockchain

{% hint style="info" %}
For **FarcasterMoxieEarningStats** API, it will return all Farcaster channels.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### order

| Name               | Description                                          |
| ------------------ | ---------------------------------------------------- |
| `allEarnings`      | Sort results by the Moxie earnings from all sources. |
| `castEarnings`     | Sort results by the Moxie earnings from casts.       |
| `frameDevEarnings` | Sort results by the Moxie earnings from Frame devs.  |
| `otherEarnings`    | Sort results by other Moxie earnings.                |

## Outputs

| Name                          | Type                                                                                       | Description                                                                                               |
| ----------------------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| `entityId`                    | `String`                                                                                   | The name of the user or channel.                                                                          |
| `entityType`                  | [`FarcasterMoxieEarningStatsEntityType!`](../enum/farcastermoxieearningstatsentitytype.md) | The type of fan token, either `USER` or `CHANNEL`                                                         |
| `allEarningsAmount`           | `Float`                                                                                    | The total earnings of Moxie by the entity.                                                                |
| `allEarningsAmountInWei`      | `String`                                                                                   | The total earnings of Moxie by the entity in wei (multiply 10^18 from the original amount).               |
| `castEarningsAmount`          | `Float`                                                                                    | The earnings of Moxie earned from casts by the entity.                                                    |
| `castEarningsAmountInWei`     | `String`                                                                                   | The earnings of Moxie earned from casts by the entity in wei (multiply 10^18 from the original amount)    |
| `frameDevEarningsAmount`      | `Float`                                                                                    | The earnings of Moxie earned from frames by the entity.                                                   |
| `frameDevEarningsAmountInWei` | `String`                                                                                   | The earnings of Moxie earned from frames by the entity  in wei (multiply 10^18 from the original amount). |
| `otherEarningsAmount`         | `Float`                                                                                    | The earnings of Moxie earned from others by the entity.                                                   |
| `otherEarningsAmountInWei`    | `String`                                                                                   | The earnings of Moxie earned from others by the entity in wei (multiply 10^18 from the original amount).  |
| `channel`                     | [`FarcasterChannels`](farcasterchannels-api.md)                                            | Channel details if the entity is `CHANNEL` type.                                                          |
| `socials`                     | [`Socials`](socials-api.md)                                                                | User details if the entiy is `USER` type.                                                                 |
| `timeframe`                   | [`FarcasterMoxieEarningStatsTimeFrame`](../enum/farcastermoxieearningstatstimeframe.md)    | either `TODAY`, `WEEKLY`, or `LIFETIME`, depending on the input chosen.                                   |
| `startTimestamp`              | `Time!`                                                                                    | The time when the entity start earning Moxie.                                                             |
| `endTimestamp`                | `Time!`                                                                                    | The time when the entity last earn Moxie.                                                                 |
