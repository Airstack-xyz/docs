---
description: Learn how to use Airstack to check if multiple users have XMTP or not.
---

# 👬 Check Multiple Users

To check multiple users if they have XMTP is similar to checking for single users.

Swap `_eq` comparator with `_in` to allow array inputs:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($addresses: [Identity!]) {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: $addresses}}}) {
    XMTP {
      isXMTPEnabled
      owner {
        addresses
        domains {
          name
        }
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

{% tab title="Variable" %}
```json
{
  "addresses": [
    "0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa",
    "shanemac.eth",
    "vitalik.lens"
  ]
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
            "addresses": [
              "0xa64af7f78de39a238ecd4fff7d6d410dbace2df0"
            ],
            "domains": [
              {
                "name": "shanemac.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "shanemac"
              },
              {
                "dappName": "lens",
                "profileName": "shanemac.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "satoshinart.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vbuterin"
              },
              {
                "dappName": "lens",
                "profileName": "vitalik.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "addresses": [
              "0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa"
            ],
            "domains": [],
            "socials": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The response will return fewer amount of element in the `XMTP` array if some of the `owner` input has no XMTP.&#x20;

Therefore, you can use `owner` field response to double check if in more details if a specific user has XMTP or not.
{% endhint %}

## Bulk Check Lens Profiles Have XMTP

### Try Demo

{% embed url="https://app.airstack.xyz/UxKTEE/rDe12B70P3" %}
Bulk Check Lens Profiles Have XMTP (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query BulkFetchPrimaryENSandXMTP($lens: [Identity!]) {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: $lens}}, limit: 100}) {
    XMTP {
      isXMTPEnabled
      owner {
        socials {
          dappName
          profileName
        }
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "lens": [
    "hoobastank.lens",
    "22776.lens",
    "barisadiguzel.lens"
  ]
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
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

