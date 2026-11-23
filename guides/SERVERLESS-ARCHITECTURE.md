# Organizing a serverless workload

Group functions by the capability they deliver. A file processor can expose an HTTP webhook, consume storage events, and run document jobs. These entry points can share source code and contracts while having different triggers, permissions, timeouts, and scaling settings. Start a separate workload when ownership, release cadence, or operating requirements justify it.

## Where code belongs

| Location | Responsibility | Example change |
| --- | --- | --- |
| Workload root | Register the entry points the runtime invokes, load configuration, and wire dependencies. | Register a storage event handler under the name referenced by deployment configuration. |
| `internal/api/webhook/` | Own the external HTTP callback protocol. Keep the handler, signature handling, payload translation, and protocol tests together. | A provider changes its callback envelope or signing header. |
| `internal/trigger/` | Translate a platform event into an application operation. | An object-finalized event carries a new storage reference. |
| Capability module, such as `internal/uploads/` | Own the state transition and the rules multiple entry points rely on. | Decide when a set of uploaded files is complete enough to create a job. |
| `internal/workers/` and, when useful, `internal/engines/` | A worker owns the lifecycle of a job. An engine owns a processing algorithm that can run on supplied input. | Recording a terminal failure belongs to the worker. Changing PDF extraction belongs to the engine. |
| Contracts and integrations | Keep event and result shapes where producers and consumers can verify them. Put storage, queue, or provider SDK calls beside the integration that owns them. | Change a storage client while preserving the job result consumed by the application. |

Treat the directory names as examples. Use the runtime's own module conventions. A single callback can start with a handler, an operation, and their tests. Introduce a shared package when multiple workloads need the same maintained behavior.

## Follow one upload

Suppose a storage provider sends a signed callback when an upload finishes. Keep signature verification, payload decoding, and callback tests together because they follow the provider's protocol. The handler verifies the signed bytes, translates the payload into a file reference, and passes it to the intake operation.

Intake records the file and schedules processing. Define what must be durable before acknowledging the callback and which response asks the sender to retry. The worker owns extraction and records the terminal result that clients can observe.

To review this boundary, follow a duplicate delivery. If intake records the file but scheduling fails, a retry must still schedule the missing work. If scheduling succeeds but the acknowledgement is lost, the next delivery should resolve to the same logical job.

## Trace the deployment alongside the code

For each entry point, locate its deployment declaration and match the registered name to the trigger, event filter or route, execution identity, and destination resource. Check timeout and concurrency against the dependency doing the work. For storage events, trace the worker's output path against the input filter so writing a result cannot unintentionally start the same operation again.

Keep invocation-specific data out of reusable process state. Use durable storage or a queue for work that must survive a successful response or instance shutdown. Document what acknowledgement means, what can be retried, and where a caller can observe completion. Link the declaration and a representative recovery check from the workload's documentation.
