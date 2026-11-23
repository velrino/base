# Project organization guide

Use this reference when starting a project or deciding where a new capability belongs. It works for a single script, a library, a web application, or a system with several independently operated parts.

Keep examples self-contained and illustrative. When borrowing a pattern from an existing system, describe the responsibilities and tradeoffs in terms a reader can use in another project. Keep project names, local paths, and private implementation identifiers in the originating repository.

## Start with one working flow

1. Write down who starts the flow, what they want to accomplish, and what they can observe when it succeeds. Use the [project brief](templates/PROJECT-BRIEF.md).
2. Identify what must run to deliver that outcome. For a command-line tool, one executable and a README may be enough. For a browser application with private server operations, identify both the client and the service.
3. Give the first capability a home. Keep its behavior, interface, and checks close enough that someone changing it can find them together.
4. Trace one input through the system and verify the outcome. Record a design decision when another maintainer would reasonably ask why you chose that approach.

## Where to look

| Decision | Reference |
| --- | --- |
| What belongs at the repository root? When does a CLI or worker get its own home? | [Repository layout](guides/REPOSITORY-LAYOUT.md) |
| What belongs to a feature, the host, or shared code? | [Module boundaries](guides/MODULE-BOUNDARIES.md) |
| Where do endpoints, business rules, AI tasks, integrations, and workers belong? | [Backend architecture](guides/BACKEND-ARCHITECTURE.md) |
| How should serverless webhooks, event triggers, and workers be organized? | [Serverless architecture](guides/SERVERLESS-ARCHITECTURE.md) |
| What goes in a README, a decision record, or a diagram? | [Documentation and diagrams](guides/DOCUMENTATION-AND-DIAGRAMS.md) |
| When do tests, evaluations, or a bridge earn their place? | [Verification and evaluations](guides/VERIFICATION-AND-EVALUATIONS.md) |

The templates are starting points for a [project brief](templates/PROJECT-BRIEF.md), an [architecture decision](templates/ARCHITECTURE-DECISION.md), and a [flow walkthrough](templates/FLOW-WALKTHROUGH.md). Copy the one you need and remove prompts that do not apply.

## Reusable skills

Use the [code review skill](skills/review/SKILL.md) to review a file, diff, pull request, or repository. It defines how to trace findings to reachable code, rank them by impact and likelihood, and distinguish verified defects from open questions.

Use the [copywriting specialist skill](skills/copywriting-specialist/SKILL.md) to draft or revise direct-response copy for a specific audience and channel, substantiate claims, and design A/B tests. It defaults to Brazilian Portuguese and adapts to the requested language and market.

Use the [frontend design-system skill](skills/frontend-design-system/SKILL.md) when starting a frontend or changing its UI. It guides the choice between an in-app gallery and a dedicated project, reviews consistency with the shared system, and helps decide which components should stay inside a feature.

Use the [backend review skill](skills/backend-review/SKILL.md) when a change affects service behavior, data integrity, authorization, AI tasks, or an external contract. Trace the affected operation and review the failure cases that matter to its consumers.
