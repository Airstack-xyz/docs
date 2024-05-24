---
description: >-
  Learn the basics of filters in Airstack Webhooks API and how to configure it
  correctly.
---

# ⚗️ Filters

You can use filters to only fetch certain events by defining a custom logical criteria that fulfill your use cases.

Each events will have a different filter data structure as shown in the table below, so ensure that you are using the appropriate filter for the appropriate events you are listening to.

In addition, when specifying a filter in the `filter` field, you will be required to provide a sample payload in the `payload` field. Each payload will need to be in certain data structure similar to filter, with the additional condition that the payload **MUST** have the same value for every field defined in `filter`. Otherwise, other keys that is not included in `filter` can have an arbitrary value in `payload`.

| Events           | Filter                                        | Payload                                          |
| ---------------- | --------------------------------------------- | ------------------------------------------------ |
| `ProfileCreated` | [`ProfileFilter`](../filter/profilefilter.md) | [`ProfilePayload`](../payload/profilepayload.md) |
| `ProfileUpdated` | [`ProfileFilter`](../filter/profilefilter.md) | [`ProfilePayload`](../payload/profilepayload.md) |
| `FollowCreated`  | [`FollowFilter`](../filter/followfilter.md)   | [`FollowPayload`](../payload/followpayload.md)   |
| `FollowDeleted`  | [`FollowFilter`](../filter/followfilter.md)   | [`FollowPayload`](../payload/followpayload.md)   |

To ensure that your filters are defined correctly, you can learn more on how to validate your filters [here](../../guides/webhooks/managing-webhooks.md#validating-filter-configuration).
