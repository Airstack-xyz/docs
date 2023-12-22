---
description: >-
  Learn all the available logical comparison operators for filtering out the
  desired API results.
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

# 📏 Working with GraphQL

In GraphQL, logical comparators like \_and , \_nor, and \_or are used to combine multiple filtering conditions, allowing you to create more complex queries. These logical comparators can only be used between unique filter inputs only. In order to add comparators to one specific filter check the information below. Here's a brief explanation of each:

#### Using GraphQL logical comparators

* <mark style="color:red;">**`_and`**</mark>: AND - Combines multiple filtering conditions, and only returns the data that meets all the specified conditions. Note: the relationship between multiple selected filters is always `AND` by default.
* <mark style="color:red;">**`_nor`**</mark>: NOR (Not OR) - Combines multiple filtering conditions, and only returns the data that does not meet any of the specified conditions.
* <mark style="color:red;">**`_or`**</mark>: OR - Combines multiple filtering conditions, and returns the data that meets at least one of the specified conditions.

In addition to the filtering conditions, Airstack GraphQL also has the following comparators which are used to filter data in queries based on specified conditions. They are shorthand for various comparison operations:

* <mark style="color:red;">**`_eq`**</mark>: Equals - Filters the data where the specified field is equal to the provided value.
* <mark style="color:red;">**`_gt`**</mark>: Greater Than - Filters the data where the specified field is greater than the provided
* <mark style="color:red;">**`_gte`**</mark>: Greater Than or Equal - Filters the data where the specified field is greater than or equal to the provided value.
* <mark style="color:red;">**`_lt`**</mark>: Less Than - Filters the data where the specified field is less than the provided value.
* <mark style="color:red;">**`_lte`**</mark>: Less Than or Equal - Filters the data where the specified field is less than or equal to the provided value.
* <mark style="color:red;">**`_ne`**</mark>: Not Equal - Filters the data where the specified field is not equal to the provided value.
* <mark style="color:red;">**`_in`**</mark>: In - Filters the data where the specified field's value is within the provided array of values.
* <mark style="color:red;">**`_nin`**</mark>: Not In - Filters the data where the specified field's value is not within the provided array of values.
* <mark style="color:red;">**`_regex`**</mark>: Regex Search - Filter the data where it fulfills the given regex script condition. Any standard regex pattern will work with this operator. Currently only available in [`PoapEvents`](../api-reference/poapevents-api.md) API.

{% hint style="info" %}
For **`_in`** and **`_nin`** operators, they have a **maximum limit of 200 addresses** as responses are only limited to 200.\
\
If you want to add more than 200 to the filters, you can get the rest of the results by using the cursor pagination.
{% endhint %}
