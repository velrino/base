# Module boundaries

Name a module after a capability someone recognizes, such as accounts, publishing, or document extraction. Put the code needed to change that capability together. Let the language and framework determine the filenames.

| Responsibility | Placement question | Example |
| --- | --- | --- |
| Feature | Which capability owns this behavior and its vocabulary? | Account invitation owns invitation acceptance and the errors it can produce. |
| Component or view | Which feature gives this interface its meaning? | An invitation form stays with accounts even when two screens display it. |
| Shared UI | Can unrelated features use it with the same interaction semantics? | A dialog supplies focus handling and dismissal. |
| Shared domain contract | Which independent consumers must agree on the same rule? | An editor and an importer both validate the same document format. |
| Adapter | Which external detail changes independently of the rule? | A storage adapter translates a domain operation into a database call. |
| Host or entry point | Where are implementations chosen and connected? | A browser host supplies navigation and storage. A CLI host supplies files and terminal output. |

## Trace a change

Suppose invitation acceptance gains an expiration rule. The accounts module checks the expiration and returns an outcome. The API maps that outcome to its response. The form explains it to the user. Another screen can call the same public operation without importing the form or knowing how expiration is stored.

Expose a small public entry point for other modules. Keep helpers local until there is a second consumer with the same semantics. Before moving code into a shared area, name its consumers and the behavior they must keep consistent. When those consumers need different rules, keep the differences with their respective owners.

A module can begin as one file. Add folders such as components, commands, or adapters as the implementation acquires those responsibilities. A feature boundary alone does not require a separate deployment.
