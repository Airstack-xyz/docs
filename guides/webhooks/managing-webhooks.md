---
description: >-
  Learn how to manage various aspects of Airstack webhooks for your application,
  from webhook creation, receiving payload, etc.
---

# 🧳 Managing Webhooks

## Create New Webhook

{% hint style="info" %}
Currently, there is no dedicated API to update an existing webhook. If you would like to make changes to your webhook, you should first [delete your webhook](managing-webhooks.md#delete-webhook) and create a new one with a new configuration.
{% endhint %}

You can simply call the `/webhooks` API as shown below to create a new webhooks with the receiving endpoint and filter configuration provided to the body request.

To learn more about filter configuration, check this section [here](managing-webhooks.md#validating-filter-configuration).

{% tabs %}
{% tab title="CURL" %}
```sh
curl -X 'POST' \
  'https://webhooks.airstack.xyz/api/v1/webhooks' \
  -H 'accept: application/json' \
  -H 'Authorization: <YOUR_AIRSTACK_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{
  "endpoint": "https://webhook.site",
  "filter_config": {
    "event_type": "profile.updated"
    }
  }
}'
```
{% endtab %}

{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">// Prerequisites: npm install axios
import axios from 'axios';

const url = 'http://webhooks.airstack.xyz/api/v1/webhooks';
const headers = {
  'accept': 'application/json',
  'Authorization': 'YOUR_AIRSTACK_API_KEY',
  'Content-Type': 'application/json'
};
const data = {
<strong>  endpoint: 'https://webhook.site', // your endpoint
</strong>  filter_config: {
<strong>    event_type: 'profile.updated', // Listen to all profile updates
</strong>  }
};

axios.post(url, data, { headers })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('There was an error!', error);
  });
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">// Prerequisites: npm install axios
const axios = require('axios');

const url = 'http://webhooks.airstack.xyz/api/v1/webhooks';
const headers = {
  'accept': 'application/json',
  'Authorization': 'YOUR_AIRSTACK_API_KEY',
  'Content-Type': 'application/json'
};
const data = {
<strong>  endpoint: 'https://webhook.site', // your endpoint
</strong>  filter_config: {
<strong>    event_type: 'profile.updated', // Listen to all profile updates
</strong>  }
};

axios.post(url, data, { headers })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('There was an error!', error);
  });
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "webhook_id": "01HYDYEBHMANSVJ0JKSVK9W3VY",
  "portal_link": "https://apiserver.instance-fm94fpopa.hc-fhtewk6q9.us-east-2.aws.f2e0a955bb84.cloud/portal?token=am4AdjH0bz67JDDTRwNesZMO",
  "authentication": {
    "header_name": "x-airstack-webhook",
    "header_value": "G85wBFVT1EH7OWrs"
  },
<strong>  "status": true, // indicates the your webhook has been successfully created
</strong>  "message": "Successfully created subscription"
}
</code></pre>
{% endtab %}
{% endtabs %}

## Receiving Payload From Webhooks

To receive data payload from webhooks, you will need to have a **POST** endpoint where you can receive the payload in the body:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import express, { Request, Response } from "express";
import bodyParser from "body-parser";

const app = express();

app.use(bodyParser.json());

app.post("/webhook", (request: Request, response: Response) => {
  console.log(request.body);

  // Add your business logic here

  response.status(200).json("Success");
});

app.listen(4000, () => console.log("Running on port 4000"));
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.json());

app.post("/webhook", (request, response) => {
  console.log(request.body);

  // Add your business logic here

  response.status(200).json("Success");
});

