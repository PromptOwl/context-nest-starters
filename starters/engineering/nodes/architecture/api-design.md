---
title: "API design conventions"
type: document
tags: ["#convention", "#architecture", "#api"]
status: published
version: 1
---

# API design conventions

Covers REST, gRPC, and GraphQL. Specifics differ; principles don't.

## Versioning

- **URL versioning** for REST: `/v1/users`. Major versions only.
- **Header versioning** is acceptable but more error-prone for consumers.
- **gRPC**: separate packages per major version (`api.users.v1`, `api.users.v2`).
- Internal APIs: version when you need to break compat for known consumers. Public APIs: every breaking change is a new major.

## Backwards compatibility

- **Adding fields is safe** if consumers are lenient. Make them lenient via the API guidelines.
- **Removing or renaming fields is breaking.** Either deprecate first (with a sunset date) or bump the major version.
- **Changing semantics is breaking** even if the shape didn't change. Wallpaper warnings won't save you.

## Errors

- **Always typed.** HTTP: a stable error code field + a human message. gRPC: use `codes.*` from the library. GraphQL: typed error extensions.
- **Stable error codes.** Once published, never change meaning. Add new codes; don't repurpose existing ones.
- **No stack traces in responses.** Leak internals; useless for callers.

## REST specifics

- Resources are nouns: `/users`, `/orders`. Not `/getUsers` or `/createOrder`.
- Actions on resources are HTTP verbs: GET, POST, PUT, PATCH, DELETE.
- PUT is full-replace idempotent. PATCH is partial update.
- POST that doesn't create a resource (e.g. `/orders/123/cancel`) is fine; not every action maps to CRUD.
- Pagination: cursor-based (`?cursor=...&limit=...`) over offset. Cursor-based survives writes during pagination.
- Filtering: small repos use query params; complex filters use POST with a body.

## gRPC specifics

- Strict semver on `.proto` packages.
- Required fields with `optional` keyword in proto3 — be explicit about what's required.
- Server streaming for one-to-many; bidi for stateful interactions.
- Deadlines on every call. The client sets them; the server respects them.

## GraphQL specifics

- One graph per service usually; federation if you've outgrown that.
- Connections / cursors per Relay spec.
- Persisted queries in production. Reject ad-hoc operations from untrusted clients to bound query complexity.
- Mutations return the updated entity so the client doesn't refetch.

## Auth and rate limits

- Authentication at the edge (gateway, sidecar). The service trusts a context carrying the identity.
- Rate limits per consumer, not per IP. IPs share NAT.
- 401 = "I don't know who you are." 403 = "I know who you are; you can't do this." Don't conflate them.

## Anti-patterns

- Returning HTTP 200 with `{"error": "..."}` in the body. Use status codes.
- Smuggling state through query params on a POST.
- `/api/users?ids=1,2,3` for batch — fragile. Use a real batch endpoint.
- "We'll version when we need to." You needed to three versions ago.

## See also

[[adr-template]] · [[service-design-checklist]]
