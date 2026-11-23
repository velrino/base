# Repository layout

Choose the first level by how people build, run, consume, or maintain the project. A small tool can keep its source at the root. Introduce a directory when its name answers a real navigation question.

| Location | When it earns a place | What belongs there |
| --- | --- | --- |
| `frontend/web/` | A browser client has its own build, navigation, and release target | The application entry point, feature modules, browser integrations, and UI |
| `backend/api/` | Clients call a service that owns trusted operations | Request handling, access checks, domain operations, and storage integrations |
| `backend/worker/` | Background jobs need a separately operated process | Job entry point, delivery handling, retries, and calls into the capability that owns the work |
| `cli/` | People or automation need repeatable commands with stable inputs and outputs | Argument parsing, command entry points, output formatting, and exit behavior |
| `docs/` | Decisions, contracts, or operating instructions outgrow the README | Topic guides, decision records, and flow walkthroughs linked from an index |
| `evals/` | The team repeatedly compares behavior across representative cases | Cases, expected outcomes, a runner, scoring criteria, and report conventions |
| `research/` | An investigation produces observations that inform a later choice | Sources, experiments, constraints, and unresolved questions |

These paths illustrate responsibilities. A standalone mobile client can use the platform's native layout. A library can organize its root around its published API and examples. A worker that runs inside an existing service can start as a module within that service.

## Worked example

A document extraction tool initially reads one file and writes structured output. A CLI can own that entire workflow. When a web client needs the same extraction rules, expose them as a module that both entry points can call. If a large batch must keep processing after the request ends, introduce a job boundary. Place a separately deployed worker in its own directory when it has its own start command and operating lifecycle.

Keep a short map of actual entry points in the project README. For each one, record who calls it, how to run it, and where its configuration is documented.
