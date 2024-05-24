---
description: >-
  Learn more details on how authentication with the Airstack Webhooks API keys
  work.
---

# 🔐 Authentication

The Airstack Webhooks API uses [API keys](https://stripe.com/docs/keys) to authenticate requests. To get your Airstack API key, follow this tutorial [here](../../get-started/get-api-key.md).

{% hint style="info" %}
If you are looking for step-by-step guide to integrate Airstack webhooks to your project and add your API key, go to the [Quickstart](../../guides/webhooks/quickstart.md).
{% endhint %}

To use webhooks, it is **MANDATORY** that you use prod key to make request to the Airstack Webhooks API as this feature will not be accessible with your test key.

For every request to the Airstack webhooks API, ensure that you provide the prod key into the request header `Authorization` field to authenticate properly.

Your API keys provide access to receive data payload from the Airstack webhooks that will consume Airstack credits associated to your account, so be sure to keep store them securely as an **environment variable** in your application!

It is strongly discouraged for you store any of the API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

In the case of your keys getting compromised, please contact us directly at Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group or email us at [support@airstack.xyz](mailto:support@airstack.xyz).
