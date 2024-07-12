---
description: >-
  Learn all the detailed references of TrendingCasts API that provide all
  trending Farcaster casts in a certain period of time, including the inputs and
  output fields.
---

# TrendingCasts API

The TrendingCasts API provides you with a list of trending Farcaster casts casted within a given time frame and sorted by a chosen criteria. You can also use the available filters to only fetch trending Farcaster casts casted by a certain user or casted in a certain channel.

## Inputs

### filter

| Name            | Type                              | Description                                                 |
| --------------- | --------------------------------- | ----------------------------------------------------------- |
| `fid`           | `TrendingCast_Int_Comparator_Exp` | Filter by caster's FID.                                     |
| `rootParentUrl` | `String_Eq_Comparator_Exp`        | Filter by Farcaster channel URL where the casts was casted. |

### criteria

| Name                    | Description                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| `likes`                 | Sort the trending casts by the number of likes.                             |
| `recasts`               | Sort the trending casts by the number of recasts.                           |
| `replies`               | Sort the trending casts by the number of replies.                           |
| `likes_recasts_replies` | Sort the trending casts by the total number of likes, recasts, and replies. |
| `social_capital_value`  | Sort the trending casts by the number of a cast's social capital value.     |

### blockchain

| Name  | Description  |
| ----- | ------------ |
| `ALL` | Base mainnet |

### timeFrame

| Name           | Description                                |
| -------------- | ------------------------------------------ |
| `one_hour`     | Get trending casts from the last 1 hour.   |
| `two_hours`    | Get trending casts from the last 2 hours.  |
| `four_hours`   | Get trending casts from the last 4 hours.  |
| `eight_hours`  | Get trending casts from the last 8 hours.  |
| `twelve_hours` | Get trending casts from the last 12 hours. |
| `one_day`      | Get trending casts from the last 1 day.    |
| `two_days`     | Get trending casts from the last 2 days.   |
| `seven_days`   | Get trending casts from the last 7 days.   |

## Outputs

| Name                          | Type                                           | Description                                                                                                   |
| ----------------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `cast`                        | [`FarcasterCast`](farcastercasts-api.md)       | **Nested Query** – Details of the trending cast.                                                              |
| `channel`                     | [`FarcasterChannel`](farcasterchannels-api.md) | **Nested Query** – Details of channel where the cast was casted.                                              |
| `criteria`                    | `String`                                       | The chosen criteria to evaluate trending casts. This is based on the `criteria` input value.                  |
| `criteriaCount`               | `Float`                                        | The total number for the criteria metric chosen.                                                              |
| `fid`                         | `Int`                                          | The caster's FID.                                                                                             |
| `hash`                        | `String`                                       | Farcaster cast hash.                                                                                          |
| `id`                          | `String`                                       | Airstack unique identifier.                                                                                   |
| `rootParentUrl`               | `String`                                       | The associated Farcaster channel's URL.                                                                       |
| `socialCapitalValueFormatted` | `Float`                                        | [Social Capital Value ](../../abstractions/social-capital-value-and-social-capital-scores.md)formatted value. |
| `socialCapitalValueRaw`       | `String`                                       | [Social Capital Value](../../abstractions/social-capital-value-and-social-capital-scores.md) raw value.       |
| `timeFrom`                    | `Time`                                         | The time when the first engagement (like, recast, or reply) within the time frame occurs.                     |
| `timeTo`                      | `Time`                                         | The time when the last engagement (like, recast, or reply) within the time frame occurs.                      |

