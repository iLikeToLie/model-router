# Model Router

**Choose a suitable model and reasoning effort for every phase of an implementation plan.**

A Claude Code skill for workflows where Claude plans and Codex implements. Model Router annotates your plan with model recommendations, effort levels, verification gates, and escalation rules.

[Read SKILL.md](model-router/SKILL.md) · [Report an issue](https://github.com/iLikeToLie/model-router/issues)

## What it does

- Recommends a Claude planner model and effort.
- Assigns a Codex model and effort to each implementation or substantive review phase.
- Distinguishes confirmed model availability from provisional recommendations.
- Includes verification gates and conditions for escalating to a stronger setting.
- Provides executable handoffs only after model, effort, and plugin options are confirmed.

Recommendations are advisory. The skill does not automatically dispatch work or change your runtime configuration.

## Installation

Clone the repository and copy the skill into your personal Claude Code skills directory:

```bash
git clone https://github.com/iLikeToLie/model-router.git
mkdir -p ~/.claude/skills/model-router
cp model-router/model-router/SKILL.md ~/.claude/skills/model-router/SKILL.md
```

For a single project, place the file at `.claude/skills/model-router/SKILL.md` inside that project instead.

**Looking for the skill?** The complete instructions are in [model-router/SKILL.md](model-router/SKILL.md).

## Usage

Ask Claude Code to apply the skill while drafting or revising a plan:

> Use model-router to annotate this implementation plan. Recommend a Codex model and effort for each phase, keep Claude as the planner, and include verification gates and escalation rules.

For a new project:

> Plan an AIS importer and trajectory predictor. Apply model-router to recommend the model and effort for each implementation phase.

Provide the available models and supported effort settings when possible. If availability cannot be established, the skill labels recommendations **provisional** and retains Codex defaults for the handoff.

## Expected output

Your plan gains a **Model routing** section containing a Claude planner recommendation and a table:

| Phase | Model | Effort | Why | Verify | Escalate |
| --- | --- | --- | --- | --- | --- |
| Parse input records | Smallest sufficient available model | Medium, if supported | Clear schema and bounded parser | Fixtures and memory limits | Unexpected formats |
| Build trajectory predictor | Stronger available model | High, if supported | Numerical and temporal edge cases | Backtest and holdout accuracy | Two focused corrections fail |
| Update documentation | Smallest sufficient available model | Low, if supported | Mechanical edits | Review instructions and examples | Requirements conflict |

Actual model IDs and effort values depend on your installation. Confirmed choices may include a copy-ready Codex handoff.

## Workflow

1. Draft the implementation plan using your usual planning workflow.
2. Apply Model Router to assess each phase.
3. Confirm model availability and plugin options.
4. Review the annotated plan and dispatch approved phases.
5. Use verification results to decide whether to escalate or reduce effort.

## Compatibility

Designed for **Claude Code** planning and **Codex** implementation or review, including workflows using the Codex plugin for Claude Code. It complements existing planning, implementation, and review skills.

This repository distributes a standalone skill folder.

## Principles

**Choose the least resource-intensive reliably sufficient option.** Large mechanical edits do not automatically require the strongest model.

**Route each phase independently.** An algorithm and a documentation update can use different settings.

**Clarify before escalating.** More reasoning effort cannot repair missing requirements.

**Keep savings claims honest.** Recommendations are heuristic and do not promise measured token savings or additional subscription capacity.

## Repository contents

| File | Purpose |
| --- | --- |
| [model-router/SKILL.md](model-router/SKILL.md) | Complete skill instructions |
| [README.md](README.md) | Overview, installation, and usage |

## Feedback

Open an [issue](https://github.com/iLikeToLie/model-router/issues) with the task, recommended routing, and observed outcome. Exclude credentials and sensitive project data.
