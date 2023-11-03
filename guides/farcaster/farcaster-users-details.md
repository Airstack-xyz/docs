---
description: >-
  Learn how to fetch Farcaster user details with 0x address, ENS domain,
  Farcaster name and ID, and Lens profile name and ID
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

# 🔎 Farcaster Users Details

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

In this tutorial, you will learn how to fetch Farcaster user details with various [web3 identities](#user-content-fn-1)[^1]. User details include FID, Username(s), associated Ethereum addresses, registration date, and more.

In this guide you will learn how to use Airstack to:

* [Get Farcaster Profile Details By Farcaster ID](farcaster-users-details.md#get-farcaster-profile-details-by-farcaster-id)
* [Get Farcaster Profile Details By Farcaster Name](farcaster-users-details.md#get-farcaster-profile-details-by-farcaster-name)
* [Get Farcaster Profile Details By 0x address](farcaster-users-details.md#get-farcaster-profile-details-by-0x-address)
* [Get Farcaster Profile Details By ENS Domain](farcaster-users-details.md#get-farcaster-profile-details-by-ens-domain)
* [Get Farcaster Profile Details By Lens Profile Name](farcaster-users-details.md#get-farcaster-profile-details-by-lens-profile-name)
* [Get Farcaster Profile Details By Lens Profile ID](farcaster-users-details.md#get-farcaster-profile-details-by-lens-profile-id)
* [Bulk Query Farcaster Profile Details](farcaster-users-details.md#bulk-query-farcaster-profile-details)
* [Bulk Query All Farcaster Users Profile Details](farcaster-users-details.md#bulk-query-all-farcaster-users-profile-details)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
**React**

```sh
npm install @airstack/airstack-react
```

**Node**

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
**React**

```sh
yarn add @airstack/airstack-react
```

**Node**

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
**React**

```sh
pnpm install @airstack/airstack-react
```

**Node**

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);
  
  if (data) {
    return <p>Data: {JSON.stringify(data)}</p>;
  }

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }
};
```
{% endtab %}

{% tab title="Node" %}
```javascript
import { init, fetchQuery } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const { data, error } = await fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """YOUR_QUERY""" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Farcaster Profile Details By Farcaster ID

You can get the Farcaster user profile details by their Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/zHwW2c2wnJ" %}
Show all farcaster profile details and registration info for farcaster user fc\_fid:3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "fc_fid:3"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "fc_fid:3",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Profile Details By Farcaster Name

You can get the Farcaster user profile details by their Farcaster Name:

### Try Demo

{% embed url="https://app.airstack.xyz/query/KbIaoE7JjM" %}
Show all farcaster profile details and registration info for farcaster user fc\_fname:dwr.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "fc_fname:dwr.eth"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Profile Details By 0x address

You can get the Farcaster user profile details by their 0x address:

### Try Demo

{% embed url="https://app.airstack.xyz/query/7joKY6qfWy" %}
Show all farcaster profile details and registration info for 0x74232bf61e994655592747e20bdf6fa9b9476f79
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "0x74232bf61e994655592747e20bdf6fa9b9476f79"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Profile Details By ENS Domain

You can get the Farcaster user profile details by their ENS domain:

### Try Demo

{% embed url="https://app.airstack.xyz/query/9Q0GxX7OzC" %}
Show all farcaster profile details and registration info for dwr.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "dwr.eth"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "dwr.eth",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Profile Details By Lens Profile Name

You can get the Farcaster user profile details by their Lens profile name:

### Try Demo

{% embed url="https://app.airstack.xyz/query/qw4LjDtYDm" %}
Show all farcaster profile details and registration info for danromero.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "danromero.lens"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "danromero.lens",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Profile Details By Lens Profile ID

You can get the Farcaster user profile details by their Lens profile ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/YxNJMrJE5R" %}
Show all farcaster profile details and registration info for lens\_id:0x0b46d
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_eq: "lens_id:0x0b46d"}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "lens_id:0x0b46d",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Bulk Query Farcaster Profile Details

You can bulk query the profile details of multiple Farcaster users by their various web3 identities:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4AI9nPEwjs" %}
Show all farcaster profile details and registration info for fc\_fname:dwr.eth and vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_in: ["fc_fname:dwr.eth", "vitalik.eth"]}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "44ece1efa9a836358b4fbe3b4307504f001fa39b1048224c18460a9d2de76e98",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "5650",
          "userAddress": "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:32:05Z",
          "userCreatedAtBlockNumber": 108874774,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:32:05Z",
          "userLastUpdatedAtBlockNumber": 108874774,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileBio": "hullo",
          "profileDisplayName": "Vitalik Buterin",
          "profileImage": "https://i.imgur.com/gF9Yaeg.jpg",
          "profileUrl": "",
          "profileName": "vitalik.eth",
          "profileTokenId": "5650",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:32:05Z",
          "profileCreatedAtBlockNumber": 108874774,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:32:05Z",
          "profileLastUpdatedAtBlockNumber": 108874774,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "vitalik.eth",
            "vbuterin"
          ]
        },
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

If you want want to get Farcaster user with fid in certain range, e.g. fid 1 to 3, then you can use the fid as an array to the `userId` input to fetch the data as shown below:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pJYR1DlOeE" %}
Show all Farcaster user with fid 1, 2, 3 profile details and registration info
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, userId: {_in: ["1", "2", "3"]}}, blockchain: ethereum}
  ) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "9732b5a6604e59aae6336e83f69ab6277c06d091c30a0aca3c98a9bdb13ab631",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "2",
          "userAddress": "0x4114e33eb831858649ea3702e1c9a2db3f626446",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0x182327170fc284caaa5b1bc3e3878233f529d741",
            "0x91031dcfdea024b4d51e775486111d2b2a715871",
            "0x4114e33eb831858649ea3702e1c9a2db3f626446"
          ],
          "profileBio": "Technowatermelon. Elder Millenial. Building Farcaster. \n\nnf.td/varun",
          "profileDisplayName": "Varun Srinivasan",
          "profileImage": "https://i.seadn.io/gae/sYAr036bd0bRpj7OX6B-F-MqLGznVkK3--DSneL_BT5GX4NZJ3Zu91PgjpD9-xuVJtHq0qirJfPZeMKrahz8Us2Tj_X8qdNPYC-imqs?w=500&auto=format",
          "profileUrl": "",
          "profileName": "varunsrin.eth",
          "profileTokenId": "2",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "varunsrin.eth",
            "v"
          ]
        },
        {
          "id": "c3f6ffbb20532bf50d158f4099336d50d5ba9f327159712972c94d51aa4730ee",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "profileBio": "Working on Farcaster and Warpcast.",
          "profileDisplayName": "Dan Romero",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "profileUrl": "",
          "profileName": "dwr.eth",
          "profileTokenId": "3",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "dwr.eth",
            "danromero.eth",
            "dwr"
          ]
        },
        {
          "id": "df59444a0a4e5799c08cdc4d52b05832757d97ee4f44b9e023807bc2255d5218",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "1",
          "userAddress": "0x8773442740c17c9d0f0b87022c722f9a136206ed",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userCreatedAtBlockNumber": 108874508,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "userLastUpdatedAtBlockNumber": 108874508,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0x8773442740c17c9d0f0b87022c722f9a136206ed",
            "0x86924c37a93734e8611eb081238928a9d18a63c0"
          ],
          "profileBio": "A sufficiently decentralized social network. farcaster.xyz",
          "profileDisplayName": "Farcaster",
          "profileImage": "https://i.imgur.com/I2rEbPF.png",
          "profileUrl": "",
          "profileName": "farcaster",
          "profileTokenId": "1",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileCreatedAtBlockNumber": 108874508,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:23:13Z",
          "profileLastUpdatedAtBlockNumber": 108874508,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "warpcast.eth",
            "farcaster.eth",
            "farcaster"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For larger range, it is recommended that you generate the array programmatically instead of manually typing them.\
