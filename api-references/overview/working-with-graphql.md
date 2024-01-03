---
description: >-
  Learn all the available logical and comparison operators for filtering out the
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

## Logical Operators

In GraphQL, logical operators like \_and , \_nor, and \_or are used to combine multiple filtering conditions, allowing you to create more complex queries. These logical operators can only be used between unique filter inputs only. In order to add operators to one specific filter check the information below. Here's a brief explanation of each:

* <mark style="color:red;">**`_and`**</mark>: AND - Combines multiple filtering conditions, and only returns the data that meets all the specified conditions. Note: the relationship between multiple selected filters is always `AND` by default.
* <mark style="color:red;">**`_nor`**</mark>: NOR (Not OR) - Combines multiple filtering conditions, and only returns the data that does not meet any of the specified conditions.
* <mark style="color:red;">**`_or`**</mark>: OR - Combines multiple filtering conditions, and returns the data that meets at least one of the specified conditions.

## Comparison Operators

In addition to the filtering conditions, Airstack GraphQL also has the following comparison operators which are used to filter data in queries based on specified conditions. They are shorthand for various comparison operators:

<table><thead><tr><th width="190">Operators</th><th width="160">Represents</th><th>Description</th></tr></thead><tbody><tr><td><mark style="color:red;"><strong><code>_eq</code></strong></mark></td><td>Equals</td><td>Fetch data that has <strong>equal</strong> value to the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_gt</code></strong></mark></td><td>Greater Than</td><td>Fetch data that has value <strong>greater than</strong> the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_gte</code></strong></mark></td><td>Greater Than or Equal</td><td>Fetch data that has value <strong>greater than or equal to</strong> the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_lt</code></strong></mark></td><td>Less Than</td><td>Fetch data that has value <strong>less than</strong> the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_lte</code></strong></mark></td><td>Less Than Equal</td><td>Fetch data that has value <strong>less than</strong> the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_ne</code></strong></mark></td><td>Not Equal</td><td>Fetch data that does not have <strong>equal</strong> value to the provided input on the specified filter input.</td></tr><tr><td><mark style="color:red;"><strong><code>_in</code></strong></mark></td><td>In</td><td>Fetch data that has value matched to one of the element <strong>in</strong> the provided array input on the specified filter input.<br><br>Currently, <mark style="color:red;"><strong><code>_in</code></strong></mark> operator has a <strong>maximum limit of 200 addresses</strong> as responses are only limited to 200.</td></tr><tr><td><mark style="color:red;"><strong><code>_nin</code></strong></mark></td><td>Not In</td><td>Fetch data that <strong>does not have</strong> value matched to one of the element <strong>in</strong> the provided array input on the specified filter input.<br><br>Currently, <mark style="color:red;"><strong><code>_in</code></strong></mark> operator has a <strong>maximum limit of 200 addresses</strong> as responses are only limited to 200.</td></tr><tr><td><mark style="color:red;"><strong><code>_regex</code></strong></mark></td><td>Regex Search </td><td>Fetch data that has value matched to provided regex pattern input on the specified filter input.<br><br>Currently only available in <a href="../api-reference/poapevents-api.md"><code>PoapEvents</code></a> and <a href="../api-reference/socials-api.md"><code>Socials</code></a> API.</td></tr><tr><td><mark style="color:red;"><strong><code>_regex_in</code></strong></mark></td><td>Multiple Regex Search</td><td>Fetch data that has value matched to one of the provided regex pattern in the array input on the specified filter input.<br><br>Currently only available in <a href="../api-reference/poapevents-api.md"><code>PoapEvents</code></a> and <a href="../api-reference/socials-api.md"><code>Socials</code></a> API.<br><br>Currently, <mark style="color:red;"><strong><code>_in</code></strong></mark> operator has a <strong>maximum limit of 200 addresses</strong> as responses are only limited to 200.</td></tr></tbody></table>
