---
name: backend-review
description: Review backend changes that affect API contracts, business rules, access boundaries, persisted data, jobs, AI tasks, integrations, or runtime behavior. Apply to a requested backend review or a meaningful change in one of those areas.
---

# Backend review

## Decide when to apply

Use this review for a changed endpoint response, permission check, query, migration, cache scope, job delivery rule, model task, provider call, or startup setting that can affect service behavior. A frontend edit can qualify if it changes the contract the backend must honor. Copy edits, formatting, and local renames with unchanged behavior can use ordinary review. Scope the review to the changed path and its affected consumers.

## Trace an actual operation

Start at a reachable route, event handler, scheduled job, or command. Read the registration and middleware as well as the handler. Follow the operation through business decisions, dependency calls, durable effects, and the response or acknowledgement. Identify who else invokes it. Compare the change with existing callers, schemas, tests, and deployed data formats. Use the repository's conventions to judge placement.

| If the change affects… | Follow this question through the code |
| --- | --- |
| Request handling or permissions | Where does trusted identity come from? Where is access to this specific record enforced, including for alternate callers? Inspect returned fields and logged data. |
| Ownership and coupling | Could another caller use the operation without recreating an HTTP request? Does a transport handler implement a rule already owned by a feature? Recommend moving it only when that fixes a concrete problem. |
| Storage, caches, or migrations | Which invariant must hold across writes? Check transaction boundaries, stale or cross-tenant cache entries, readers of old records, and mixed-version deployment order. |
| Jobs and events | Trace a duplicate delivery or a crash after a side effect but before acknowledgement. Locate the idempotency decision, retry limits, and how the caller learns the final outcome. |
| AI or an external provider | Trace timeout, malformed output, and quota exhaustion. Check output validation before persistence, data sent to the provider, bounded retries and cost, and whether fallback preserves the promised result. For a prompt or model change, inspect representative evaluations when available. |
| Runtime or performance | Use the expected workload to assess query count, pagination, concurrency, cancellation, and resource cleanup. Check whether existing signals distinguish rejection, dependency failure, and successful completion. |

For serverless changes, match the deployed trigger and its filters to the registered entry point. Trace what happens if an invocation writes state and then times out before acknowledging delivery. Check which operation owns deduplication and whether worker output can retrigger intake. The [serverless architecture guide](../../guides/SERVERLESS-ARCHITECTURE.md) includes a worked webhook example.

## Report and verify

For each material issue, give its severity, location, triggering input or event sequence, observed evidence, impact, smallest practical correction, and a focused verification step. Distinguish a confirmed defect from a question about an undocumented contract. For a diff, identify issues introduced or worsened by the change and label relevant pre-existing issues separately.

Run appropriate existing checks within the review scope. Report what actually ran, what it established, and what remains unverified. When no material issues emerge, name the paths and failure cases examined. Keep a review read-only unless implementation is requested.
