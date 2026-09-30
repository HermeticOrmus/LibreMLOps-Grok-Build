---
name: experiment-track
description: Experiment tracking: params, metrics, artifacts, lineage. Use when naming a run, writing a run card, or reproducing a candidate that model-eval will gate.
---

# Experiment Track

Write a **run card**: what was trained, on which data, with which knobs, where the artifacts live, and how a stranger reproduces it. A dashboard screenshot is not lineage.

Gold Hat: teach the *why* of each logged field. Logging twenty vanity plots extracts attention; a card that lets the next person re-run the candidate empowers `model-eval`.

## When to use

- Starting a training or prompt-change run that might be promoted
- Someone says "r17 was better" and there is no pointer to data, code, or metrics
- Before `model-eval` — the eval card needs a named run and a frozen holdout
- After a notebook experiment you might actually ship

This is not an MLflow/W&B/Neptune tutorial. Any store that keeps the card is fine. Hand the promote gate to `model-eval` (melted). Hand feature as-of / skew to `feature-store-lite` (melted). Hand live SLOs to `model-monitor` (stub).

Do not invent metric values, run ids, or "reproduced" claims. If you did not open the artifact or re-run the command, mark **unverified**.

## Operating steps

1. **Name the intent** of this run in one line (hypothesis, not "try stuff").
2. **Fill the run card fields below** from what exists in the repo, tracker, or logs.
3. **Pin lineage** — code ref, data ref, feature as-of, random seed if it matters.
4. **State how to reproduce** in commands a person can run, or say you cannot.
5. **Hand the eval** to `model-eval` rather than declaring a winner here.

Stop if the run cannot be pointed at. Inventing an experiment name is extraction.

## Run card fields

### Identity

| Check | Pass | Fail |
|-------|------|------|
| Stable id | Immutable run id or commit+timestamp | "latest" or "final_final" |
| Hypothesis | One sentence you could falsify | "improve the model" |
| Parent | Pointer to the baseline run or prod artifact | Orphan candidate |

### Data and code

| Check | Pass | Fail |
|-------|------|------|
| Code ref | Git SHA or image digest | "main from last week" |
| Data ref | Dataset version, query, or snapshot id | A local CSV with no hash |
| Feature as-of | Same contract as `feature-store-lite` | Features pulled "now" into a historical train |
| Split | How train/val were cut | Random rows with leaking entities |

### Params and artifacts

Log what would change the prediction: architecture, hyperparameters, prompt text, threshold, preprocessing version. Skip decorations.

| Check | Pass | Fail |
|-------|------|------|
| Knobs | Values that affect the model | Empty "params" and a mystery checkpoint |
| Artifacts | Model/prompt/preprocessor paths that still resolve | Broken links, overwritten `model.pkl` |
| Env | Language + key library versions, or a lockfile | "Python on my laptop" |

### Metrics (pointers, not a gate)

This skill **records** where metrics live. It does not promote. If a number appears, it must name the set and n or be **unverified**. Do not copy a leaderboard into existence.

Compare view: same holdout, same primary metric, two run ids. If the compare UI cannot say that, it is a gallery, not a compare.

### Reproduce

A stranger with access should reach the same artifact (or the same metric on a frozen file) from the card. If secrets are required, name the *secret names*, never the values.

## Severity

| Rank | Meaning | Example |
|------|---------|---------|
| Critical | Cannot find the artifact or the data | Overwritten checkpoint; no data ref |
| High | Cannot reproduce the decision | Missing SHA; holdout not frozen |
| Medium | Card incomplete | Seed/env missing on a stochastic train |
| Low | Hygiene | Run title is unreadable |

## Worked example — "r17 was better"

Incoming note: `r17 beat last week. Use that.`

Run card (abridged):

```markdown
## Intent
Unknown — the note has no hypothesis.

## Card
- Id: `r17` — **unverified** (no tracker link, no SHA)
- Code: **unverified**
- Data: **unverified**
- Artifacts: **unverified**
- Metrics: "better" — not a metric

## Findings
1. **Critical — identity.** `r17` is a nickname, not a pointer.
   Remediation: resolve to a run id or commit; if none exists, do not eval.
2. **High — reproduce.** No command, no data ref.
   Remediation: write `train` + `data` pointers before `model-eval`.

## Call
**Cannot hand to eval yet.**

## Teach
If you cannot point at code, data, and an artifact, you do not have a run.

## Leftovers
Promote gate → `model-eval`. Feature as-of → `feature-store-lite`.
```

## Output shape

```markdown
## Intent
[falsifiable hypothesis]

## Card
- Id: …
- Code ref: …
- Data ref: …
- Feature as-of: …
- Params: …
- Artifacts: …
- Metrics pointer: [set + n | unverified]
- Reproduce: [commands | cannot]

## Findings (severity-ranked)
1. **[Critical|High|Medium|Low] — [field].** [what] [why]
   Remediation: …

## Teach
[one reusable sentence]

## Leftovers
- [skill] — [what you did not pretend to finish]
```

## Quality bar

A pass is done when a stranger can find the artifact and the data, or the card honestly says they cannot. Refuse a metric-only paste with no lineage.

## Suite

Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os). Gold Hat: [GOLD_HAT.md](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/GOLD_HAT.md). Sibling Libre*-Grok-Build packs: [README suite footer](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/README.md).
