---
name: frontend-design-system
description: Use when planning a frontend or implementing or reviewing UI changes. Choose a home for the design system, check changes against existing patterns, and decide whether new UI belongs in the shared system or in a feature.
---

# Frontend design system

Start by locating the components, styles, tokens, and examples the application actually consumes. Check imports on a comparable screen. An older style guide or an example built from look-alike markup can disagree with the shipping implementation.

## Choose a home

| Project context | Practical starting point |
| --- | --- |
| One application with a small UI | Keep reusable UI in the application's shared UI module. Dedicate a page or development route to live examples imported from that module. |
| A growing application or several frontends in one repository | Give shared UI a workspace package or module with a public entry point. A dedicated gallery app can exercise it just like a consumer. |
| Independent applications or teams need separate release schedules | Consider a dedicated design-system project with a gallery, documented exports, and a way for consumers to adopt releases deliberately. |

Use the smallest arrangement that makes the system easy to find, use, and verify. Growth signals include duplicated interaction fixes, multiple consumers coordinating changes, and a need to release independently. If the team already maintains a library or gallery, build on it. Keep product data, branding choices, and navigation policy with the application that owns them.

## Decide where a new UI element belongs

1. **Reuse** an existing component when its meaning and interaction already fit. Check documented variants, composition points, and tokens before adding local CSS.
2. **Compose locally** when the screen combines familiar controls with domain-specific copy, permissions, data fetching, or workflow rules. Give the composition a meaningful feature name.
3. **Extend the shared system** when consumers need the same behavior, a shared accessibility fix, or a deliberate system-wide visual variant. Identify the consumers or invariant, check compatibility with existing uses, and demonstrate the change in the gallery.
4. **Keep an exception local** when sharing it would require feature imports, product-specific flags, or an API built around a single screen. Revisit when another use case clarifies what the two actually share.

For example, a school picker with a logo in its trigger and options can compose the shared Select with school names and permissions from the feature. If several pickers need leading icons, the shared Select can own icon placement, spacing, keyboard behavior, and accessible option names. The school lookup remains in the school feature.

## Review every frontend change

- Compare the changed UI with the closest existing component and a production consumer. Check typography, spacing, color roles, variants, and interaction states. Trace an apparent mismatch to its source before reporting it.
- Exercise the relevant states on the actual screen. For a picker inside a dialog, for example, verify keyboard selection, focus return, and popup placement at a narrow width. Check supported themes and disabled, error, or loading states when the change affects them.
- For shared changes, use the same public component in the gallery and at a representative call site. Add or update the example that demonstrates the changed behavior. For a feature-local composition, verify the feature flow.
- Report the affected screen or file, the shared reference, the observable mismatch, and the smallest practical correction. For new UI, state the placement decision and its reason. Distinguish checks performed from checks still needed. A review request produces findings; implementation requires an implementation request.
