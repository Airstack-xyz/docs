---
description: Learn more details on how authentication with the Airstack API keys work.
---

# 🔐 Authentication

The Airstack API uses [API keys](https://stripe.com/docs/keys) to authenticate requests. To get your Airstack API key, follow this tutorial [here](../../get-started/get-api-key.md).

{% hint style="info" %}
If you are looking for step-by-step guide to integrate Airstack to your project and add your API key, go to the [Quickstart](../../get-started/quickstart/).
{% endhint %}

There are **two types of API key**:

* :rocket: **Production Key**
  * It provides you with 3000 req/min and bursts of 300 req/sec
  * All usage with this key will be charged $0.00002 per credits used. For more details, check our [pricing](https://app.airstack.xyz/pricing) page
  * Suitable for **production** purpose
* :microscope: **Test Key**
  * It provides you with 20 req/min and bursts of 5 req/sec
  * All usage with this key is free of charge
  * Suitable for **test** purpose
  * The test key is only available once you have added a card to your Airstack account. The card will not be charged on any test key usage and will only charge production key usage once all free credits are exhausted.

Your API keys provide access to call the Airstack API that will consume Airstack credits associated to your account, so be sure to keep store them securely as an **environment variable** in your application!

It is strongly discouraged for you store any of the API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

In the case of your keys getting compromised, please contact us directly at Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group or email us at [support@airstack.xyz](mailto:support@airstack.xyz).
