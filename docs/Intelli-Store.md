---
tags: [Store, Intelli-Store]
---

# Getting Started with IntelliGems Store API

## Overview

The IntelliGems Store API will enable you to run experiments in your virtual store. You can use these endpoints to configure your store and will also be used to render the items in your store.

### Mobile API Endpoints
#### Get the items in a collection
`GET /store/items?userId=abc123` - This is the API your mobile app will use to get the items, prices, and currencies for a user in your store. If any AB tests are running, it will return just the relevant information for the requesting user. 

Example response:
```json
{
  "items": [
    {
      "id": "60b2f0ee-cff7-4c97-8312-546df9d5cbe5",
      "extId": "bronze-sword",
      "name": "Bronze Sword",
      "price": 150,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    },
    {
      "id": "53d76c0c-7a77-4013-94f2-c86a20c59530",
      "extId": "gold-sword",
      "name": "Gold Sword",
      "price": 450,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    }
  ],
  "currencies": [
    {
      "id": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "extId": "coin",
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
```json
{
  "currencies": {
    "extId": "coin",
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
      "extId": "bronze-sword",
      "name": "Bronze Sword",
      "price": 150,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    },
    {
      "extId": "gold-sword",
      "name": "Gold Sword",
      "price": 450,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    }
  ]
}
```

Example response body:
```json
{
  "created": [
    {
      "id": "60b2f0ee-cff7-4c97-8312-546df9d5cbe5",
      "extId": "bronze-sword",
      "name": "Bronze Sword",
      "price": 150,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    },
    {
      "id": "53d76c0c-7a77-4013-94f2-c86a20c59530",
      "extId": "gold-sword",
      "name": "Gold Sword",
      "price": 450,
      "currencyId": "ad20def9-8593-4c1c-8b10-28caa0029c18",
      "isVisible": true
    }
  ],
  "updated": [],
  "failed": []
}
```

### AB Tests

AB Tests define a test where different groups of users will see different content. A group of users is defined as a *Variant* and each variant contains overrides. Each variant also contains a percentage, and the sum of the percentages in an AB test should be 100. 

Here is an example AB test configuration:
```json
{
  "name": "Sword Price Test",
  "isActive": true,
  "variants": [
    {
      "name": "Lower Prices",
      "percentage": 33,
      "isControl": false,
      "overrides": [
        {
          "itemId": "60b2f0ee-cff7-4c97-8312-546df9d5cbe5",
          "property": "price",
          "value": 125
        },
        {
          "itemId": "53d76c0c-7a77-4013-94f2-c86a20c59530",
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
          "itemId": "60b2f0ee-cff7-4c97-8312-546df9d5cbe5",
          "property": "price",
          "value": 175
        },
        {
          "itemId": "53d76c0c-7a77-4013-94f2-c86a20c59530",
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
          "itemId": "60b2f0ee-cff7-4c97-8312-546df9d5cbe5",
          "property": "price",
          "value": 150
        },
        {
          "itemId": "53d76c0c-7a77-4013-94f2-c86a20c59530",
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