app.listen(4000, () => console.log("Running on port 4000"));
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "3761",
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ]
}
```
{% endtab %}
{% endtabs %}

Then, simply add your business logic before returning status 200 and your endpoint is ready to receive data payload from AIrstack webhooks.

This endpoint have to be accessible through the internet, so if you are developing locally, you should expose your endpoint by creating a secure tunnel with service such as [ngrok](https://ngrok.com).

To create a secure tunnel with ngrok, simply run the following command:

```sh
ngrok http 4000 # Or replace with the PORT for your endpoint
```

This will provide you with a tunnel URL that you can provide as an endpoint when creating the webhook.

## Validating Filter Configuration

Filter configuration has 3 available fields:

* **event\_type (Required): s**pecify the type of events on Farcaster network to listen to, check out the list [here](../../webhooks-api-reference/enums/eventtypes.md).
* **filter:** specify to filter based on the available filter field and the value specified. For profile.created and profile.updated events, check out [**ProfileFilter**](../../webhooks-api-reference/filter/profilefilter.md)**.** For follow.created and follow.deleted events, check out [**FollowFilter**](../../webhooks-api-reference/filter/followfilter.md).
* **payload**: Only required if **filter** field exist. This field accepts a sample payload which needs to have the same field value for fields specified in **filter**, while others can have arbitrary value. For profile.created and profile.updated events, check out [**ProfilePayload**](../../webhooks-api-reference/payload/profilepayload.md)**.** For follow.created and follow.deleted events, check out [**FollowPayload**](../../webhooks-api-reference/payload/followpayload.md).

In order to ensure that your webhooks are created successfully, it is best practice that you validate your filter configuration as shown below:

{% hint style="info" %}
In `payload`, ensure that the value of fields matched to the value provided in `filter` field. Other fields that is not provided in `filter` field can just use arbitrary value in `payload`.

Otherwise, if this is not fulfilled, validation will fail.
{% endhint %}

{% tabs %}
{% tab title="CURL" %}
<pre class="language-sh"><code class="lang-sh">curl -X 'POST' \
  'http://webhooks.airstack.xyz/api/v1/filters/test' \
  -H 'accept: application/json' \
  -H 'Authorization: &#x3C;YOUR_AIRSTACK_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{
    "event_type": "follow.deleted",
    "filter": {
<strong>      "followerProfileId": "3761"
</strong>    },
    "payload": {
<strong>      "followerProfileId": "3761",
</strong>      "followingProfileId": "326089"
    }
  }'
</code></pre>
{% endtab %}

{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">// Prerequisites: npm install axios
import axios from 'axios';

const url = 'http://webhooks.airstack.xyz/api/v1/webhooks';
const headers = {
  'accept': 'application/json',
  'Authorization': 'YOUR_AIRSTACK_API_KEY',
  'Content-Type': 'application/json'
};
const data = {
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
<strong>    event_type: 'follow.deleted', // Listen to all unfollow events
</strong>    filter: {
<strong>      followerProfileId: "3761"
</strong>    },
    payload: {
<strong>      followerProfileId: "3761",
</strong>      followingProfileId: "326089"
    }
  }
};

axios.post(url, data, { headers })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('There was an error!', error);
  });
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">// Prerequisites: npm install axios
const axios = require('axios');

const url = 'http://webhooks.airstack.xyz/api/v1/webhooks';
const headers = {
  'accept': 'application/json',
  'Authorization': 'YOUR_AIRSTACK_API_KEY',
  'Content-Type': 'application/json'
};
const data = {
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
<strong>    event_type: 'follow.deleted', // Listen to all unfollow events
</strong>    filter: {
<strong>      followerProfileId: "3761"
</strong>    },
    payload: {
<strong>      followerProfileId: "3761",
</strong>      followingProfileId: "326089"
    }
  }
};

axios.post(url, data, { headers })
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('There was an error!', error);
  });
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "success": true
}
```
{% endtab %}
{% endtabs %}

## Delete Webhook

To delete an existing webhook, simply run the following code and provide the webhook ID associated with the webhook that would like to be deleted:

{% tabs %}
{% tab title="CURL" %}
```sh
curl -X 'DELETE' \
  'http://webhooks.airstack.xyz/api/v1/webhooks/<webhook_id>' \
  -H 'accept: application/json' \
  -H 'Authorization: <YOUR_AIRSTACK_API_KEY>'
```
{% endtab %}

{% tab title="TypeScript" %}
```typescript
// Prerequisites: npm install axios
import axios from "axios";

const url = "http://webhooks.airstack.xyz/api/v1/webhooks/<webhook_id>";
const headers = {
  accept: "application/json",
  Authorization: "YOUR_AIRSTACK_API_KEY",
};

axios
  .delete(url, { headers })
  .then((response) => {
    console.log("Webhook deleted successfully:", response.data.message);
  })
  .catch((error) => {
    console.error("There was an error!", error);
  });
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Prerequisites: npm install axios
const axios = require("axios");

const url = "http://webhooks.airstack.xyz/api/v1/webhooks/<webhook_id>";
const headers = {
  accept: "application/json",
  Authorization: "YOUR_AIRSTACK_API_KEY",
};

axios
  .delete(url, { headers })
  .then((response) => {
    console.log("Webhook deleted successfully:", response.data.message);
  })
  .catch((error) => {
    console.error("There was an error!", error);
  });
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "message": "Webhook deleted successfully",
  "status": true
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding managing Airstack webhooks, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Quickstart Guide](quickstart.md)
* [Advanced Filter Patterns](../../webhooks-api-reference/overview/advanced-filter-patterns.md)
* [Webhooks API Reference](../../webhooks-api-reference/overview/)
