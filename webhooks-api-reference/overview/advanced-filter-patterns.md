---
description: >-
  Learn various filter combinations to build both simple and complex filter for
  only listening to certain Farcaster events.
---

# ⚙️ Advanced Filter Patterns

Subscription filtering is used to decide what events an endpoint will receive based on the webhook event payload. The subscription filter is an enriched JSON syntax for both simple and complex filters (such as special logical and arithmetic operators `$or`, `$gte`, `$eq`).

## [​](https://docs.getconvoy.io/product-manual/subscriptions#simple-filters)Simple filters <a href="#simple-filters" id="simple-filters"></a>

Simple filters directly match keys to values, and they can be nested. They can also match items in an array.

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#direct-object-match)**Direct object match**

This filter is used to validate simple JSON webhook payloads.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "userId": "602"
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
<strong>  "userId": "602", // Match userId with value "602"
</strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#nested-object-match)**Nested object match**

This filter is used to validate nested webhook payloads.

{% hint style="info" %}
Airstack webhooks filter only accept nested filters up to **3 levels**. More than 3-level nesting will result in error.
{% endhint %}

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "socialCapital": {
    "socialCapitalRank": 1
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  "socialCapital": {
    "socialCapitalScore": 279,
    // match socialCapital.socialCapitalRank with exact value 1
<strong>    "socialCapitalRank": 1
</strong>  },
  "profileDisplayName": "dwr",
  "profileImage": "https://i.imgur.com/Y1au7ZB.jpg",
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#array-contains-match)**Array contains match**

This filter is used to validate if there is any match in an array field in the webhook payloads.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "userAssociatedAddresses": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a"
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  "userId": "3761",
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    // Match with any result that contain the mentioned address
    // in the array
<strong>    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
</strong>    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

## [​](https://docs.getconvoy.io/product-manual/subscriptions#complex-filters)Complex filters <a href="#complex-filters" id="complex-filters"></a>

Complex filters contain more logic such as logical operators and special operators. Complex filters are employed to filter events using one or more conditions, e.g., `$or` logical operator filter.

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#neq-filter)**$neq**

This filter matches event which directly does not match the event type in the webhook payload.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "userId": {
    "$neq": "1"
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  // Match with any payload that doesn't have userId value 1
<strong>  "userId": "602", 
</strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#or-and-and-filters)**$or**

This filter is used to match multiple conditions evaluated using **OR** logic as defined in the schema.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "$or": [
    {
      "userId": "602"
    },
    {
      "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a"
    }
  ]
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  // Match either the userId value or userAddress value
<strong>  "userId": "602",
</strong><strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
</strong>  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### $and

This filter is used to match multiple conditions evaluated using **AND** logic as defined in the schema. This can also be combined with other operators such as `$or`.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "$and": [
    {
      "$.socialCapital.$.socialCapitalRank": {
        "$lte": 100
      }
    },
    {
      "$or": [
        {
          "userId": "602"
        },
        {
          "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a"
        }
      ]
    }
  ]
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
<strong>  "userId": "602",
</strong><strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
</strong>  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "socialCapital": {
<strong>    "socialCapitalRank": 70,
</strong>    "socialCapitalScore": 100
  },
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#array-contains-filters)**Array Contains filters**

This filter is used to match keys where the value can be a range of values. It can be used in place of `$or` if you are comparing on the same field. The opposite of this operator `$nin` can be used to match keys where the value is not in a range of values.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "userId": {
    "$in": ["602", "2602"]
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
<strong>  "userId": "602", // 602 is inside the $in operator
</strong>  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "socialCapital": {
    "socialCapitalRank": 70,
    "socialCapitalScore": 100
  },
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#arithmetic-filters)**Arithmetic filters**

These filters match events based on arithmetic operators. For example, the filter below will match all events where the age is greater than 1. The operators supported are `$gt`, `$gte`, `$lt`, `$lte`

{% tabs %}
{% tab title="Filter Schema " %}
```json
{
  "socialCapital": {
    "socialCapitalRank": {
      "$lte": 100
    }
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  "socialCapital": {
    "socialCapitalScore": 279,
    // Only match payload with socialCapital.socialCapitalRank &#x3C;= 100
<strong>    "socialCapitalRank": 1
</strong>  },
  "profileDisplayName": "dwr",
  "profileImage": "https://i.imgur.com/Y1au7ZB.jpg",
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#array-positional-filters)**Array positional filters**

These filters match events with payloads that are array either in the root or nested.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "$.socialCapital.$.socialCapitalRank": {
    "$lte": 100
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  "socialCapital": {
    "socialCapitalScore": 279,
    // Only match payload with socialCapital.socialCapitalRank &#x3C;= 100
<strong>    "socialCapitalRank": 1
</strong>  },
  "profileDisplayName": "dwr",
  "profileImage": "https://i.imgur.com/Y1au7ZB.jpg",
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

### [**​**](https://docs.getconvoy.io/product-manual/subscriptions#regex-filters)**Regex filters**

These filters match events with payloads that match the supplied regex.

{% tabs %}
{% tab title="Filter Schema" %}
```json
{
  "profileName": {
    "$regex": "^betashop"
  }
}
```
{% endtab %}

{% tab title="Matched Event Payload" %}
<pre class="language-json"><code class="lang-json">{
  // Match with profileName that starts with "betashop"
<strong>  "profileName": "betashop.eth",
</strong>  "userId": "602",
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  // ...Other fields
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding defining custom filters for your use cases, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Quickstart Guide](../../guides/webhooks/quickstart.md)
* [Managing Webhooks](../../guides/webhooks/managing-webhooks.md)
* [Webhooks API Reference](./)
