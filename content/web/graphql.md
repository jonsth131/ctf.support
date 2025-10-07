---
title: "GraphQL"
description: "Enumerate and exploit GraphQL endpoints through introspection and crafted query manipulation."
date: 2025-10-06
draft: false
categories: ["Web"]
tags: ["graphql", "introspection", "api", "ctf web"]
authors: ["CTF.Support Team"]
summary: "Identify GraphQL schemas and hidden operations using introspection and automated query enumeration."
aliases: ["/web/graphql/"]
slug: "graphql"
toc: true
---

## Introduction

GraphQL APIs expose structured query endpoints that return data through flexible requests.
If introspection is enabled, full schema and field enumeration becomes possible, a common web CTF vector.

## Quick Reference

| Task                   | Example Payload                                    |
|------------------------|----------------------------------------------------|
| Enumerate schema       | `{"query":"{__schema{types{name,fields{name}}}}"}` |
| Query objects directly | `{"query":"{user(id:1){name,email}}"}`             |

## Tools

| Tool                                                                | Purpose                                |
|---------------------------------------------------------------------|----------------------------------------|
| [graphql-playground](https://github.com/graphql/graphql-playground) | GUI for exploring endpoints            |
| [InQL](https://github.com/doyensec/inql)                            | Burp extension for GraphQL enumeration |

## Tips

- Try POSTing `{"query":"{__schema{types{name}}}"}`, success means introspection is ON.
- IDs and filters often leak internal logic (e.g., `isAdmin`, `flag`).
