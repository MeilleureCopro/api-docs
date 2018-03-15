---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the MeilleureCopro API! You can use our API to access MeilleureCopro API endpoints to simulate building expenses.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.meilleurecopro.com/echo"
  -H "Authorization: JWT"
```

> Make sure to replace `JWT` with your API token.

MeilleureCopro uses JWT tokens to allow access to the API. You can ask for tokens by email dev at meilleurecopro.com.

We expects for the JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: JWT`

<aside class="notice">
You must replace <code>JWT</code> with your personal token API.
</aside>

# Building expenses

## Simulate building expenses

```shell
curl -XPOST "https://api.meilleurecopro.com/v1/building-expenses/simulate"
  -H "Authorization: JWT"
```

> The above command returns JSON structured like this:

```json
{
  "expected_expenses": 1000
}
```

This endpoint retrieves all kittens.

### HTTP Request

`POST https://api.meilleurecopro.com/v1/building-expenses/simulate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
address |  | Address of the building.
expenses |  | Current building expenses.

<aside class="success">
Remember ...!
</aside>

