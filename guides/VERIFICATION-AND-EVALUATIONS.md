# Verification and evaluations

Choose the evidence that answers the question. A single capability can need more than one kind of check.

| Question | Check | Example |
| --- | --- | --- |
| Does this rule behave as specified? | Focused test | An expired invitation returns the expected outcome. |
| Do these components agree at their boundary? | Integration check | A worker can read the exact job payload the API enqueues. |
| Can someone complete the flow? | End-to-end check | Upload a document and observe its completed result in the client. |
| Does a change improve or degrade behavior across representative inputs? | Evaluation suite | Compare extracted invoice fields against reviewed examples. |

## Organize an evaluation

Keep case inputs, expected outcomes, and scoring rules identifiable. Give cases stable IDs. Record which dataset and implementation revision produced a report. For invoice extraction, score field values and source evidence separately so a correct total with the wrong supporting text remains visible. Use a baseline and inspect the cases that changed.

## When a bridge helps

If the evaluation runner and product use the same language and compatible dependencies, call the production function directly. When a language or process boundary prevents that, a small bridge can expose the same function to the runner.

1. The runner selects cases and sends only inputs and case IDs.
2. The bridge invokes the production operation and returns observed outputs with those IDs.
3. The runner compares outputs to expectations and reports both mismatches and execution errors.

JSON over standard input and output is enough for a local bridge. Keep scoring in the runner, business rules in the product, and diagnostic logs separate from machine-readable results. Record which dependencies were simulated. A scripted provider can verify fallback policy; measuring extraction quality on real documents requires evidence from the actual extraction path.
