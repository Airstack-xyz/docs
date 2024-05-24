---
description: >-
  Learn how to use Airstack webhooks to listen to various use cases on profile
  updates on the Farcaster network.
---

# 📸 Profile Update

## Listen To All Profile Updates

You can use Airstack webhooks to listen to all Farcaster profile update events by using the following configuration:

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
  endpoint: 'https://webhook.site', // your endpoint
  filter_config: {
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

{% tab title="Payload" %}

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
  ],
  "isDefault": false,
  "profileName": "tokenstaker.eth",
  "profileTokenId": "3761",
  "profileTokenIdHex": "0x0eb1",
  "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b",
  "profileCreatedAtBlockNumber": 111894270,
  "profileCreatedAtBlockTimestamp": "2023-11-07T20:01:57Z",
  "profileTokenUri": "",
  "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
  "profileMetadata": null,
  "coverImageURI": "",
  "twitterUserName": "",
  "location": "",
  "website": "",
  "profileImageContentValue": {
    "image": {
      "extraSmall": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/extra_small.jpg",
      "large": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/large.jpg",
      "medium": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/medium.jpg",
      "original": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/original_image.jpg",
      "small": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/small.jpg"
    }
  },
  "isFarcasterPowerUser": false,
  "followingCount": 117,
  "followerCount": 108,
  "profileBio": "Building Airstack",
  "profileDisplayName": "Sarvesh",
  "profileImage": "https://i.imgur.com/Y1au7ZB.jpg",
  "socialCapital": {
    "socialCapitalScore": 0.00151243059,
    "socialCapitalScoreRaw": "151243059"
  }
}
```

{% endtab %}
{% endtabs %}

## Listen To All Profile Updates By A Certain FID

You can use Airstack webhooks to listen to all Farcaster profile updates on a certain FID by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
<strong>      "userId": "602"
</strong>    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
<strong>      userId: "602"
</strong>    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: false,
      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
<strong>      userId: "602"
</strong>    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: false,
      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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

<pre class="language-json"><code class="lang-json">{
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
<strong>  "userId": "602", // Match userId as specified in filter
</strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  // ...Other fields
}
</code></pre>

{% endtab %}
{% endtabs %}

## Listen To All Profile Updates Of An Array of FIDs

You can use Airstack webhooks to listen to all Farcaster profile updates on an array of FIDs by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
      "userId": {
<strong>        "$in": ["602", "2602"]
</strong>      }
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      userId: {
<strong>        $in: ["602", "2602"]
</strong>      }
    },
    payload: {
      // userId must have one of the value in $in array as defined
      // in filter
<strong>      userId: "602", 
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: false,
      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      userId: {
<strong>        $in: ["602", "2602"]
</strong>      }
    },
    payload: {
      // userId must have one of the value in $in array as defined
      // in filter
<strong>      userId: "602", 
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: false,
      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602"
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Listen To A Certain FID When Receiving Farcaster Power Badge

You can use Airstack webhooks to listen to a certain FID when earning a Farcaster power badge by using the following configuration:

{% hint style="info" %}
To implement the reverse, that is when losing a Farcaster power badge, simply set `isFarcasterPowerUser` to `false` in `filter`.
{% endhint %}

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
    "event_type": "profile.updated",
    "filter": {
      "$and": [
        {
<strong>          "userId": "602"
</strong>        },
        {
<strong>          "isFarcasterPowerUser": true
</strong>        }
      ]
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
<strong>      "isFarcasterPowerUser": true,
</strong>      "followingCount": 117,
      "followerCount": 108,
      "socialCapital": {
        "socialCapitalScore": 0.00151243059,
        "socialCapitalRank": 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
<strong>          isFarcasterPowerUser: true
</strong>        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      // this must be true as defined in filter
<strong>      isFarcasterPowerUser: true,
</strong>      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
<strong>          isFarcasterPowerUser: true
</strong>        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      // this must be true as defined in filter
<strong>      isFarcasterPowerUser: true,
</strong>      followingCount: 117,
      followerCount: 108,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  "isFarcasterPowerUser": true
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Listen To A Certain FID When Follower Count > X

You can use Airstack webhooks to listen to a certain FID when follower count is above certain number by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
      "$and": [
        {
<strong>          "userId": "602"
</strong>        },
        {
          "followerCount": {
<strong>            "$gte": 1000
</strong>          }
        }
      ]
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      "isFarcasterPowerUser": false,
      "followingCount": 117,
<strong>      "followerCount": 1080,
</strong>      "socialCapital": {
        "socialCapitalScore": 0.00151243059,
        "socialCapitalRank": 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          followerCount: {
<strong>            $gte: 1000
</strong>          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match to value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 117,
<strong>      followerCount: 1080, // follower count must be >= 1000
</strong>      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          followerCount: {
<strong>            $gte: 1000
</strong>          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match to value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 117,
<strong>      followerCount: 1080, // follower count must be >= 1000
</strong>      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  "followerCount": 2000
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Listen To A Certain FID When Following Count > X

You can use Airstack webhooks to listen to a certain FID when following count is above certain number by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
      "$and": [
        {
<strong>          "userId": "602"
</strong>        },
        {
          "followingCount": {
<strong>            "$gte": 1000
</strong>          }
        }
      ]
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      "isFarcasterPowerUser": false,
<strong>      "followingCount": 1170,
</strong>      "followerCount": 1080,
      "socialCapital": {
        "socialCapitalScore": 0.00151243059,
        "socialCapitalRank": 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          followingCount: {
<strong>            $gte: 1000
</strong>          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match to value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
<strong>      followingCount: 1170, // following count must be >= 1000
</strong>      followerCount: 1080,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          followingCount: {
<strong>            $gte: 1000
</strong>          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match to value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
<strong>      followingCount: 1170, // following count must be >= 1000
</strong>      followerCount: 1080,
      socialCapital: {
        socialCapitalScore: 0.00151243059,
        socialCapitalRank: 1512
      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  "followingCount": 2000
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Listen To A Certain FID When Social Capital Score > X

You can use Airstack webhooks to listen to a certain FID when social capital score is above certain number by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
      "$and": [
        {
<strong>          "userId": "602"
</strong>        },
        {
          "socialCapital": {
            "socialCapitalScore": {
              "$gte": 100
            }
          }
        }
      ]
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      "isFarcasterPowerUser": false,
      "followingCount": 1170,
      "followerCount": 1080,
      "socialCapital": {
<strong>        "socialCapitalScore": 110,
</strong>        "socialCapitalRank": 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          socialCapital: {
            socialCapitalScore: {
<strong>              $gte: 100
</strong>            }
          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 1170,
      followerCount: 1080,
      socialCapital: {
<strong>        socialCapitalScore: 110, // social capital score > 100
</strong>        socialCapitalRank: 1512
      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          socialCapital: {
            socialCapitalScore: {
<strong>              $gte: 100
</strong>            }
          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 1170,
      followerCount: 1080,
      socialCapital: {
<strong>        socialCapitalScore: 110, // social capital score > 100
</strong>        socialCapitalRank: 1512
      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  "socialCapital": {
    "socialCapitalScore": 150,
    "socialCapitalRank": 100
  }
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Listen To A Certain FID When Social Capital Rank < X

You can use Airstack webhooks to listen to a certain FID when social capital rank is below certain rank number by using the following configuration:

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
    "event_type": "profile.updated",
    "filter": {
      "$and": [
        {
<strong>          "userId": "602"
</strong>        },
        {
          "socialCapital": {
            "socialCapitalRank": {
<strong>              "$lte": 100
</strong>            }
          }
        }
      ]
    },
    "payload": {    
<strong>      "userId": "602",
</strong>      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      "userAssociatedAddresses": [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      "profileName": "betashop.eth",
      "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      "isFarcasterPowerUser": false,
      "followingCount": 1170,
      "followerCount": 1080,
      "socialCapital": {
        "socialCapitalScore": 0.00151243059,
<strong>        "socialCapitalRank": 10
</strong>      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          socialCapital: {
            socialCapitalRank: {
<strong>              $lte: 100
</strong>            }
          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 1170,
      followerCount: 1080,
      socialCapital: {
        socialCapitalScore: 110,
        // social capital rank must be below 100
<strong>        socialCapitalRank: 10
</strong>      }
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
    event_type: 'profile.updated', // Listen to all profile updates
    filter: {
      $and: [
        {
<strong>          userId: "602"
</strong>        },
        {
          socialCapital: {
            socialCapitalRank: {
<strong>              $lte: 100
</strong>            }
          }
        }
      ]
    },
    payload: {    
<strong>      userId: "602", // userId must match value in filter
</strong>      userAddress: "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
      userAssociatedAddresses: [
        "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
        "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
        "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
      ],
      profileName: "betashop.eth",
      userRecoveryAddress: "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
      isFarcasterPowerUser: true,
      followingCount: 1170,
      followerCount: 1080,
      socialCapital: {
        socialCapitalScore: 110,
        // social capital rank must be below 100
<strong>        socialCapitalRank: 10
</strong>      }
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
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "602", // Match userId as specified in filter
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "betashop.eth",
  "profileTokenId": "602",
  "socialCapital": {
    "socialCapitalScore": 150,
    "socialCapitalRank": 10
  }
  // ...Other fields
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding creating webhook for listening to Farcaster profile update events, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Advanced Filter Patterns](../../webhooks-api-reference/overview/advanced-filter-patterns.md)
- [Webhooks API Reference](../../webhooks-api-reference/overview/)
