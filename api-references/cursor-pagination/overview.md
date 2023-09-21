---
description: >-
  Learn how does cursor pagination works in Airstack API to paginate through all
  the data returned.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# Overview

## How Does It Work?

Each response result from an API call is a **page** and is indicated by a `cursor` value.

Thus, in order to go to the next (or previous) page, you will need to get the `cursor` value for the next (or previous) page, which is provided in every Airstack GraphQL schema as `pageInfo.nextCursor` (or `pageInfo.prevCursor`):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/gyvtfKZmiI" %}
Show me all holders of @BoredApeYachtClub (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        identity
      }
    }
    pageInfo {
<strong>      nextCursor # cursor to next page (2nd page)
</strong><strong>      prevCursor # cursor to prev page
</strong>    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenBalances": {
      // Token holders from the 1st page result
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xbc8dd02976bdfbda4f4146a89445ee66e28806da"
          }
        },
        {
          "owner": {
            "identity": "0xcafba4ca4a4886ae0938cc8ef91b605db9e4bf08"
          }
        },
        {
          "owner": {
            "identity": "0xdbfd76af2157dc15ee4e57f3f942bb45ba84af24"
          }
        }
      ],
      "pageInfo": {
        // `nextCursor`: Cursor for the next page (2nd page)
<strong>        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QweDMzYzFmZWZmYmY3MjE3ZDdiMjI0NGRjOTA1MWYwNGM0OTdhMDRhMDI1Nzc4IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjkyOTE4MTc5IiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
</strong>        // `prevCursor`: Cursor for the prev page (empty for 1st page)
<strong>        "prevCursor": ""
</strong>      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If `pageInfo.nextCursor` or `pageInfo.prevCursor` is returned as an empty string, then it implies that there is no more next or previous page, respectively.
{% endhint %}

Once you have the cursor for the next page, you can use the cursor value to retrieve data from the 2nd page by making another API call with the `pageInfo.nextCursor` value provided in the `cursor` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ypsxxeH9TC" %}
Show me all holders of @BoredApeYachtClub on the 2nd page (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: {
          _eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"
        }
      },
      blockchain: ethereum,
      limit: 200,
      # Cursor of the 2nd page
<strong>      cursor: "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QweDMzYzFmZWZmYmY3MjE3ZDdiMjI0NGRjOTA1MWYwNGM0OTdhMDRhMDI1Nzc4IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjkyOTE4MTc5IiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ=="
</strong>    }
  ) {
    TokenBalance {
      owner {
        identity
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenBalances": {
      // The token holders from the 2nd page result
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xf15c93562bc3944a68e938ef75d2a3360d98ca57"
          }
        },
        {
          "owner": {
            "identity": "0xdbfd76af2157dc15ee4e57f3f942bb45ba84af24"
          }
        },
        {
          "owner": {
            "identity": "0xdbfd76af2157dc15ee4e57f3f942bb45ba84af24"
          }
        }
      ],
      "pageInfo": {
        // `nextCursor`: cursor to the next page (3rd page)
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QweGIwZmZjNzhjODMyZDJjZjdmY2M2M2Y3OTIzOGZkZDcyMGI1Y2VlMzUxMDY0IiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjkyNjM2MDIzIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        // `prevCursor`: cursor to the prev page (1st page)
        "prevCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QweGYxNWM5MzU2MmJjMzk0NGE2OGU5MzhlZjc1ZDJhMzM2MGQ5OGNhNTc4NzYwIiwiRGF0YVR5cGUiOiJzdHJpbmcifSwibGFzdFVwZGF0ZWRUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjkyOTE1NjExIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6IlBSRVYifQ=="
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding how cursor pagination work, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.
