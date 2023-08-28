---
description: >-
  Learn how to fetch Farcaster user details with 0x address, ENS domain,
  Farcaster name and ID, and Lens profile name and ID
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

## Get Farcaster Profile Details By Farcaster ID

You can get the Farcaster user profile details by their Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/wJN8EPRanE" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
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

{% embed url="https://app.airstack.xyz/query/k4bu90KgzA" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
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

{% embed url="https://app.airstack.xyz/query/sxRKmMtrFz" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
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

{% embed url="https://app.airstack.xyz/query/7GfIkzc5gx" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
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

{% embed url="https://app.airstack.xyz/query/KbcMBVVFsf" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
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

{% embed url="https://app.airstack.xyz/query/2Lzos9c5Rr" %}
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
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
          "isDefault": false,
          "identity": "fc_fname:dwr.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Bulk Query Farcaster Profile Details

You can bulk query the profile details of multiple Farcaster users by their various [web3 identities](#user-content-fn-2)[^2]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/8A9aPBliCG" %}
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
          "id": "1ee2f59c97582671b93635aebfc645d40510cbadd6339e532fcaec4295e6e037",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "5650",
          "userAddress": "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
          "userCreatedAtBlockTimestamp": "2022-11-18T20:44:00Z",
          "userCreatedAtBlockNumber": 7977439,
          "userLastUpdatedAtBlockTimestamp": "2022-11-18T20:44:00Z",
          "userLastUpdatedAtBlockNumber": 7977439,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "profileName": "vitalik.eth",
          "profileTokenId": "53546877787533711135172528420529478392632952428887988304966222706809455509504",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-11-18T20:44:00Z",
          "profileCreatedAtBlockNumber": 7977439,
          "profileLastUpdatedAtBlockTimestamp": "2022-11-18T20:44:00Z",
          "profileLastUpdatedAtBlockNumber": 7977439,
          "profileTokenUri": "http://www.farcaster.xyz/u/vbuterin.json",
          "isDefault": false,
          "identity": ""
        },
        {
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
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

If you want want to get Farcaster user with fid in certain range, e.g. fid 1 to 3, then you can use the fid as an array to the `userId` input to fetch the data as shown below:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/pJYR1DlOeE" %}
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
          "id": "2616763182d1539bc20585a137a5f0c4005fd0c2851b829e5a9a1eb7c1ed04a1",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "1",
          "userAddress": "0x8773442740c17c9d0f0b87022c722f9a136206ed",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x8773442740c17c9d0f0b87022c722f9a136206ed",
            "0x86924c37a93734e8611eb081238928a9d18a63c0"
          ],
          "profileName": "farcaster",
          "profileTokenId": "46308084199157716655702464398050049919145165628022664991789129153135907962880",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/farcaster.json",
          "isDefault": false,
          "identity": ""
        },
        {
          "id": "b9b952e45ab0156b3035de25915e72cc5a1c315e21108d72c6c55151e8bccac5",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "2",
          "userAddress": "0x4114e33eb831858649ea3702e1c9a2db3f626446",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x4114e33eb831858649ea3702e1c9a2db3f626446",
            "0x182327170fc284caaa5b1bc3e3878233f529d741",
            "0x91031dcfdea024b4d51e775486111d2b2a715871"
          ],
          "profileName": "varunsrin.eth",
          "profileTokenId": "53372916132825433828052250902442082526116633556818697486937480128647458193408",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/v.json",
          "isDefault": false,
          "identity": ""
        },
        {
          "id": "c9067a911d67d849200404f1f7e1cc0b9eff4716e085d244db877dad59982b95",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "3",
          "userAddress": "0x74232bf61e994655592747e20bdf6fa9b9476f79",
          "userCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userCreatedAtBlockNumber": 7648814,
          "userLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "userLastUpdatedAtBlockNumber": 7648814,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0x74232bf61e994655592747e20bdf6fa9b9476f79",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ],
          "profileName": "dwr.eth",
          "profileTokenId": "45442326458118800696824595983775420889468637770166871407996878418806401138688",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileCreatedAtBlockNumber": 7648814,
          "profileLastUpdatedAtBlockTimestamp": "2022-09-24T03:08:48Z",
          "profileLastUpdatedAtBlockNumber": 7648814,
          "profileTokenUri": "http://www.farcaster.xyz/u/dwr.json",
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

{% hint style="info" %}
For larger range, it is recommended that you generate the array programmatically instead of manually typing them.\
\
It is best practice that you keep the size of the array to a **maximum length of 200** per API call.
{% endhint %}

## Bulk Query All Farcaster Users Profile Details

You can bulk query the profile details of all Farcaster users:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/YY02CjXaPG" %}
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
          "id": "00032cc00ddf7b9ccacb73fa6055a23024650e7ce91b9ffaa7919da577a4d2c8",
          "chainId": "5",
          "blockchain": "ethereum_goerli",
          "dappName": "farcaster",
          "dappSlug": "farcaster_goerli",
          "dappVersion": "goerli",
          "userId": "4226",
          "userAddress": "0xfb0d4b9bc273a4a1c50d014815497bae44cf3402",
          "userCreatedAtBlockTimestamp": "2022-10-25T01:37:24Z",
          "userCreatedAtBlockNumber": 7829021,
          "userLastUpdatedAtBlockTimestamp": "2022-10-25T01:37:24Z",
          "userLastUpdatedAtBlockNumber": 7829021,
          "userHomeURL": "https://www.farcaster.xyz/",
          "userRecoveryAddress": "0x0000000000000000000000000000000000000000",
          "userAssociatedAddresses": [
            "0xfb0d4b9bc273a4a1c50d014815497bae44cf3402"
          ],
          "profileName": "ratan",
          "profileTokenId": "51735852133051278057081227170404867683004963210937239958095082937883562082304",
          "profileTokenAddress": "0xe3be01d99baa8db9905b33a3ca391238234b79d1",
          "profileCreatedAtBlockTimestamp": "2022-10-25T01:37:24Z",
          "profileCreatedAtBlockNumber": 7829021,
          "profileLastUpdatedAtBlockTimestamp": "2022-10-25T01:37:24Z",
          "profileLastUpdatedAtBlockNumber": 7829021,
          "profileTokenUri": "http://www.farcaster.xyz/u/ratan.json",
          "isDefault": false,
          "identity": ""
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

[^1]: 0x addresses, ENS domains, Lens, XMTP

[^2]: 0x address, ENS domain, Lens profile name and ID, Farcaster name and ID&#x20;
