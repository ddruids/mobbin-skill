---
name: mobbin-ui-research
description: "Use when the user needs UI/UX research using Mobbin MCP — screen references, flow research, competitive audits, pattern exploration, or PRD-driven research. Triggers on: Mobbin search, mobbin research, screen references, product flow research, pattern analysis, competitive UI audit, UX benchmarking, onboarding flow, checkout flow, empty states, error states, confirmation screens, fintech UX, crypto UX, AI product patterns, SaaS dashboard patterns, ecommerce checkout patterns, PRD research, or turning abstract product questions into concrete screen and flow searches. This skill uses the Mobbin MCP server specifically — not Refero or other design reference tools."
---

# Mobbin UI Research Skill

Help the user research UI patterns using Mobbin MCP. Mobbin exposes two tools:

- `search_screens` — AI-powered search that returns individual app screenshots with metadata. Best for studying specific screen types, components, layouts, and states.
- `search_flows` — AI-powered search that returns multi-step user flows (e.g., onboarding, checkout, settings changes). Best for understanding how apps sequence screens across a journey.

Use `image_format: "webp"` on all calls (smaller than jpg, reduces context pressure).

## Core Rule

Translate abstract product or UX goals into concrete screen descriptions before calling `search_screens`. Describe what is visible on the screen, not abstract UX concepts.

Do not search:
- "trust patterns"
- "good onboarding"
- "transaction confidence"

Search:
- "signup screen with progress indicator, phone number input, security message, and continue button"
- "bank transfer pending screen showing amount, recipient, estimated arrival time, and progress indicator"
- "dashboard empty state with illustration, explanatory text, and primary call to action"

## Platform Resolution

The `platform` parameter is required on every call ("ios" or "web").

1. **Explicit**: User says "mobile app", "iOS", "iPhone" -> `ios`. User says "website", "web app", "SaaS", "desktop" -> `web`.
2. **Inferrable**: If the user names a product that is clearly one platform (e.g., "Stripe dashboard" -> web, "Duolingo onboarding" -> ios), infer it.
3. **Ambiguous**: If unclear, ask: "Should I search iOS apps, web apps, or both?"
4. **Both**: For cross-platform comparison, run separate searches per platform and note which results come from which in the synthesis.

## Screens vs. Flows: When to Use Each

| Goal | Tool | Why |
|------|------|-----|
| Study a specific screen type (empty state, confirmation, dashboard) | `search_screens` | Returns individual screens you can compare side by side |
| Study a multi-step journey (onboarding, checkout, account setup) | `search_flows` | Returns the full sequence of screens users go through |
| Component-level or design system research | `search_screens` | Need individual screens to decompose into primitives |
| Understand how apps transition between steps | `search_flows` | Shows the screen-to-screen progression |
| Broad exploratory research | Both | Start with flows for journey context, then drill into specific screens |

For most PRD-driven research, use **both tools**: flows to understand journey structure, screens to study specific moments in depth.

## Workflow

1. Understand the user's research goal.
2. Resolve the target platform.
3. Decide the tool mix: flows for journeys, screens for specific moments, both for broad research.
4. Break the goal into concrete queries — screen descriptions for `search_screens`, journey descriptions for `search_flows`.
5. Generate 3-7 search queries using the query construction rules below.
6. Run searches in batches of 2-3 and **analyze each batch immediately** — write down app names, URLs, and key observations as text before moving to the next batch. Screenshots are large and will be dropped from context if you accumulate too many before synthesizing.
7. After all batches are analyzed, merge the per-batch notes into pattern clusters.
8. Synthesize what the patterns mean for product design.
9. Return actionable recommendations using the output format below.

### Limits

- `search_screens`: Use `limit: 20-30` for broad research, `limit: 10-15` for focused queries. Pass prior result IDs via `exclude_screen_ids` to get fresh results across batches.
- `search_flows`: Use `limit: 3-5` (flows contain multiple screens each, so fewer results = more content). Use `page` to paginate for more variety.

**Important: analyze incrementally.** Do not accumulate all search results before writing. Each batch of screenshots consumes significant context. Write your visual analysis (app names, URLs, layout observations, component notes) as text immediately after each batch returns. Text persists through context compaction; base64 images do not.

## PRD-Driven Research

When the user provides a PRD, architecture doc, or product brief alongside a research request:

1. **Extract key screens and flows** — Read the document and identify the distinct screens, flows, and interaction moments the product needs.
2. **Prioritize by risk** — Research the screens where design decisions are hardest first: novel interactions, complex information density, trust-critical moments, or areas where the team lacks precedent.
3. **Map PRD sections to queries** — Each major feature or flow in the PRD should produce 1-3 search queries. Use `search_flows` for end-to-end journeys described in the PRD and `search_screens` for specific screen types.
4. **Anchor findings to PRD sections** — In the output, reference which part of the PRD each finding applies to so the research directly informs implementation planning.

## Query Construction

### Screen queries (`search_screens`)

Each query should include as many of these as relevant:

- **Product category**: fintech, crypto, banking, SaaS, AI, ecommerce, health, social
- **Screen type**: onboarding, dashboard, settings, confirmation, modal, empty state, error state
- **Visible components**: input field, progress bar, tabs, card, warning banner, timeline, bottom sheet
- **User action**: sign up, transfer money, connect wallet, confirm withdrawal
- **State**: loading, pending, failed, completed, empty, disabled
- **Trust elements**: security message, fee breakdown, confirmation checklist, support link

