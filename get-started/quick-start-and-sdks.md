# 🚀 Quick Start & SDKs

{% hint style="info" %}
**Developer Essentials**

* **Front end** [https://app.airstack.xyz ](https://app.airstack.xyz)
* **API Endpoint** [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql)
* **Rate limits & API Keys**
  * Without login: 50 requests / 5 minutes
  * Rate limits with Login and Free API Key:&#x20;
    * Create a free account at app.airstack.xyz with 50 requests/min, burst of 5/sec. You can find the key by clicking on your **profile -> View API keys**.&#x20;
    * If you need a higher rate limit, [please fill out this form](https://o5weogb3uux.typeform.com/to/u5CNxhWc) to request an upgrade.&#x20;
    * To pass the API Key enter "authorization" in the header with the key value as follows: {"authorization":"your\_key"}
* **Record limit** per API call: 200
  * We support cursor based pagination to retrieve the next set of results.
* **Please join** [**this private Telegram group**](https://t.me/+iL8v1-mSZmZiYzRh) **for direct access to the team**
* **Latency.** New Ethereum & Polygon transactions should be searchable on Airstack within seconds from the transaction finalization.
* **Pricing**.&#x20;
  * All Airstack APIs and services are currently provided for free.
  * We are focused right now on enabling developer use cases to flourish with Airsack. We're only charging right now for enterprise applications.
{% endhint %}

{% hint style="info" %}
**Airstack SDKs & Demo Apps**\
You can use the SDK to integrate the Airstack APIs in **React** or **Python**. SDK functions:

\- Handles the Auth key for all the requests.

\- Hooks to make API calls.

\- Hooks to take care of the paginations for any Airstack Query. Page cursor management will be taken care of internally.

\- A component that renders the NFT images. Just provide the contract address, token Id, and chain name. It will load and render the image as per the view size.\
\
The details to get started are here&#x20;

* React SDK [https://github.com/Airstack-xyz/airstack-web-sdk](https://github.com/Airstack-xyz/airstack-web-sdk)
* React Demo [https://github.com/Airstack-xyz/Demo](https://github.com/Airstack-xyz/Demo)
* NPM [https://www.npmjs.com/package/@airstack/airstack-react](https://www.npmjs.com/package/@airstack/airstack-react)
* Python SDK [https://pypi.org/project/airstack/](https://pypi.org/project/airstack/)
* NodeJS SDK [https://github.com/Airstack-xyz/airstack-node-sdk](https://github.com/Airstack-xyz/airstack-node-sdk)
{% endhint %}
