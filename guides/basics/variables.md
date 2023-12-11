---
description: >-
  Learn how you can add variables to an Airstack GraphQL query to provide
  dynamic inputs into the API.
---

# 🌪 Variables

You can add variables to an [Airstack](https://airstack.xyz) GraphQL query to provide dynamic inputs.

To do so, simply define the variable with `$` prefix and specify the correct typing.

As an example, below is shown on how a query looks like **after** and **before** adding variable to an [Airstack](https://airstack.xyz) query:

{% tabs %}
{% tab title="After" %}
<pre class="language-graphql"><code class="lang-graphql"><strong>query MyQuery($tokenAddress: Address) { # Define variable on the top level with the correct typing
</strong>  TokenBalances(
    input: {
      filter: {
        tokenAddress: {
<strong>          _eq: $tokenAddress # Variable as an input
</strong>        }
      },
      blockchain: ethereum
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          isPrimary
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Before" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: {
          _eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"
        }
      },
      blockchain: ethereum
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          isPrimary
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding adding variables into your Airstack query, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [API Overview](../../api-references/overview/cursor-pagination/overview.md)
* [API References](../../api-references/api-reference/)