\
It is best practice that you keep the size of the array to a **maximum length of 200** per API call.
{% endhint %}

## Bulk Query All Farcaster Users Profile Details

You can bulk query the profile details of all Farcaster users:

### Try Demo

{% embed url="https://app.airstack.xyz/query/YY02CjXaPG" %}
Show all farcaster users profile details and registration info
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(input: {filter: {dappName: {_eq: farcaster}}, blockchain: ethereum}) {
    Social {
      id
      chainId
      blockchain
      dappName
      dappSlug
      dappVersion
      userId
      userAddress
      userCreatedAtBlockTimestamp
      userCreatedAtBlockNumber
      userLastUpdatedAtBlockTimestamp
      userLastUpdatedAtBlockNumber
      userHomeURL
      userRecoveryAddress
      userAssociatedAddresses
      profileBio
      profileDisplayName
      profileImage
      profileUrl
      profileName
      profileTokenId
      profileTokenAddress
      profileCreatedAtBlockTimestamp
      profileCreatedAtBlockNumber
      profileLastUpdatedAtBlockTimestamp
      profileLastUpdatedAtBlockNumber
      profileTokenUri
      isDefault
      identity
      fnames
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "id": "000158147aca2d61c855b4a0199be5d84e1d7c79d5385daa1fd2a75fde79aa46",
          "chainId": "10",
          "blockchain": "optimism",
          "dappName": "farcaster",
          "dappSlug": "farcaster_optimism",
          "dappVersion": "v2",
          "userId": "14980",
          "userAddress": "0x0e19bbc1cf8d38a1e5dfee3b2b8029ad4ccb980e",
          "userCreatedAtBlockTimestamp": "2023-08-29T22:47:09Z",
          "userCreatedAtBlockNumber": 108875226,
          "userLastUpdatedAtBlockTimestamp": "2023-08-29T22:47:09Z",
          "userLastUpdatedAtBlockNumber": 108875226,
          "userHomeURL": "",
          "userRecoveryAddress": "0x00000000fcd5a8e45785c8a4b9a718c9348e4f18",
          "userAssociatedAddresses": [
            "0xb64c53fe28949054dfa164fd6dc302b94f699755",
            "0x0e19bbc1cf8d38a1e5dfee3b2b8029ad4ccb980e"
          ],
          "profileBio": "Anti-minimalist AI-collaborative artist\n| Work exhibited in NYC, London, Liverpool, Hong Kong, Seattle, Tokyo, Toronto and Rome and collected by hundreds.",
          "profileDisplayName": "Maneki Neko",
          "profileImage": "https://i.seadn.io/gcs/files/6406664e1c50bb12dcf7805c99a18f8a.jpg?w=500&auto=format",
          "profileUrl": "",
          "profileName": "manekineko",
          "profileTokenId": "14980",
          "profileTokenAddress": "0x00000000fcaf86937e41ba038b4fa40baa4b780a",
          "profileCreatedAtBlockTimestamp": "2023-08-29T22:47:09Z",
          "profileCreatedAtBlockNumber": 108875226,
          "profileLastUpdatedAtBlockTimestamp": "2023-08-29T22:47:09Z",
          "profileLastUpdatedAtBlockNumber": 108875226,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "",
          "fnames": [
            "manekineko"
          ]
        },
        // Other farcaster users
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster user(s) profile details, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Resolve Identities](../resolve-identities/)
  * [Farcaster](../resolve-identities/farcaster.md)
* [Farcaster Resolver](../../use-cases/farcaster/universal-resolver.md)
* [Socials API Reference](../../api-references/api-reference/socials-api/)

1. 0x address, ENS domain, Lens profile name and ID, Farcaster name and ID

[^1]: 0x addresses, ENS domains, Lens, XMTP
