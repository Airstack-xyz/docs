---
description: >-
  Learn how to deploy your subgraph using Airstack Managed Graph Node to the
  DEGEN L3 chain.
---

# 🚀 Quickstart

Airstack enables Degenchain developers to deploy subgraphs to Airstack's Managed Graph Node.

* This is currently offered for free during an initial learning period&#x20;
* An Airstack API key is required but usage of your Degenchain subgraph will not be rated towards Airstack API credits&#x20;
* Following the learning period there may be some charges to cover our costs of providing this service and for especially high volume usage.

## Prerequisites

* [Airstack API key](../get-started/get-api-key.md)
* [The Graph CLI](https://www.npmjs.com/package/@graphprotocol/graph-cli)
* Your subgraph code. If you don't have one, feel free to clone Airstack's [`user-contract-interactions`](https://github.com/Airstack-xyz/Subgraphs/tree/main/user-contact-interactions) subgraph repo for a starter.

{% tabs %}
{% tab title="Git (HTTP)" %}
```sh
git clone https://github.com/Airstack-xyz/Subgraphs.git && \
 cd Subgraphs/user-contract-interactions/
```
{% endtab %}

{% tab title="Git (SSH)" %}
```sh
git clone git@github.com:Airstack-xyz/Subgraphs.git && \
 cd Subgraphs/user-contract-interactions/
```
{% endtab %}

{% tab title="GitHub CLI" %}
```sh
gh repo clone Airstack-xyz/Subgraphs && \
  cd Subgraphs/user-contract-interactions/
```
{% endtab %}
{% endtabs %}

## Step 1: Create Subgraph

First, create the subgraph that you want to deploy in the Airstack Graph Node indexer by providing your [**Airstack API key**](../get-started/get-api-key.md) and the **name** for your subgraph:

```sh
graph create \
  --node https://subgraph.airstack.xyz/indexer/ \
  --access-token <AIRSTACK_API_KEY> <SUBGRAPH_NAME>
```

## Step 2: Deploy Subgraph

Once the subgraph is successfully created, deploy the subgraph by using the following command:

```sh
graph deploy \
  --node https://subgraph.airstack.xyz/indexer/ \
  --access-token <AIRSTACK_API_KEY> \
  --ipfs https://ipfs.airstack.xyz/ipfs/api/v0 \
  --headers '{"Authorization": "<AIRSTACK_API_KEY>"}' <SUBGRAPH_NAME>
```

Once you subgraph is deployed, you can check on the [Subgraph Dashboard](https://graph-monitor.airstack.xyz/public-dashboards/60d5d75c5d9f408f9081eec1b4070d22?orgId=1) to confirm if the deployment is successful:

{% embed url="https://graph-monitor.airstack.xyz/public-dashboards/60d5d75c5d9f408f9081eec1b4070d22?orgId=1" %}
Subgraph Dashboard for DEGEN L3 Chain
{% endembed %}

## Step 3: Query The Deployed Subgraph

And when it is successfully deployed you can try to query data from the GraphQL API from the deployed subgraph:

{% hint style="info" %}
If you are using Airstack subgraph repository, you can test the GraphQL API by setting the query field to: `--data '{"query":"{\n _meta {\n block {\n number\n }\n }\n}"}'`
{% endhint %}

```sh
curl --location 'https://subgraph.airstack.xyz/query/subgraphs/name/<SUBGRAPH_NAME>' \
  --header 'Accept-Language: en-GB,en-US;q=0.9,en;q=0.8' \
  --header 'Connection: keep-alive' \
  --header 'accept: application/json, multipart/mixed' \
  --header 'content-type: application/json' \
  --header 'Authorization: Bearer <AIRSTACK_API_KEY>' \
  --data '{"query":"STRINGIFIED_GRAPHQL_QUERIES"}'
```

Alternatively, you can also test your subgraph by making GraphQL queries from platforms such as [**Postman**](https://www.postman.com/) by using the following API information:

| Data     | Value                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------ |
| Endpoint | https://subgraph.airstack.xyz/query/subgraphs/name/\<SUBGRAPH\_NAME>                                         |
| Headers  | <ul><li>Key: <code>Authorization</code></li><li>Value: <code>Bearer &#x3C;AIRSTACK_API_KEY></code></li></ul> |

🎉 :partying\_face: Congratulations you've just deployed your 1st subgraph to index the DEGEN L3 chain!

## **D**eveloper Support

If you have any questions or need help regarding deploying your subgraph to the Airstack Graph Node, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Subgraphs Repository](https://github.com/Airstack-xyz/Subgraphs)
* [The Graph CLI Reference](https://www.npmjs.com/package/@graphprotocol/graph-cli)
