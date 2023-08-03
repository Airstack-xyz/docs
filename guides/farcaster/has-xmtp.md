---
description: >-
  Learn how to check if Farcaster name or ID have XMTP enabled on their account
  or not.
---

# 📬 Has XMTP

## Topics

* [Check Farcaster Has XMTP Enabled](has-xmtp.md#check-farcaster-has-xmtp-enabled)
* [Bulk Check Farcasters Have XMTP Enabled](has-xmtp.md#bulk-check-farcasters-have-xmtp-enabled)

## Check Farcaster Has XMTP Enabled

You can check if a Farcaster has XMTP enabled or not by providing either the Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/tHpIYqruY8" %}
Check Farcaster has XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "fc_fname:vbuterin"}}}) {
    XMTP {
      isXMTPEnabled
    }
  }
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

## Bulk Check Farcasters Have XMTP Enabled

You can bulk check a list of Farcasters have XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/uf0qK2hOaJ" %}
Check Farcaster Name and ID have XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: ["fc_fname:betashop.eth", "fc_fid:5650"]}}}) {
    XMTP {
      isXMTPEnabled
      owner {
        socials {
          dappName
          profileName
        }
      }
    }
  }
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
        },
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

## Developer Support

If you have any questions or need help regarding checking if Farcaster(s) have XMTP enabled, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Has XMTP](../has-xmtp/)
  * [Check Farcaster](../has-xmtp/check-single-user.md#by-farcaster-name-or-id)
  * [Bulk Check Farcasters](has-xmtp.md#bulk-check-farcasters-have-xmtp-enabled)
* [Farcaster Resolver](../../use-cases/farcaster/universal-resolver.md)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
