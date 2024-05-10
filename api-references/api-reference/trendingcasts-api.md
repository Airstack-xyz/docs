---
description: >-
  Learn all the detailed references of TrendingCasts API that provide all
  trending Farcaster casts in a certain period of time, including the inputs and
  output fields.
---

# TrendingCasts API

## Inputs

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

| Name                          | Type                                     | Description                                                                                                                  |
| ----------------------------- | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `cast`                        | [`FarcasterCast`](farcastercasts-api.md) | **Nested Query** – Details of the trending cast.                                                                             |
| `criteria`                    | `String`                                 | The chosen criteria to evaluate trending casts. This is based on the `criteria` input value.                                 |
| `criteriaCount`               | `Float`                                  | The total number for the criteria metric chosen.                                                                             |
| `hash`                        | `String`                                 | Farcaster cast hash.                                                                                                         |
| `id`                          | `String`                                 | Airstack unique identifier.                                                                                                  |
| `socialCapitalValueFormatted` | `Float`                                  | [Social Capital Value ](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md)formatted value. |
| `socialCapitalValueRaw`       | `String`                                 | [Social Capital Value](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md) raw value.       |
| `timeFrom`                    | `Time`                                   | The time when the first engagement (like, recast, or reply) within the time frame occurs.                                    |
| `timeTo`                      | `Time`                                   | The time when the last engagement (like, recast, or reply) within the time frame occurs.                                     |

