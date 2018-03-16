---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl

toc_footers:
  - <a href='https://www.meilleurecopro.com'>MeilleureCopro</a>

includes:
  - errors

search: true
---

# API Reference

MeilleureCopro API is organized around REST. We use HTTP response codes to indicate API error. JSON is returned by all API responses, including errors.

> API endpoint :
```
https://api.meilleurecopro.com
```


# Authentication

Authenticate your account when using the API by including your secret API key in the request. Authentication to the API is performed via a Json Web Token put in Authorization header.

> To authorize, use this code:

```curl
curl "https://api.meilleurecopro.com/v1/echo"
  -H "Authorization: Bearer JWT"
```

> Make sure to replace `JWT` with your API token.

We expects for the JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer JWT`

# Building expenses

## Flat expenses simulation object


> Example simulation response

```json
{
  "accuracy": 4,
  "current_expenses": 2000,
  "expected_expenses": 1000,
  "potential_saving": 1000,
  "label": {
    "A": {"min": 0, "max": 400},
    "B": {"min": 401, "max": 600},
    "C": {"min": 601, "max": 800},
    "D": {"min": 801, "max": 1000},
    "E": {"min": 1001, "max": 1200},
    "F": {"min": 1201, "max": 1500},
    "G": {"min": 1501}
  }
}
```

Attributes | Description
--------- | -----------
current_expenses |  Current building expenses of the flat.
expected_expenses |  The building expenses the simulator expects.
potential_saving | The potential saving on the flat expenses.
accuracy | Accuracy of the simulator from 1 (not accurate) to 5 (very accurate).
label | Label object with expenses grades from A to G.


## Expenses label

> Example of a label

```json
{
  "A": {"min": 0, "max": 400},
  "B": {"min": 401, "max": 600},
  "C": {"min": 601, "max": 800},
  "D": {"min": 801, "max": 1000},
  "E": {"min": 1001, "max": 1200},
  "F": {"min": 1201, "max": 1500},
  "G": {"min": 1501}
}
```

Attributes | Description
--------- | -----------
A |  Expenses range for grade A
B |  Expenses range for grade B
C |  Expenses range for grade C
D |  Expenses range for grade D
E |  Expenses range for grade E
F |  Expenses range for grade F
G |  Expenses range for grade G

## Simulate expenses

```curl
curl -X POST https://api.meilleurecopro.com/v1/building-expenses/simulate
  -H "Authorization: Bearer JWT"
  -d '{"address":"5 Parvis Alan Turing 75013 Paris", "surface": 50, "expenses": 2000,  \
  "elevator": true, "caretaker": false, "heating_type": "COLLECTIVE", \
  "water_heating_type": "INDIVIDUAL"}'
```

> The above command returns JSON structured like this:

```json
{
  "accuracy": 4,
  "current_expenses": 2000,
  "expected_expenses": 1000,
  "potential_saving": 1000,
  "label": {
    "A": {"min": 0, "max": 400},
    "B": {"min": 401, "max": 600},
    "C": {"min": 601, "max": 800},
    "D": {"min": 801, "max": 1000},
    "E": {"min": 1001, "max": 1200},
    "F": {"min": 1201, "max": 1500},
    "G": {"min": 1501}
  }
}
```

This endpoint simulate expenses for a given flat and returns what expenses the owner should pay and the grade of its expenses.


### HTTP Request

`POST https://api.meilleurecopro.com/v1/building-expenses/simulate`

### Body parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
address | string | true if insee_code is not present | Address of the flat.
insee_code | string | true if address is not present | Insee code of the flat.
expenses | number | true | Current building expenses.
surface | number |true | Flat surface.
elevator | boolean |true | Elevator in the building.
caretaker| boolean |true | Caretaker in the building.
heating_type | string | true | COLLECTIVE or INDIVIDUAL.
room_count | number | false | Flat room count.
water_heating_type | string |false | COLLECTIVE or INDIVIDUAL.
construction_year | number |false | Construction year (approximative) of the building.
lot_count | number |false | Number of flat in the building.
floor | number |false | Flat floor.
floor_count | number |false | Building floor count.
parking | boolean |false | Flat with parking.
parking_count | number |false | Flat parking count.
green_spaces  | number | false | Size of garden / green spaces from 1 to 5.
syndic_type | string |false | PRO or VOLUNTEER.



