---
description: >-
  Use FilterConfig to set filter configuration for your webhooks to customize
  the data payload you will receive in your endpoint.
---

# FilterConfig

## Example

```json
{
  "endpoint": "https://webhook.site",
  "filter_config": {
    "event_type": "profile.updated",
    "filter": {
      "userId": "3761"
    },
    "payload": {
      "userId": "3761",
      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "tokenstaker.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      "isFarcasterPowerUser": false,
      "followingCount": 117,
      "followerCount": 108,
      "socialCapital": {
        "socialCapitalScore": 0.00151243059,
        "socialCapitalRank": 1512
      }
    }
  }
}
```

## Fields

| Name         | Type                                                                                               | Description                                                                                                                                                                                                                                                                                                                         |
| ------------ | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `event_type` | [`EventTypes`](../enums/eventtypes.md)                                                             | <p>Type of events for the webhooks to subscribe to.<br><br>Check out <a href="../enums/eventtypes.md"><code>EventTypes</code></a> for all the available event types.</p>                                                                                                                                                            |
| `filter`     | [`ProfileFilter`](../filter/profilefilter.md) `\|` [`FollowFilter`](../filter/followfilter.md)     | <p>Filter configuration to only listen to certain events that fulfill the inputted filter.<br><br>Depending on the event types that you are subscribing to, the API will accept a different filter schema.<br><br>For more advanced filter patterns, check our docs <a href="../overview/advanced-filter-patterns.md">here</a>.</p> |
| `payload`    | [`ProfilePayload`](../payload/profilepayload.md) \| [`FollowPayload`](../payload/followpayload.md) | Sample input for verification with the API.                                                                                                                                                                                                                                                                                         |
