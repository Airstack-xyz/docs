---
description: >-
  Learn how to use Airstack webhooks to listen to various use cases on follow
  events on the Farcaster network.
---

# 🫂 Follow

## Listen To All Follow Events on Farcaster

You can use Airstack webhooks to listen to all Farcaster follow events by using the following configuration:

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
    "event_type": "follow.created"
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
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
<strong>    event_type: 'follow.created', // Listen to all follow events
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
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
<strong>    event_type: 'follow.created', // Listen to all follow events
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

{% tab title="Payload" %}

```json
{
  "id": "000000020c162a7ffcaf089ae12f3e53372302cde3c95cf6bab7b3244df5daa6",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "followerProfileId": "3761"
  "followingProfileId": "326089",
  "followerSince": "2024-02-09T07:37:59Z"
}
```

{% endtab %}
{% endtabs %}

## Listen To Any Farcaster User That Follows A Certain Farcaster User

You can use Airstack webhooks to listen to all events when any Farcaster user follows a certain FID by using the following configuration:

{% tabs %}
{% tab title="CURL" %}

<pre class="language-sh"><code class="lang-sh">curl -X 'POST' \
  'https://webhooks.airstack.xyz/api/v1/webhooks' \
  -H 'accept: application/json' \
  -H 'Authorization: &#x3C;YOUR_AIRSTACK_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{
  "endpoint": "https://webhook.site",
  "filter_config": {
    "event_type": "follow.created",
    "filter": {
<strong>      "followerProfileId": "3761"
</strong>    },
    "payload": {
<strong>      "followerProfileId": "3761",
</strong>      "followingProfileId": "326089"
    }
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
<strong>    event_type: 'follow.created', // Listen to follow events,
</strong>    filter: {
<strong>      followerProfileId: "3761" // filter by this FID
</strong>    },
    payload: {
      // Ensure that this field matched the value in filter
<strong>      followerProfileId: "3761"
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
<strong>    event_type: 'follow.created', // Listen to follow events,
</strong>    filter: {
<strong>      followerProfileId: "3761" // filter by this FID
</strong>    },
    payload: {
      // Ensure that this field matched the value in filter
<strong>      followerProfileId: "3761"
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

{% tab title="Payload" %}

```json
{
  "id": "000000020c162a7ffcaf089ae12f3e53372302cde3c95cf6bab7b3244df5daa6",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "followerProfileId": "3761"
  "followingProfileId": "326089",
  "followerSince": "2024-02-09T07:37:59Z"
}
```

{% endtab %}
{% endtabs %}

## Listen To A Farcaster User When Following Another Farcaster User

You can use Airstack webhooks to listen to all follow events when a Farcaster user is following another Farcaster user by using the following configuration:

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
    "event_type": "follow.created",
    "filter": {
      "followingProfileId": "326089"
    },
    "payload": {
      "followerProfileId": "3761",
      "followingProfileId": "326089"
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
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
<strong>    event_type: 'follow.created', // Listen to follow events,
</strong>    filter: {
<strong>      followingProfileId: "326089" // filter by this FID
</strong>    },
    payload: {
      followerProfileId: "3761"
      // Ensure that this field matched the value in filter
<strong>      followingProfileId: "326089"
</strong>    }
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
<strong>    event_type: 'follow.created', // Listen to follow events,
</strong>    filter: {
<strong>      followingProfileId: "326089" // filter by this FID
</strong>    },
    payload: {
      followerProfileId: "3761"
      // Ensure that this field matched the value in filter
<strong>      followingProfileId: "326089"
</strong>    }
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

{% tab title="Payload" %}

```json
{
  "id": "000000020c162a7ffcaf089ae12f3e53372302cde3c95cf6bab7b3244df5daa6",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "followerProfileId": "3761"
  "followingProfileId": "326089",
  "followerSince": "2024-02-09T07:37:59Z"
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding creating webhook for listening to Farcaster following events, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Advanced Filter Patterns](../../webhooks-api-reference/overview/advanced-filter-patterns.md)
- [Webhooks API Reference](../../webhooks-api-reference/overview/)
