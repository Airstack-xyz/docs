---
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

# XMTPs API Examples

<details>

<summary>Check if shanemac.eth has XMTP</summary>

```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "shanemac.eth"}}}) {
    XMTP {
      isXMTPEnabled
    }
  }
}
```

</details>

<details>

<summary>Show me vitalik.lens xmtp and primary domain</summary>

```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "vitalik.lens"}}}) {
    XMTP {
      isXMTPEnabled
      owner {
        primaryDomain {
          name
        }
      }
    }
  }
}
```

</details>

