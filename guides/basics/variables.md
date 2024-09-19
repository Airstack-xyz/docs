---
description: >-
  Learn how you can add variables to an Airstack GraphQL query to provide
  dynamic inputs into the API.
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

# 🌪️ Variables

You can add variables to an [Airstack](https://airstack.xyz) GraphQL query to provide dynamic inputs.

To do so, simply define the variable with `$` prefix and specify the correct typing.

As an example, below is shown on how a query looks like **after** and **before** adding variable to an [Airstack](https://airstack.xyz) query:

{% tabs %}
{% tab title="After" %}
<pre class="language-graphql"><code class="lang-graphql"><strong>query MyQuery($fid: String) { # Define the variable with its typing
</strong>  Socials(
    input: {
      filter: {
<strong>        userId: {_eq: $fid} # replace the value with the variable
</strong>      },
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Before" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        userId: {_eq: "3"}
      }, 
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
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
* [API References](../../api-references/api-reference/)
* [Direct API Call](../../get-started/quickstart/direct-api-call.md)
* [Multiple Queries Execution](multiple-queries-execution.md)
* [Cross Chain Queries](broken-reference)
