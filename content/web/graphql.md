---
title: GraphQL
---

## Introspection
It's often useful to ask a GraphQL schema for information about what queries it supports. GraphQL allows us to do so using the introspection system!

https://graphql.org/learn/introspection/

## Introspection payloads

``` json
{"query":"query {__schema{types{name,fields{name}}}}"}
```