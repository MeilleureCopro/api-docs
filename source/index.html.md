---
title: MeilleureCopro API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl

toc_footers:
  - <a href='https://www.meilleurecopro.com'>MeilleureCopro</a>

includes:
  - errors

search: true
---

# MeilleureCopro API Reference

This website documents the public API for [MeilleureCopro](https://www.meilleurecopro.com).

You can view code examples in the dark area to the right. If anything is missing or seems incorrect, please check the [GitHub issues](https://github.com/MeilleureCopro/api-docs/issues) for existing known issues or [create a new issue](https://github.com/MeilleureCopro/api-docs/issues/new).

MeilleureCopro API is organized around REST. We use HTTP response codes to indicate API error. JSON is returned by all API responses, including errors.

> API endpoint :
```
https://api.meilleurecopro.com
```


# Authentication

Authentication to the API is performed via JWT (Json Web Token) put in Authorization header.

> To authorize, use this code:

```curl
curl "https://api.meilleurecopro.com/v1/echo"
  -H "Authorization: Bearer JWT"
```

> Make sure to replace `JWT` with your API token.

We expects the JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer JWT`

# Building expenses estimation

Our API estimate building expenses and provide an "expenses label" for a given building or given flat. 
The following image summarize the main results : there are 7 ratings from A to G and the expenses here are rate E.
 
![alt text](/images/label-example-small.png "Label example")

## Estimate flat expenses



```curl
curl -X POST https://api.meilleurecopro.com/v1/building-expenses/estimate
  -H "Authorization: Bearer JWT"
  -d '{"address":"5 Parvis Alan Turing 75013 Paris", "surface": 50, "expenses": 2000,  \
  "elevator": true, "caretaker": false, "construction_year": 1900, "heating_type": "COLLECTIVE", \
  "water_heating_type": "INDIVIDUAL", "zip_code": "75013"}'
```

> The above command returns JSON structured like this:

```json
{
  "accuracy": 4,
  "expected_expenses": 1000,
  "label": {
    "rating": "G",
    "current_expenses": 2000,
    "a": {"min": 0, "max": 400},
    "b": {"min": 401, "max": 600},
    "c": {"min": 601, "max": 800},
    "d": {"min": 801, "max": 1000},
    "e": {"min": 1001, "max": 1200},
    "f": {"min": 1201, "max": 1500},
    "g": {"min": 1501}
  },
  "label_image_data": "..."
}
```

This endpoint estimate building expenses for a given flat and returns what expenses the owner should pay and the rating of its expenses.


### HTTP Request

`POST https://api.meilleurecopro.com/v1/building-expenses/estimate`

### Body parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
expenses | number | true | Current building expenses.
surface | number |true | Flat surface.
zip_code | string | true | Insee code of the flat.
insee_code | string | true if zip_code not present | Insee code of the flat.
description | string | false | Text description of the flat.
elevator | boolean |required if no description | Elevator in the building.
lot_count | number |required if no description| Number of flat in the building.
construction_year | number | required if no description | Construction year (approximative) of the building.
caretaker| boolean |required if no description | Caretaker in the building.
heating_type | string | required if no description | COLLECTIVE or INDIVIDUAL.
water_heating_type | required if no description |false | COLLECTIVE or INDIVIDUAL.
water_meter | boolean| false | True if water meters are present in the building.
heating_meter|boolean| false | True if heating meter are present on flat radiators.
syndic_type | string |false | PRO or VOLUNTEER.
green_space  | string | false | Size of green space: NONE, SMALL, NORMAL, BIG.
swimming_pool| boolean|false | Swimming pool.
parking_count | number |false | Flat parking count.
underground_parking | boolean |false | True if parking is underground.
floor_count | number |false | Building floor count.
floor | number |false | Flat floor.
room_count | number | false | Flat room count.
resident_count|number|false | Number of resident in the flat.
address | string | false | Address of the flat.
latitude | number | false | Latitude of the flat.
longitude | number | false | Longitude of the flat.


### Simulation response


> Example of a simulation response

```json
{
  "accuracy": 4,
  "expected_expenses": 1000,
  "label": {
    "rating": "G"
    "current_expenses": 2000,
    "a": {"min": 0, "max": 400},
    "b": {"min": 401, "max": 600},
    "c": {"min": 601, "max": 800},
    "d": {"min": 801, "max": 1000},
    "e": {"min": 1001, "max": 1200},
    "f": {"min": 1201, "max": 1500},
    "g": {"min": 1501},
  },
  "label_image_data": ""
}
```

Attributes | Description
--------- | -----------
expected_expenses |  The building expenses the simulator expects.
accuracy | Accuracy of the simulator from 1 (not accurate) to 5 (very accurate).
label | Label object with expenses ratings from A to G described below.
label_image_data | label image base64 encoded of size 285x250 such as below

![alt text](/images/label-example.png "Label example")

### Expenses label object

> Example of a label object

```json
{
  "rating": "G"
  "current_expenses": 2000,
  "a": {"min": 0, "max": 400},
  "b": {"min": 401, "max": 600},
  "c": {"min": 601, "max": 800},
  "d": {"min": 801, "max": 1000},
  "e": {"min": 1001, "max": 1200},
  "f": {"min": 1201, "max": 1500},
  "g": {"min": 1501},
}
```

Attributes | Description
--------- | -----------
rating | Rating of the current expenses (A -> G)
current_expenses | The current expenses
a |  Expenses range for rating A
b |  Expenses range for rating B
c |  Expenses range for rating C
d |  Expenses range for rating D
e |  Expenses range for rating E
f |  Expenses range for rating F
g |  Expenses range for rating G

