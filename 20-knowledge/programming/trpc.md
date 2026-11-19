---
type: concept
area: programming
date: 2026-11-08
tags: [typescript, backend, api]
status: draft
---
# tRPC

## What is it? (one or two sentences)
A TypeScript library for building APIs where the client automatically knows the exact types of every server function, with no schema files and no code generation. You call server functions from the client almost like normal functions.

## Why does it exist? What problem does it solve?
With REST (Express), the server and client are separate worlds. If you change a response field on the server, the client doesn't know until it breaks at runtime. You end up writing the types twice or maintaining an OpenAPI/GraphQL schema. tRPC removes that gap: the client imports the *type* of the server router, so TypeScript shows an error immediately if they disagree.

## How it works
- **Router**: a group of API functions. Like a folder of routes.
- **Procedure**: one API function. Two main kinds: `query` (read, like GET) and `mutation` (write, like POST/PUT/DELETE).
- **Input validation**: usually with Zod. The same schema validates at runtime and gives the TypeScript type.
- **AppRouter type**: the server exports `type AppRouter = typeof appRouter`. The client imports only this type (no server code ships to the browser).
- **Adapters**: tRPC is not a web server. It plugs into one: Express, Fastify, Next.js, or its own standalone adapter. Under the hood it is still plain HTTP requests.
- **Context**: per-request object (user, db) available in every procedure. Similar idea to Express middleware setting `req.user`.
- **Middleware**: tRPC has its own, for things like auth checks on procedures.

## Express vs tRPC
| | Express | tRPC |
|---|---|---|
| What it is | Web server framework | API layer that runs *on top of* a server (can be inside Express) |
| Unit | Route (`app.get("/users/:id")`) | Procedure (`user.getById`) |
| Client calls | `fetch("/users/1")`, you type the response yourself | `trpc.user.getById.query({ id: 1 })`, fully typed |
| Types shared with client | No (manual) | Yes, automatic |
| Validation | Manual or library | Built in via input schema (Zod) |
| Language | JS or TS | TypeScript, on both sides |
| Public API for others | Great (REST is universal) | Poor fit, clients must be TypeScript |

Short version: Express handles HTTP. tRPC is a way to define the API you serve over HTTP, with type safety. They are not competitors; you can mount tRPC inside Express.

## Small example
```ts
// server
import { initTRPC } from "@trpc/server";
import { z } from "zod";

const t = initTRPC.create();

export const appRouter = t.router({
  greet: t.procedure
    .input(z.object({ name: z.string() }))
    .query(({ input }) => `Hello ${input.name}`),
});

export type AppRouter = typeof appRouter;
```
```ts
// client (only imports the TYPE)
import type { AppRouter } from "../server";
// client setup omitted
const msg = await trpc.greet.query({ name: "Pamod" }); // msg is string
// trpc.greet.query({ name: 5 })  -> TypeScript error
```
> Setup code differs between tRPC versions. Check the video / official docs for the exact client setup.

## Common mistakes / gotchas
- Thinking tRPC replaces Express. It doesn't; it needs a server adapter.
- Importing server code (not just the type) into the client.
- Works best when client and server share one TypeScript codebase (monorepo). Hard if the client isn't TypeScript.
- Not a good choice for a public API that third parties consume.

## Source
- Video: (add title / link)

## Related
- [[ ]] Express notes
- [[ ]] Zod
- [[ ]] REST API basics
