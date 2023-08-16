---
description: >-
  Learn how to fetch Lens profile details with 0x address, ENS domain, Lens
  profile name and ID, and Farcaster name and ID.
---

# 🔎 Lens Profile Details

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to fetch Lens profile details with various [web3 identities](#user-content-fn-1)[^1]. Profile details include Lens profile name, profile ID, associated Ethereum addresses, registration date, and more.

In this guide you will learn how to use Airstack to:

* [Get Lens Profile Details By Farcaster ID](lens-profile-details.md#get-lens-profile-details-by-farcaster-id)
* [Get Lens Profile Details By Farcaster Name](lens-profile-details.md#get-lens-profile-details-by-farcaster-name)
* [Get Lens Profile Details By 0x address](lens-profile-details.md#get-lens-profile-details-by-0x-address)
* [Get Lens Profile Details By ENS Domain](lens-profile-details.md#get-lens-profile-details-by-ens-domain)
* [Get Lens Profile Details By Lens Profile Name](lens-profile-details.md#get-lens-profile-details-by-lens-profile-name)
* [Get Lens Profile Details By Lens Profile ID](lens-profile-details.md#get-lens-profile-details-by-lens-profile-id)
* [Bulk Query Lens Profile Details](lens-profile-details.md#bulk-query-lens-profile-details)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

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

## Get Lens Profile Details By Farcaster ID

You can get the Lens profile details by their Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/PXvk55hBBU" %}
Show all lens profile details and registration info for farcaster user fc\_fid:5650
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "fc_fid:3"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "fc_fid:5650"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile Details By Farcaster Name

You can get the Lens profile details by their Farcaster Name:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/wJvbCHRULZ" %}
Show all lens profile details and registration info for farcaster user fc\_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "fc_fname:vitalik.eth"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "fc_fname:vitalik.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile Details By 0x address

You can get the Lens profile details by their 0x address:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/LfkdEgsNU3" %}
Show all lens profile details and registration info for 0xd8da6bf26964af9d7eed9e03e53415d37aa96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile Details By ENS Domain

You can get the Farcaster user profile details by their ENS domain:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/v4NtCmLXSB" %}
Show all lens profile details and registration info for vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "vitalik.eth"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "vitalik.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile Details By Lens Profile Name

You can get the Lens profile details by their Lens profile name:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/JjYZwRJ2Sg" %}
Show all lens profile details and registration info for vitalik.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "vitalik.lens"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "vitalik.lens"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Profile Details By Lens Profile ID

You can get the Lens profile details by their Lens profile ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/OAWbkc4BG7" %}
Show all lens profile details and registration info for lens\_id:0x0187b3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_eq: "lens_id:0x0187b3"}}, blockchain: ethereum}
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
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": "lens_id:0x0187b3"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Bulk Query Lens Profile Details

You can bulk query the profile details of multiple Lens profiles by their various [web3 identities](#user-content-fn-2)[^2]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lmpub2yKiR" %}
Show all lens profile details and registration info for stani.lens and vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "vitalik.eth"]}}, blockchain: ethereum}
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
          "id": "15f737a892e56d09064da17d1e5b8d339361b87b6f1ea378ae1c8f99504b6502",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0x7241dddec3a6af367882eaf9651b87e1c7549dff",
          "userAddress": "0x7241dddec3a6af367882eaf9651b87e1c7549dff",
          "userCreatedAtBlockTimestamp": "2023-02-10T19:45:20Z",
          "userCreatedAtBlockNumber": 39146497,
          "userLastUpdatedAtBlockTimestamp": "2023-02-10T19:45:20Z",
          "userLastUpdatedAtBlockNumber": 39146497,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
          ],
          "profileName": "stani.lens",
          "profileTokenId": "5",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-05-17T17:36:56Z",
          "profileCreatedAtBlockNumber": 28437340,
          "profileLastUpdatedAtBlockTimestamp": "2022-08-17T23:00:35Z",
          "profileLastUpdatedAtBlockNumber": 32011659,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": ""
        },
        {
          "id": "57e3c7ae14d823399f2e2c6c57c6c926766b4ca37ff60908822d91297eb13986",
          "chainId": "137",
          "blockchain": "polygon",
          "dappName": "lens",
          "dappSlug": "lens_polygon",
          "dappVersion": "polygon",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userCreatedAtBlockNumber": 35512897,
          "userLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "userLastUpdatedAtBlockNumber": 35512897,
          "userHomeURL": "",
          "userRecoveryAddress": "",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenAddress": "0xdb46d1dc155634fbc732f92e853b10b288ad5a1d",
          "profileCreatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileCreatedAtBlockNumber": 35512897,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-12T11:05:35Z",
          "profileLastUpdatedAtBlockNumber": 35512897,
          "profileTokenUri": "",
          "isDefault": false,
          "identity": ""
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

[^1]: 0x addresses, ENS domains, Lens, XMTP

[^2]: 0x address, ENS domain, Lens profile name and ID, Farcaster name and ID&#x20;