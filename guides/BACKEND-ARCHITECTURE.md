# Organizing a backend

Start with one operation a caller needs. Follow it from input to observable result. For each decision, ask who owns it and which other caller would need the same behavior. A small service can keep its route, operation, and tests together. Split them when the split makes a caller, invariant, or failure boundary easier to find.

## Where code belongs

| Responsibility | Typical home | Placement test |
| --- | --- | --- |
| Deliver an operation over HTTP | `api/endpoints/`, middleware, or a feature's routes | It reads a request, establishes caller context, parses input, or maps an outcome to a status and response. |
| Enforce a business rule | A capability such as `modules/documents/` | The rule still applies when a worker, CLI, or another endpoint calls the operation. Keep resource-level permissions and state transitions enforceable on those paths. |
| Produce a model-assisted result | A feature-local task or a group such as `ai/` | The code defines task inputs, prompt construction, output interpretation, or a fallback. A named task can be called with ordinary values and evaluated directly. |
| Access a database or external provider | The owning feature's data-access code or a named integration | The code translates between product concepts and an external system's query, SDK, or wire format. |
| Accept background delivery | A job handler or worker entry point | It understands delivery attempts, acknowledgement, and retry behavior. It can call the same operation as a synchronous entry point. |
| Assemble the runtime | Startup or the executable entry point | It reads configuration and connects actual clients, routes, and workers for this process. |

Use the project's own vocabulary. A second module needing the same capability can call its public operation. Move code into shared infrastructure when callers need the same contract and failure semantics. A module directory can live inside one executable for its entire lifetime.

## Worked example: summarizing a document

Suppose a client requests a summary in a preferred language. The endpoint reads the document ID and language from the request. The document operation checks whether the caller can read that document. The summarization task receives the authorized text and language, constructs the model request, and validates the result.

If the client starts sending the language in a header, change the endpoint. If the default summary language changes, update the task. If access rules change, update the document operation so the same rule applies when a background job requests a summary.

## What to document

For an important flow, record the entry point, trusted identity, owning operation, dependencies, durable writes, and what the caller sees on failure. If a queue is involved, distinguish acceptance of the job from completion of its work. Draw the flow when it crosses enough boundaries that the order of events is hard to see in code. Use the [flow walkthrough](../templates/FLOW-WALKTHROUGH.md) and apply the [backend review skill](../skills/backend-review/SKILL.md) when a change affects those boundaries.
