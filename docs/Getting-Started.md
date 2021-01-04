---
tags: [Getting Started]
---

# Getting Started with IntelliGems Experimentation

## Overview

The IntelliGems API enables running experiments. For our virtual-store specific api, check out the [Intelli-Store API docs](../Intelli-Store.md).

### Key Concepts
With IntelliGems, you run A/B tests, which we call `experiments`. You can configure experiments through our dashboard or programmatically through our API. 

Each experiment contains `variants`, which define the treatment effects or independent variables that we are testing.

When running an experiment, we will assign a `user` to a variant. When a request is made in your application, we will only return the information relevant for the specific user.  

### Experiments
#### Get all experiments for a user
`GET /experiments?userId=abc123` - This is the API your application or website will use to get the different experiments 

Example response:
```json
{
  "experiments": [
  {
    "id": "promotion-experiment",
    "variant": {
      "id": "discount10",
      "custom": {
        "discount": 10
      },
      "isControl": false,
      "percentage": 30
    },
    "name": "Promotion Experiment",
    "description": "Running a discount for new users."
  }
]
}
```

#### Get single experiment for a user
`GET /experiments/{experimentId}?userId=abc123` - use this endpoint when you want to get only a specific experiment for a user. 

Example response:
```json
{
  "experiment": {
    "id": "promotion-experiment",
    "variant": {
      "id": "discount10",
      "custom": {
        "discount": 10
      },
      "isControl": false,
      "percentage": 30
    },
    "name": "Promotion Experiment",
    "description": "Running a discount for new users."
  }
}
```


#### The Control

It's generally a good idea to define one control, which can be achieved with the `isControl` boolean of the Variant. This will help the algorithms understand what the baseline is and help with reporting and analysis. Only one variant per AB test can be the control.

