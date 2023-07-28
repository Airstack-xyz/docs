---
description: >-
  Learn how to fetch the list of common POAP holders or event attendees of
  multiple POAPs.
---

# Multiple POAPs

## Topics

* [Best Practices](multiple-poaps.md#best-practices)
* [Common Holders of 2 POAPs](multiple-poaps.md#common-holders-of-2-poaps)
* [Common Holders of More Than 2 POAPs](multiple-poaps.md#common-holders-of-more-than-2-poaps)

## Best Practices

With [nested queries](../../api-references/nested-queries.md), Airstack finds the intersection of common holders of multiple POAPs in the following order:

1. Filtering POAP holders on the 1st outermost query
2. Comparing POAP holders of 1st outermost query against the POAP holders on 2nd outermost query
3. Comparing POAP holders of the 1st and 2nd outermost query against the POAP holders on the 3rd outermost query
4. Comparing POAP holders of 1st, 2nd, ..., nth outermost query against the POAP holders on (n + 1)-th outermost query

Since the number of objects returned in the responses will be dependent on the number of POAP holders on the 1st outermost query, it's most efficient that you have the **POAP with the least amount of holders** on the 1st outermost query.

{% hint style="info" %}
Suppose there are two POAPs:

* POAP A: 100,000 holders
* POAP B: 1,000 holders.

If POAP A is the input on the 1st outermost query, then the end result will be **100,000 objects** in the response array.

On the other hand, if POAP B is the input on the 1st outermost query, then the end result will be instead **ONLY** 1,000 objects in the response array.

The latter approach will be more efficient and easier for further formatting.
{% endhint %}

## Common Holders of 2 POAPs

### Fetching

You can fetch the common holders of two given POAP event IDs, e.g. [EthGlobal Lisbon 2023 Partner Attendee POAP](https://explorer.airstack.xyz/token-holders?address=127462\&blockchain=\&rawInput=%23%E2%8E%B1127462%E2%8E%B1%28127462++++ID\_POAP%29\&inputType=POAP) & [EthCC\[6\] Attendee POAP](https://explorer.airstack.xyz/token-holders?address=141910\&blockchain=gnosis\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29+\&inputType=POAP):

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfEthGlobalLisbonAndEthCC {
<strong>  Poaps(input: {filter: {eventId: {_eq: "127462"}}, blockchain: ALL, limit: 200}) {
</strong>    Poap {
      owner {
<strong>        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: "141910"}}}) {
</strong>          owner {
            addresses
          }
        }
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
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "addresses": [
                    "0xa960bcc07fdd90c94cbb40de105ad909f7012a9c"
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have EthGlobal Lisbon, but no EthCC[6]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) => owner?.addresses)
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'Poaps' in data and 'Poap' in data['Poaps']:
        for item in data['Poaps']['Poap']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'addresses' in poap['owner']:
                        result.append(poap['owner']['addresses'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  "0xc77d249809ae5a118eef66227d1a01a3d62c82d4",
  "0x3291e96b3bff7ed56e3ca8364273c5b4654b2b37",
  "0xe348c7959e47646031cea7ed30266a6702d011cc",
  // ...other token holders
  "0xa69babef1ca67a37ffaf7a485dfff3382056e78c",
  "0x46340b20830761efd32832a74d7169b29feb9758",
  "0x2008b6c3d07b061a84f790c035c2f6dc11a0be70"
]
```

## Common Holders of More Than 2 POAPs

### Fetching

Fetching common holders for more than 2 POAPs works the same way as fetching only 2 POAPs with the addition of more POAP event ID parameters and additional nesting as shown below:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfMoreThanTwoPOAPs {
  Poaps(input: {filter: {eventId: {_eq: "80393"}}, blockchain: ALL, limit: 200}) {
    Poap {
      owner {
        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: "79011"}}}) {
          owner {
<strong>            poaps(input: {blockchain: ALL, filter: {eventId: {_eq: "15678"}}}) {
</strong>              owner {
                addresses
              }
            }
          }
        }
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
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "poaps": [
                    {
                      "owner": {
                        "addresses": [
                          "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Don't have all 3 POAPs
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' addresses will be returned inside the innermost `owner.addresses` field.

### Formatting

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) =>
      owner?.poaps?.map(({ owner }) => owner?.addresses)
    )
  )
    .filter(Boolean)
    .flat(3)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'Poaps' in data and 'Poap' in data['Poaps']:
        for item in data['Poaps']['Poap']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'poaps' in poap['owner']:
                        for nested_poap in poap['owner']['poaps']:
                            if 'owner' in nested_poap and 'addresses' in nested_poap['owner']:
                                result.append(nested_poap['owner']['addresses'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Nested Queries](../../api-references/nested-queries.md)
