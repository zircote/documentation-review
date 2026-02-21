---
diataxis_type: explanation
diataxis_topic: Why REST was chosen over GraphQL for the TaskManager API
---

# Understanding the TaskManager API Design

## Background

When we designed the TaskManager API, we faced a fundamental choice between REST and GraphQL. This document explains the reasoning behind our decision and the trade-offs involved.

## The problem we were solving

TaskManager serves a straightforward domain: creating, reading, updating, and deleting tasks. The data model is shallow — tasks have a fixed set of fields with no deeply nested relationships. Clients typically need all fields when they fetch a task.

This is significant because the strengths of GraphQL — flexible field selection, efficient traversal of deep object graphs, reducing over-fetching — provide the most value when the data model is complex and client needs vary widely.

## Why REST was a better fit

REST maps naturally to CRUD operations on resources. Each task is a resource with a URL, and the HTTP methods (`GET`, `POST`, `PATCH`, `DELETE`) correspond directly to the operations users need. This makes the API intuitive for anyone familiar with HTTP conventions.

The simplicity of REST also reduces the infrastructure needed. There is no schema definition layer, no query parser, no resolver chain. The server is a thin layer between HTTP and the data store. For a small, focused API like TaskManager, this simplicity is a genuine advantage — less code means fewer bugs, faster development, and easier maintenance.

## What we gave up

REST is not without trade-offs. As the TaskManager evolves, we may encounter limitations:

- **Multiple resource fetches.** If we add subtasks or comments, a client listing tasks with their subtasks would need multiple requests (or a custom endpoint). GraphQL handles this with a single query.
- **Over-fetching.** Mobile clients that need only task titles still receive the full task object. GraphQL lets clients request exactly the fields they need.
- **Versioning friction.** Adding fields is easy, but removing them requires careful deprecation. GraphQL's schema-first approach makes evolution more explicit.

Some teams prefer GraphQL precisely because it prevents these problems from emerging. That is a valid perspective, but for our current scale and use case, the cost of adopting GraphQL outweighs the benefit.

## The role of conventions

One factor in our decision was team familiarity. Every developer on the team had built REST APIs before. None had production GraphQL experience. Choosing a technology the team knows well reduces onboarding time and avoids a class of mistakes that come from inexperience with a new paradigm.

This is not a universal argument against GraphQL — a team experienced with GraphQL should absolutely consider using it. Context matters.

## Further reading

- [Tutorial: Build your first task](../tutorials/first-task.md) — Hands-on introduction to the API
- [TaskManager API reference](../reference/api.md) — Complete endpoint documentation
- [How to paginate results](../guides/pagination.md) — Practical guide for working with lists
