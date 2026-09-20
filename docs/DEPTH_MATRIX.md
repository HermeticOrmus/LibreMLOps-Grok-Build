# Depth matrix

Update this table when melting. Status words mean what they say:

| Status | Meaning |
|--------|---------|
| stub | Thin cue only. Usable as a reminder, not a playbook. |
| melted | Real Grok skill: when-to-use, steps, measurable checks, example, output shape. |

Never copy Claude plugin / agent / command totals into this inventory. Upstream [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) is proof that the *job* exists, not a count this repo has earned.

| ID | Kind | Status | Source (Claude, for melt) | Notes |
|----|------|--------|---------------------------|-------|
| experiment-track | skill | melted | plugins/experiment-tracking | Run card: id, code/data refs, reproduce. No invented metrics. |
| model-eval | skill | melted | plugins/model-evaluation | Eval card: primary, holdout, slices, gate, promote/reject/hold. |
| rag-architecture | skill | stub | plugins/rag-architecture | Chunk/retrieve/cite cue only. |
| model-deploy | skill | stub | plugins/model-deployment | Canary/rollback cue only. |
| data-pipeline | skill | stub | plugins/data-pipelines | Schema/version cue only. |
| model-monitor | skill | stub | plugins/model-monitoring | Drift/SLO cue only. |
| prompt-eval | skill | stub | plugins/prompt-engineering | Fixture/rubric cue only. |
| feature-store-lite | skill | melted | plugins/feature-engineering | Train/serve checklist: as-of joins, parity, freshness. No invented PSI. |
| mlops-orchestrator | agent | stub | LibreMLOps-Claude-Code | Coordinates the skills; not a melted specialist. |

This repo now: **3 melted skills**, **5 stub skills**, **1 stub agent**.

Dogfood copies of every skill live at `.grok/skills/<name>/SKILL.md` and must match `skills/<name>/SKILL.md`.

## Suite

Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os). Sibling Libre*-Grok-Build packs: [README suite footer](../README.md).
