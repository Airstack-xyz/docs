---
description: >-
  Delete an existing webhooks subscription by using the /webhooks/<webhook-id>
  REST API to stop receiving real-time event notifications from Farcaster by
  providing the webhook ID.
---

# \[DELETE] /webhooks/\<webhook-id>

## Header

| Name            | Type     | Description           |
| --------------- | -------- | --------------------- |
| `Authorization` | `String` | The AIrstack API key. |

## URL Parameter

|              |          |                                                              |
| ------------ | -------- | ------------------------------------------------------------ |
| `webhook-id` | `String` | Provide webhook ID of the webhooks you would like to delete. |

## Success Response (Status 200)

| Name      | Type      | Description                                               |
| --------- | --------- | --------------------------------------------------------- |
| `message` | `String`  | Success message declaring successful deletion of webhooks |
| `status`  | `Boolean` | `true`                                                    |

## Fail Response (Status 400)

| Name             | Type                                             | Description                                               |
| ---------------- | ------------------------------------------------ | --------------------------------------------------------- |
| `message`        | `String`                                         | Success message declaring successful deletion of webhooks |
| `status`         | `Boolean`                                        | `false`                                                   |
| `authentication` | [`Authentication`](../objects/authentication.md) | Empty object `{}`                                         |