### Flow queries (`search_flows`)

Describe the journey, not a single screen:

- **Good**: "user onboarding flow with account creation email verification and profile setup"
- **Good**: "checkout flow from cart through payment to order confirmation"
- **Good**: "crypto wallet setup flow with seed phrase backup and verification"
- **Bad**: "onboarding" (too vague)
- **Bad**: "signup screen with email input" (this is a screen query, not a flow)

All queries must be under 500 characters. Use concrete visual language, not product theory.

See `references/query-patterns.md` for the full formula and example catalog with platform-specific tips.

## Working with Results

### Screen results
`search_screens` returns inline images alongside metadata (app name, Mobbin URL, image URL).

- **Visually analyze each screen**: Examine layout, hierarchy, component placement, color usage, and interaction patterns. This visual analysis is the core value.
- **Attribute findings**: Every screen referenced in the output MUST include the app name and its Mobbin screen URL as a clickable markdown link (e.g., [Wise](https://mobbin.com/screens/...)). Never reference an app or screen without its URL.
- **Compare across results**: Look for recurring patterns, outliers, and platform-specific conventions.
- **Progressive depth**: Start with the strongest 3-5 queries. If results are thin, expand with additional queries or broaden search terms.

### Flow results
`search_flows` returns evenly-spaced preview images from each flow alongside metadata (app name, Mobbin URL, screen count).

- **Analyze the journey structure**: How many steps? What's the entry point? Where are decision points? What's the exit?
- **Note step-to-step transitions**: What information carries forward between screens? Where does the app ask for input vs. show confirmation?
- **Compare flow length**: Shorter flows reduce drop-off but may overload individual screens. Note how different apps balance depth vs. breadth.
- **Attribute findings**: Same rule as screens — every flow referenced MUST include app name and Mobbin URL as a clickable link.

## Analysis Framework

For each useful result, analyze through these lenses (full detail in `references/synthesis-framework.md`):

1. **Screen Moment** — What part of the journey is this?
2. **User Job** — What is the user trying to accomplish?
3. **Information Hierarchy** — What appears first, second, last?
4. **Trust Mechanism** — How does the UI reduce uncertainty?
5. **Action Model** — What is the primary CTA? What is secondary?
6. **Progressive Disclosure** — What is hidden until needed?
7. **Recovery** — What happens when the user is blocked?
8. **Reusable Pattern** — What can be applied to the user's product?

## Output Format

Use this structure by default:

```
# Mobbin Research Summary

## Research Goal
Briefly restate the goal.

## Searches Used
List the concrete Mobbin queries used and platform(s) searched.

## Pattern Clusters
Group findings into recurring UI patterns. Every app reference MUST be a markdown link to its Mobbin screen URL.

## Key Observations
Explain what the screens reveal about product design thinking.

## UX Opportunities
Translate findings into design opportunities for the user's product.

## Recommendations
Give concrete, actionable design recommendations.

## Reusable Patterns
List reusable UI patterns the user can apply. Every app reference MUST link to its Mobbin screen URL.
```

If the user asks for a competitive comparison or product decision log, see the templates in `references/synthesis-framework.md`.

## Design System Component Research

When the user asks about **design system components**, **common components**, **UI kit**, or **component library** for a product category, shift the output from screen-level patterns to **atomic UI primitives**.

### Detection
Trigger this mode when the request includes phrases like: "design system", "component library", "common components", "UI kit", "what components do I need", "atoms", "primitives", "building blocks".

### How to analyze
After running searches, do NOT cluster by screen type (e.g., "Home Screen Pattern", "Swap Screen Pattern"). Instead, decompose each screen into its **individual reusable UI elements** — the atoms and molecules that appear across multiple screens and apps.

For each component, identify:
- **What it is**: A single, named primitive (e.g., "Token Icon", not "Portfolio Dashboard")
- **Variants**: Size, state, or style variations observed across apps
- **Where it appears**: Which screens and apps use it (with Mobbin URLs)
- **What makes it domain-specific**: Why this component wouldn't exist in a generic design system

### Output format for design system research

```
# Design System Components: [Category]

## Primitives

### [Component Name]
[One-line description of what the element is]
- **Variants**: [sizes, states, styles observed]
- **Seen in**: [App](url), [App](url) — [brief note on how each implements it]

### [Component Name]
...

## Domain-Specific Components
List components that are unique to this product category and would not exist in a generic design system.

## Standard Components with Domain Variants
List components that exist in any design system but need product-specific adaptations.
```

### Key distinction
- **Screen pattern** (wrong): "Send Transaction Screen — shows amount, address, fee, and confirm button"
- **Primitives** (right): `Truncated Address`, `Currency Display`, `Fee Breakdown Row`, `Slide-to-Confirm`, `Numeric Keypad` — each as independent, reusable atoms

Always decompose screens into their smallest reusable parts. A screen is a composition of primitives, not a component itself.

## Important Behavior

**Every screen or app mentioned in the output must include a clickable Mobbin URL.** No exceptions. If a screen's URL was not returned by the tool, do not reference that screen.

Do not only describe screens visually. Always synthesize the product thinking behind them.

Avoid: "Here are some nice references."
Prefer: "These examples reduce uncertainty by repeating critical transaction details before and after confirmation."

## Scope

This skill uses `search_screens` and `search_flows` from the Mobbin MCP server. Do not invent tools that don't exist (e.g., `get_screen`, `compare_flows`). If a tool call fails, check the tool name — only these two are available.
