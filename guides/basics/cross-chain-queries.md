---
description: >-
  Learn how you can do cross-chain queries using Airstack GraphQL API to fetch
  data more efficiently in a single request.
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

# ♋ Cross Chain Queries

In GraphQL, you can call multiple queries in parallel in a single request as shown in [Multiple Queries Execution](multiple-queries-execution.md).

As [Airstack](https://airstack.xyz) is a GraphQL API, it inherits this feature that enables you to call multiple APIs in one request, which is particularly useful for building cross-chain queries to fetch data across multiple [Airstack-supported chains](../overview.md#supported-chains):

### Try Demo

{% embed url="https://app.airstack.xyz/query/3eBk5PMyBM" %}
Show ERC20 tokens on Ethereum, Base, and Zora owned by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query ERC20OwnedByLensProfiles {
<strong>  Ethereum: TokenBalances( # first query fetch Ethereum ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      formattedAmount
      tokenAddress
      token {
        name
        symbol
      }
    }
  }
<strong>  Base: TokenBalances( # second query fetch Base ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      formattedAmount
      tokenAddress
      token {
        name
        symbol
      }
    }
  }
<strong>  Zora: TokenBalances( # third query fetch Zora ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: zora
      limit: 50
    }
  ) {
    TokenBalance {
      formattedAmount
      tokenAddress
      token {
        name
        symbol
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "formattedAmount": 10860.787005472,
          "tokenAddress": "0x6390ee51738652b4f22c20f0fd02275cbd5ff685",
          "token": {
            "name": "From Vitalik",
            "symbol": "Vitalik"
          }
        }
        // Other Ethereum ERC20s
      ]
    },
    "Base": {
      "TokenBalance": [
        {
          "formattedAmount": 1420,
          "tokenAddress": "0x4ed4e862860bed51a9570b96d89af5e1b0efefed",
          "token": {
            "name": "Degen",
            "symbol": "DEGEN"
          }
        }
        // Other Base ERC20s
      ]
    },
    "Zora": {
      "TokenBalance": [
        {
          "formattedAmount": 100,
          "tokenAddress": "0x6c99db030dcc2c4d2e464c481812db078f4b30cc",
          "token": {
            "name": "ZOO",
            "symbol": "zoo"
          }
        }
        // Other Zora ERC20s
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding adding variables into your Airstack query, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [API Overview](../../api-references/overview/)
* [Variables Guides](variables.md)
* [Direct API Call](../../get-started/quickstart/direct-api-call.md)
* [Multiple Queries Execution](multiple-queries-execution.md)
* [TokenBalances API References](broken-reference)
