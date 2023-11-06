---
description: Learn how Airstack pricing will be implemented from November 22, 2023.
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

# 🔋 Pricing

## Before November 2023

[Airstack](https://airstack.xyz) is available **free of charge** until November 22, 2023.&#x20;

\
The exception is Enterprise use cases, defined as utilizing more than 10M API credits per month. If you are falling into this bucket, we'll contact you. \


If you need a rate limit increase, you can request one on your Dashboard's [Manage Billing](https://app.airstack.xyz/profile-settings/manage-plans) page.&#x20;

##

## From November 22, 2023

Airstack API use will be charged accordingly to the following guidelines:

### 1. Free to Start

* A very generous free bundle of credits (X millions) will be provided to all users&#x20;
* Many users will be able to make use of the free credits for many months before encountering any charges. The intent is to only charge for scaling production apps.&#x20;

### 2. Free to learn and test, always

* All Clients will get 2 API keys:
  * Test key: low rate limit, no credits charged for usage
  * Production key: 3000 requests per minute, bursts of 300 per second, credits charged for usage

### 3. No Pay-Walled Features

* All API features will be available to all developers at all times
* Unlimited use of Airstack AI for all users
* All devs will get a high rate limit:&#x20;

### 4. Pay Only For What You Use

* After you've exhausted your free credits, you will pay for to the value you are receiving, as measured in **API queries**.&#x20;
* Each API query will cost a # of **credits** depending on the number of nested API calls/variables in the query.&#x20;
  * A simple query with 1 input and 1 response variable = 1 credit. So, you would have to run that query millions of times to exhaust your free credits.&#x20;
  * The formula for calculating credits is as follows:
    * The # of inputs (e.g. wallet addresses/Identities, or contracts)
    * (multiplied by) the Top Level Query and number of Nested Queries within the query
      * This is constructed such that we are agnostic as to whether you query it as one big nested query or as individual queries.
      * It would still add up to the same amount of credits consumed.
* After the initial free credits, each additional credit will cost $0.00002, billed weekly based on your actual usage.&#x20;
* There are no flat or minimum monthly flat fees. You only pay for what you use. \


### 5. You're in full control

* Clients will have full visibility in their Airstack dashboard as to how many credits each query consumes and Airstack team will work with Clients to forecast and optimize their billing.
  * We want you to value what you pay for and make sure you are in full control; no surprises
* Let us know if you would like a report already on your credit utilization per query.
