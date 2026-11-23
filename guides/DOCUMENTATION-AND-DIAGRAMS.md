# Documentation and diagrams

Use the root README to answer what the project does, how to run its first useful flow, and where to look next. Put a durable explanation under docs when it needs its own audience or changes on a different schedule.

| Reader's question | Best starting point |
| --- | --- |
| What runs, who owns the data, and what connects to what? | A system overview with actors, processes, data stores, and labeled connections |
| What happens after a specific action? | A sequence diagram or numbered walkthrough of one flow |
| Why was this design chosen? | An [architecture decision](../templates/ARCHITECTURE-DECISION.md) with constraints, alternatives, and consequences |
| How do I perform or recover an operation? | A runbook with prerequisites, actions, and an observable completion check |

## Draw one flow

Start with the actor or event and end with an observable result. Introduce each participant at first use. Label arrows with an action and its payload. For asynchronous work, show who acknowledges receipt and who eventually reports completion.

This illustrative flow explains how a user can follow a document upload.

~~~mermaid
sequenceDiagram
    actor User
    participant API
    participant Queue
    participant Worker
    participant Status as Job status
    User->>API: Upload document
    API->>Queue: Enqueue document reference and job ID
    API-->>User: Return job ID
    Queue->>Worker: Deliver extraction job
    Worker->>Status: Save extracted fields and completion state
    User->>Status: Request status by job ID
    Status-->>User: Return result
~~~

Use role names when the question concerns responsibilities. Use actual product names when deployment, permissions, or provider behavior matters. Identify proposed components explicitly. Mark any polling, subscription, retry, or failure path according to what the system actually does.

Keep the editable source beside the explanation. Inline Mermaid can serve as both source and rendered diagram. When a separate image is needed, keep the source, export, and [flow walkthrough](../templates/FLOW-WALKTHROUGH.md) together and link them from the overview.
