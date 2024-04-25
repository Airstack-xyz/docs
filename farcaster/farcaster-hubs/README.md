---
description: >-
  Build Farcaster apps easily without managing your own Hubs using Airstack Hubs
  API
---

# 🖥️ Farcaster Hubs 🆓

The Airstack Hubs API is offered for **FREE** for and provides **access to all read and write endpoints available on Farcaster Hubs.**

{% embed url="https://youtu.be/cKtBYrqnxYo" %}
Airstack Hubs API Overview
{% endembed %}

'An Airstack API key is required but no credits are charged for read/write to Hubs. For more advanced queries, feeds, abstractions, and composability see our [Farcaster APIs](../farcaster/) and [Farcaster Frames](../farcaster-frames/) guides.

For the Airstack Hubs API, the rate limit is 3000 per minute / 300 per second.

Currently, the Airstack Hubs API is offered as both an **HTTP API** and **GRPC API**:

## HTTP API

| Inputs   | Description                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| Base URL | `https://hubs.airstack.xyz`                                                                                     |
| Headers  | Provide [Airstack API ](../../get-started/get-api-key.md)key through `x-airstack-hubs` field for authorization. |

You can test out the HTTP API live by using Swagger UI [here](https://swagger.airstack.xyz/api-docs) and provide Airstack API key to the **Authorize** button (located on the right side of the page).

To integrate Hubs HTTP API into your Farcaster app, click [here](quickstart/http-api.md).

## GRPC API



| Inputs   | Description                                                                                                     |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| Base URL | `hubs-grpc.airstack.xyz`                                                                                        |
| Metadata | Provide [Airstack API ](../../get-started/get-api-key.md)key through `x-airstack-hubs` field for authorization. |

To integrate Hubs GRPC API into your Farcaster app, click [here](quickstart/grpc-api.md).
