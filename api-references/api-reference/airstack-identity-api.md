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

# Airstack Identity API

Airstack Identity API has been integrated throughout all of the GraphQL queries!

You can filter your queries using the identity inputs by passing the following values instead of an EVM 0x address:

## **Solana Address**

You can enter the Solana addresses of a user directly in the `owner`, `address`, or `identity` field:

| Sample Input                                                                   | Description                    |
| ------------------------------------------------------------------------------ | ------------------------------ |
| <mark style="color:red;">`GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV`</mark> | Valid base58 Solana addresses  |

## **ENS** Domains

You can enter the [ENS domains](https://ens.domains), both on-chain or off-chain (e.g. [cb.id](https://cb.id) or [Namestone](https://namestone.xyz)), directly in the `owner`, `address`, or `identity` field:

| Sample Input                                                | Description                            |
| ----------------------------------------------------------- | -------------------------------------- |
| <mark style="color:red;">`vitalik.eth`</mark>               | Plain ENS domain ending in `.eth`      |
| <mark style="color:red;">`ens:vitalik.eth`</mark>           | ENS domain with optional `ens:` prefix |
| <mark style="color:red;">`leighton.pooltogether.eth`</mark> | Off-chain ENS domain from Namestone    |
| <mark style="color:red;">`yosephks.cb.id`</mark>            | Coinbase ID that is resolved off-chain |

## **Farcaster** ID and Name

You can enter Farcaster ID or name directly in the `owner`, `address`, or `identity` field:

| Sample Input                                        | Description                                                      |
| --------------------------------------------------- | ---------------------------------------------------------------- |
| <mark style="color:red;">`fc_fid:5650`</mark>       | Farcaster user ID with required `fc_fid` prefix                  |
| <mark style="color:red;">`fc_fname:vbuterin`</mark> | Farcaster user name (not ENS) with required `fc_fname` prefix    |
| <mark style="color:red;">`fc_fname:dwr.eth`</mark>  | Farcaster user name (ENS domain) with required `fc_fname` prefix |

Alternatively, you can enter the Farcaster profile name directly in the Socials `profileName` field.

{% hint style="info" %}
You can enter all fnames that Farcaster user has, including the one that is not actively used as a profile name.\
\
For example, a Farcaster user have multiple fnames: `fc_fname:varunsrin.eth` and `fc_fname:v`, then you should be able to use both as an input of any identity field.
{% endhint %}
