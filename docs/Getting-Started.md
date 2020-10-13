---
tags: [Getting Started]
---

# Getting Started with IntelliGems

## Overview

The IntelliGems API will enable you to run experiments in your virtual store. You can use these endpoints to configure your store and will also be used to render the items in your store.

### Mobile API Endpoints
#### Get the items in a collection
`GET /collection/{collectionId}/items?userId=abc123` - This is the API your mobile app will use to get the items, prices, and currencies for a user in your store. If any AB tests are running, it will return just the relevant information for the requesting user. 

Example response:
```json
{
  "items": [
    {
      "id": "bronze-sword",
      "name": "Bronze Sword",
      "price": 150,
      "currencyId": "coin",
      "visible": true
    },
    {
      "id": "gold-sword",
      "name": "Gold Sword",
      "price": 450,
      "currencyId": "coin",
      "visible": true
    }
  ],
  "currencies": [
    {
      "id": "coin",
      "name": "Coin",
      "icon": "https://yourdomain.com/static/img/currency.jpg"
    }
  ]
}
```

### Add Currencies and Items


#### Add Currencies

`POST /store/configure/currencies` - Add or change currencies to the store.

Example request body:
``` json
{
  "currencies": {
    "id": "coin",
    "name": "Coin",
    "icon": "https://yourdomain.com/static/img/currency.jpg"
  }
}
```

#### Add Items

`POST /store/configure/items` - Add or change items to the store

Example request body:
```json
{
  "items": [
    {
      "id": "bronze-sword",
      "name": "Bronze Sword",
      "price": 150,
      "currencyId": "coin",
      "visible": true
    },
    {
      "id": "gold-sword",
      "name": "Gold Sword",
      "price": 450,
      "currencyId": "coin",
      "visible": true
    }
  ]
}
```

### AB Tests

AB Tests define a test where different groups of users will see different content. A group of users is defined as a *Variant* and each variant contains overrides. Each variant also contains a percentage, and the sum of the percentages in an AB test should be 100. 

Here is an example AB test configuration:
```json
{
  "id": "ab-test-1",
  "name": "Sword Price Test",
  "active": true,
  "variants": [
    {
      "name": "Lower Prices",
      "percentage": 33,
      "isControl": false,
      "overrides": [
        {
          "itemId": "bronze-sword",
          "property": "price",
          "value": 125
        },
        {
          "itemId": "gold-sword",
          "property": "price",
          "value": 425
        }
      ]
    },
    {
      "name": "Higher Prices",
      "percentage": 33,
      "isControl": false,
      "overrides": [
        {
          "itemId": "bronze-sword",
          "property": "price",
          "value": 175
        },
        {
          "itemId": "gold-sword",
          "property": "price",
          "value": 475
        }
      ]
    },
    {
      "name": "Control",
      "percentage": 34,
      "isControl": true,
      "overrides": [
        {
          "itemId": "bronze-sword",
          "property": "price",
          "value": 150
        },
        {
          "itemId": "gold-sword",
          "property": "price",
          "value": 450
        }
      ]
    }
  ]
}
```

In this AB test, we have 3 variants: Lower Prices, Higher Prices, and Control. The prices for both items are lower and higher by 25 coins in the Lower and Higher prices variants respectively. 

#### The Control

It's generally a good idea to define one control, which can be achieved with the `isControl` boolean of the Variant. This will help the algorithms understand what the baseline is and help with reporting and analysis. Only one variant per AB test can be the control.

