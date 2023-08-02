---
description: Learn how to check if Lens profiles have XMTP enabled on their account or not.
---

# 📬 Has XMTP

## Topics

* [Check Lens Profile Has XMTP Enabled](has-xmtp.md#check-lens-profile-has-xmtp-enabled)
* [Bulk Check Lens Profiles Have XMTP Enabled](has-xmtp.md#bulk-check-lens-profiles-have-xmtp-enabled)

## Check Lens Profile Has XMTP Enabled

You can check if a Lens profile has XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/38TR2Gxuqf" %}
Check Lens Profile has XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "vitalik.lens"}}}) {
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

## Bulk Check Lens Profiles Have XMTP Enabled

You can bulk check a list of Lens profiles have XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4wzNFy5ceS" %}
Bulk Check Lens Profiles Have XMTP (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query BulkFetchLensProfilesHaveXMTP {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: ["hoobastank.lens", "22776.lens", "barisadiguzel.lens"]}}, limit: 100}) {
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
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "hoobastank.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "22776.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "barisadiguzel.lens"
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding checking if Lens profile(s) have XMTP enabled, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Has XMTP](../has-xmtp/)
  * [Check Lens Profile](../has-xmtp/check-single-user.md#by-lens-profile)
  * [Bulk Check Lens Profiles](../has-xmtp/check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
* [Lens Resolver](../../use-cases/lens/universal-resolver.md)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
