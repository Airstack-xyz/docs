---
description: Learn how to use Airstack to check if a single user has XMTP or not.
---

# 🧍 Check Single User

## By 0x Address

Simply use the following query to check if a user has XMTP or not:

```graphql
query MyQuery($address: Identity!) {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: $address}}}) {
    XMTP {
      isXMTPEnabled
    }
  }
}
```

If a user has XMTP, then `isXMTPEnabled` will be `true`. Otherwise, the whole response object will be `null`.

Here's an example of a user that **has an XMTP**:

{% tabs %}
{% tab title="Variable" %}
```json
{
  "address": "0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Here's an example of a user that **does not have any XMTP**:

{% tabs %}
{% tab title="Variable" %}
```json
{
  "address": "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"
}
```
{% endtab %}
{% endtabs %}

## By ENS

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input to any ENS names:

{% tabs %}
{% tab title="Variable" %}
```json
{
  "address": "vitalik.eth"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## By Lens Profile

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input to any Lens Profile:

{% tabs %}
{% tab title="Variable" %}
```json
{
  "address": "vitalik.lens"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## By Farcaster Name or ID

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input to any Farcaster Name or ID:

{% tabs %}
{% tab title="Variable" %}
```json
{
  "address": "fc_fname:vbuterin"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}
