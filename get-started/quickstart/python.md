---
description: >-
  Learn how to integrate Airstack APIs to your Python-based application using
  the Airstack Python SDK.
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

# 🐍 Python

## 🐍 Python

In this tutorial, you will learn how to start integrating [Airstack](https://airstack.xyz) API into your Python application.

## Table Of Contents

* [Step 0: Pre-requisites](python.md#step-0-pre-requisites)
* [Step 1: Install Airstack Python SDK](python.md#step-1-install-airstack-python-sdk)
* [Step 2: Set Environment Variable](python.md#step-2-set-environment-variable)
* [Step 3: Create API Client](python.md#step-3-create-api-client)
* [Step 4: Call Your Query](python.md#step-4-call-your-query)

### Step 0: Pre-requisites

* Completed [Get API Key](../get-api-key.md)
* Git
* Node v.16+

### Step 1: Install Airstack Python SDK

Use a package manager to install the [Airstack Python SDK](https://pypi.org/project/airstack/) into your Python project:

```sh
pip install airstack
```

### Step 2: Set Environment Variable

Create a new `.env` file:

```sh
touch .env
```

Add the [Airstack API key](../get-api-key.md) as the environment variable:

```sh
AIRSTACK_API_KEY=YOUR_AIRSTACK_API_KEY
```

Install [`python-dotenv`](https://pypi.org/project/python-dotenv/) package to your project:

```bash
pip install python-dotenv
```

and import it into your Python project to inject the environment variables:

{% code title="index.py" %}
```python
from dotenv import load_dotenv

load_dotenv()
```
{% endcode %}

### Step 3: Create API Client

Wrap your application or React component with `AirstackClient` from the SDK to initialize it with the [Airstack API key](../get-api-key.md):

{% code title="index.py" %}
```python
import os
from airstack.execute_query import AirstackClient

api_key = os.environ.get("AIRSTACK_API_KEY")
if api_key is None:
  raise Exception("Please set the AIRSTACK_API_KEY environment variable")

api_client = AirstackClient(api_key=api_key)
```
{% endcode %}

### Step 4: Call Your Query

Once you have initialized the SDK, you can use the [`execute_query`](https://github.com/Airstack-xyz/airstack-python-sdk#execute\_query) to call the Airstack API.

Below you have been provided with Airstack query to fetch the 0x address, Lens, and Farcaster owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

{% hint style="info" %}
For more query examples, check out [**Guides**](broken-reference) for various use cases you can build with Airstack.
{% endhint %}

{% code title="index.py" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

query = """
query MyQuery {
  Wallet(input: {identity: "vitalik.eth", blockchain: ethereum}) {
    socials {
      dappName
      profileName
    }
    addresses
  }
}
"""



async def main():
  execute_query_client = api_client.create_execute_query_object(query=query)
  query_response = await execute_query_client.execute_query()
  data = query_response.data
  error = query_response.error
  if query_response.error is not None:
    raise Exception(error.message)

  print(data)

asyncio.run(main())
```
{% endcode %}

The `data` variable will return and logged into your terminal as follows:

```json
{
  "data": {
    "Wallet": {
      "socials": [
        {
          "dappName": "farcaster",
          "profileName": "vitalik.eth"
        }
      ],
      "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
    }
  }
}
```

### Developer Support

If you have any questions or need help regarding integrating [Airstack](https://airstack.xyz) into your Python application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

Learn to build more with Airstack using our tutorials:

* [Resolve Identities](../../guides/resolve-identities/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Python SDK Reference](https://pypi.org/project/airstack/)
