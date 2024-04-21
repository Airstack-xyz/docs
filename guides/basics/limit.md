---
description: Learn how the limit input parameter works in the Airstack APIs.
---

# Limit

You can set the [pagination](pagination-in-airstack-sdk.md) limit for Airstack API by using the `limit` input parameter to adjust the number of responses returned from the API.

By default, the APIs will return 50 responses if the `limit` is not specified.

You have the option to input an integer up to 200 to the `limit` input parameter to get the most number of responses per API call. If you need more responses, simply [paginate](pagination-in-airstack-sdk.md) to the next page.
