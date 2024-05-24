---
description: >-
  Learn how to quickly create you webhooks and start to listen to real-time
  events from the Farcaster network.
---

# 🚀 Quickstart

In this tutorial, you will learn how to quickly create your first Airstack webhooks to easily listen and receive real-time data payload in your server application whenever there is an event occurs in the Farcaster network.

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of webhooks

## Create A Receiving Endpoint

{% hint style="info" %}
If you already have an endpoint to receive data payload from Airstack webhooks, feel free to skip this section.
{% endhint %}

Before creating your first webhook, make sure that you have an endpoint to receive the data payload pushed by Airstack webhooks.

There are two options to get an endpoint:

### Option #1: webhook.site (Easy)

This is **highly recommended** if you are testing Airstack webhooks for the first time.

Simply click on the link below and you'll be provided with an endpoint that can receive real-time data payload from Airstack for any Farcaster events occur:

{% embed url="https://webhook.site/" %}
webhook.site
{% endembed %}

### Option #2: Dedicated Backend (Advanced)

In this tutorial, you will be shown how to create a dedicated **POST** endpoint using Express.

However, if you are familiar with building other frameworks, feel free to build the POST endpoint with the frameworks of your choice.

First, install `express` npm package to your project:

{% tabs %}
{% tab title="npm" %}
```sh
npm i express
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add express
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm add express
```
{% endtab %}
{% endtabs %}

Then, create a **POST** endpoint where you can receive the payload in the body and further deconstruct it to get the `eventName` and `data` from the webhooks:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import express, { Request, Response } from 'express';

const app = express();

app.post('/webhook', (request: Request, response: Response) => {
  const { eventName, data } = request.body ?? {};
  
  // Add your business logic here

  response.status(200).json("Success");
});

app.listen(4000, () => console.log('Running on port 4000'));
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const express = require('express');

const app = express();

app.post('/webhook', (request, response) => {
  const { eventName, payload } = request.body ?? {};
  
  // Add your business logic here

  response.status(200).json("Success");
});

app.listen(4000, () => console.log('Running on port 4000'));
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

## Create Your First Webhooks

Once you have your endpoint ready, you can then create your webhooks by calling the `/webhooks` API.

In the example below, it shows you configuration to listen to all profile update events on the Farcaster network:

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
    "event_type": "ProfileUpdated"
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
<strong>    event_type: 'ProfileUpdated', // Listen to all profile updates
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
<strong>    event_type: 'ProfileUpdated', // Listen to all profile updates
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

Once the webhook is succesfully created, you will receive data payload sent to your endpoint in real-time whenever a profile is updated.

🥳 Congratulations! You've just created your 1st Airstack webhook!

## Developer Support

If you have any questions or need help regarding building your 1st webhook, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Webhooks API Reference](../../webhooks-api-reference/overview/)
