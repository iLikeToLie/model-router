---
name: model-router
description: Use when Claude Code is drafting, reviewing, or revising a multi-phase software implementation plan that may delegate engineering work, debugging, or code review to Codex, especially when model and reasoning-effort choices affect usage limits.
---

# Model Router

## Purpose

Recommend the least resource-intensive **reliably sufficient** model and reasoning effort **per phase** of an implementation plan. Keep Claude's planning choice separate from Codex's execution choices. This skill annotates plans; it is not a replacement for the planning, TDD, or review skills.

## Route the plan

1. **Identify work packages.** Use the implementation plan's independently verifiable phases. Do not split a phase merely to obtain more routing rows. Preserve the user's chosen Claude-planner/Codex-implementer division.
2. **Establish available choices.** Use models/efforts actually exposed by the user's Claude Code and authenticated Codex installation, a trustworthy runtime model listing, or the user's supplied list. The official Codex plugin `openai/codex-plugin-cc` inherits the local Codex runtime/config. An API model catalog alone does **not** prove availability through this plugin or subscription. Configuration defaults alone do not prove that a model is permitted. If availability cannot be established, label model names **provisional** and do not construct an executable command using an unverified model ID.
3. **Assess each phase.** Consider reasoning difficulty, ambiguity, number of interacting components, reversibility/blast radius, verification burden, and autonomy. Distinguish *difficult reasoning* from *large but mechanical work*: file count alone does not justify the strongest model. First improve underspecified acceptance criteria; increasing effort cannot fix a missing spec.
4. **Choose the model tier.** For routine, well-scoped edits choose the smallest capable **available** model; for multi-file implementations and ordinary debugging choose a balanced model; reserve the strongest model for difficult algorithms, unresolved root causes, security-sensitive work, or architecture with costly errors. Prefer scaling **effort on the same capable model** before escalating the model, if the task is failing from insufficient checking rather than a capability gap.
5. **Choose effort.** `low` for trivial changes and explanations; `medium` for normal scoped implementation with tests; `high` for interacting edge cases, deep debugging, migrations, or extensive verification; `xhigh` for unusually challenging work; `max` only when supported and justified by exceptional autonomy/risk. Effort names and defaults vary by model. Match supported values from the runtime. Never default every phase to `high`, `xhigh`, or `max`.
6. **Assign a verification gate and escalation rule.** State what tests, checks, or review must pass. Escalate effort/model if two focused attempts fail, assumptions change, the failure spans modules, or correctness cannot be established. Prefer asking for clarification over blindly raising effort when requirements are ambiguous. Reduce effort for follow-up mechanical cleanup when appropriate.
7. **Annotate, then hand off.** Put the routing section near the top of the finished plan, before implementation tasks. Keep exact task steps unchanged. Recommendations are advisory until the user approves and dispatches.

## Required plan output

Add a concise **Model routing** section:

- **Claude planner:** model, effort, availability status, and one-sentence reason. Recommend a change only when it would materially help; do not change the running session.
- **Codex per phase:** use a table with `Phase | Model | Effort | Why | Verify | Escalate`. Every implementation or substantive review phase gets its own row. Include `confirmed` or `provisional` status in the Model cell.
- **Handoff:** show a copy-ready plugin invocation for each phase **only if** its model and effort are confirmed; otherwise show the plain-language handoff and say to retain Codex defaults until availability is checked.

Example (assuming these models and effort settings have been confirmed in the installed Codex runtime):

| Phase | Model | Effort | Why | Verify | Escalate |
| --- | --- | --- | --- | --- | --- |
| Filter AIS ZIP records | `gpt-6-luna` (confirmed) | `medium` | Clear schema and bounded parser | Fixture tests and streaming memory bound | Unexpected formats across many files |
| Implement trajectory predictor | `gpt-6.1-sol` (confirmed) | `high` | Numerical and temporal edge cases | Backtest and holdout accuracy | Model fails two focused corrections |

Copy-ready example for the second phase:

```text
/codex:rescue --model gpt-6.1-sol --effort high Implement the trajectory-prediction phase exactly as specified in the approved implementation plan. Run its stated tests and report results and remaining risks.
```

## Safety and economy

- **Do not execute** a Codex handoff, spend external API credits, modify `~/.codex/config.toml` or `.codex/config.toml`, or change Claude `/model` or `/effort` merely to make the recommendation.
- Confirm support for `--model` and `--effort` in the installed plugin version before outputting commands. If unavailable, present the model/effort choices as metadata for manual setup.
- Do not claim measured token or subscription savings from a heuristic recommendation. Recalibrate on real outcomes; promote a model only when a cheaper one fails relevant checks.
- Respect the existing implementation plan, user approval gates, allowed tools, and safety restrictions. Routing guidance does not authorize implementation.
