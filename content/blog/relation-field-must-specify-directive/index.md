---
title: The relation field must specify a `@relation` directive`
date: "2021-05-19"
description: "You got this GraphQL error because you have two fields with the same value"
---

## The error

```bash
The relation field [fieldname] must specify a `@relation` directive: `@relation(name: "MyRelation")`
```

## Why you got this error

You have two fields that have the same value, for example:

```graphql
user {
    id: ID
    name: String
    currentCompany: UserCompany
    previousCompany: UserCompany
}
```

This set of graphql query fields has two fields that have the value `UserCompany`. You have to either indicate a relation between user companies and users, or else create a subtype so they don't reside at the same level.
