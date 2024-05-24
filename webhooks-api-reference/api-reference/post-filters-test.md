---
description: >-
  Test and validate your filters configuration by using the /filters/test REST
  API before creating a webhook subscription to ensure that your filter is
  configured correctly.
---

# \[POST] /filters/test

## Header

| Name            | Type     | Description           |
| --------------- | -------- | --------------------- |
| `Authorization` | `String` | The AIrstack API key. |

## Body

| Name         | Type                                                                                                         | Description                                                                                                                                                                                                                                                                                                                                                               |
| ------------ | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventTypes` | [`EventTypes`](../enums/eventtypes.md)                                                                       | <p>Type of events for the webhooks to subscribe to.<br><br>Check out <a href="../enums/eventtypes.md"><code>EventTypes</code></a> for all the available event types.</p>                                                                                                                                                                                                  |
| `filter`     | `{ body:` [`ProfileFilter`](../filter/profilefilter.md) `\|` [`FollowFilter`](../filter/followfilter.md) `}` | <p>Filter configuration to only listen to certain events that fulfill the inputted filter.<br><br>Depending on the event types that you are subscribing to, the API will accept a different filter schema.<br><br>For more advanced filter patterns, check Convoy's docs on filter <a href="https://docs.getconvoy.io/product-manual/subscriptions#filters">here</a>.</p> |
| `payload`    | [`ProfilePayload`](../payload/profilepayload.md) \| [`FollowPayload`](../payload/followpayload.md)           | Sample input for verification with the API.                                                                                                                                                                                                                                                                                                                               |

## Success Response (Status 200)

| Name     | Type      | Description |
| -------- | --------- | ----------- |
| `status` | `Boolean` | `true`      |

## Fail Response (Status 400)

| Name     | Type      | Description |
| -------- | --------- | ----------- |
| `status` | `Boolean` | `false`     |
