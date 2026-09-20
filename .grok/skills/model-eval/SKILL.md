---
name: model-eval
description: Offline/online eval harness — metrics, slices, gates. Use when writing an eval card, setting promote/reject rules, or reviewing a holdout you actually have.
---

# Model Eval

Write an **eval card**: one primary metric, a held-out policy, slices, a gate, and a promote/reject call. Truth over flattery. A number you did not compute is not a finding — mark it **unverified**.

Gold Hat: teach the *why* of the metric and the gate. A card that only dumps scores extracts attention; a card that leaves a reusable promote rule empowers the next run.

## When to use

- Before promoting a candidate vs the current production model
- When someone says "the model looks good" and you need a gate
- After a training run that already has a named holdout (or you can name one)
- Reviewing an existing eval report for missing slices or metric shopping

Do not use this as a full monitoring, fairness-compliance, or RAG-citation audit. Hand leftovers to `model-monitor`, `prompt-eval`, and `rag-architecture` (still stubs). Call the stub; do not invent its depth.

Do not invent a quality score, an AUC, a lift, or a "statistically significant" win. If you did not compute it on a named set, it is **unverified**.

## Operating steps

1. **Restate the job** in one line (who is affected, what the model decides, what promote/reject means). If unknown, ask or infer from the repo and say you inferred it.
2. **Lock the card before looking at candidate numbers.** Primary metric, holdout rule, slice list, gate. Changing the metric after seeing scores is shopping — say so if you catch it.
3. **Walk the card fields below.** For each: what you have, what is missing, measured vs unverified.
4. **Call promote, reject, or hold.** Hold is valid. "Looks fine" is not a call.
5. **Teach one sentence** — why this metric and this gate, reusable on the next candidate.
6. **Hand off leftovers** to the matching stub (`model-monitor`, `prompt-eval`, `experiment-track` if lineage is unnamed) instead of writing a fake full harness.

## Eval card fields

### Job and decision

Who is this for, what does a positive/negative (or rank/generation) *do*, and what happens if you promote the wrong model?

If you cannot name the decision, stop. Guessing a metric is extraction.

### Primary metric (one)

One number the gate is about. Secondary metrics may inform; they do not silently replace the primary.

| Check | Pass | Fail |
|-------|------|------|
| Named before scores | Metric written, then numbers | Metric chosen after peeking at a leaderboard |
| Fits the job | Imbalance / ranking / generation has a matching primary | Accuracy alone on a rare-event task |
| Measured or marked | Value + set + n, or **unverified** | A bare "0.94" with no set |

Common pairings (choose what the job needs; do not list them all as computed):

- Rare positives (fraud, churn, defect): PR-AUC / average precision, or F1/MCC at a **pre-declared** threshold
- Ranking / retrieval: a ranking metric on a labeled query set you have
- Generation / RAG: a fixture + rubric (hand to `prompt-eval` / `rag-architecture` if that is the real job)
- Probabilities used as probabilities: include a calibration check (Brier / reliability). Discrimination (AUC-ROC) does not prove calibration

Accuracy on a 92% majority class is not a quality signal. Say that, then pick a primary that can fail.

### Held-out policy

How did rows get into the eval set? Time split, entity split, random row split — name it.

| Check | Pass | Fail |
|-------|------|------|
| No leakage path | Split unit matches the serving unit (user, claim, document) | Random rows when the same entity appears in train and test |
| Time | Future labels are after the feature as-of time | "Latest snapshot" features joined to older labels |
| Size named | n, date range, or **unknown** | "the test set" with no n |

If you cannot see the split code or a written policy, mark **unverified** and do not promote.

### Slices

Overall metrics hide systematic misses. Name 2–5 slices that can change the decision (segment, device, locale, time bucket, confidence band).

| Check | Pass | Fail |
|-------|------|------|
| n per slice | Reported; tiny slices flagged unreliable | A 12-row "segment fail" treated as a gate |
| Same primary | Slice uses the same primary metric | A new metric invented per slice |
| Concern rule | Written (e.g. primary drops more than your gate slack **and** n is enough) | "this group looks worse" with no n |

Do not invent slice scores. If slices were not computed, list the slices you *would* run and mark them unverified.

### Gate and compare

A gate is a rule, not a vibe.

- **Absolute:** primary ≥ written floor on this holdout (floor comes from a prior agreement or prod baseline you actually have).
- **Relative:** candidate vs current prod on the **same** holdout. Overlapping uncertainty is not a win. If you have no interval and n is small, say so — do not claim significance.

Refuse "0.2% better, ship it" without a named comparison set and a pre-written slack.

### Human review sample

Pull a small, named sample (errors, high-confidence misses, slice concerns). Record who looked and what they can veto.

If nobody reviewed and the job is high-cost (money, safety, access), the call is **hold**, not promote.

### Promote / reject / hold

| Call | Meaning |
|------|---------|
| Promote | Gate passed on the named holdout; leftovers are listed, not ignored |
| Reject | Gate failed, leakage, or primary does not match the job |
| Hold | Missing holdout, unverified primary, or no human review when the job needs it |

## Severity (for card defects)

| Rank | Meaning | Example |
|------|---------|---------|
| Critical | Cannot trust the number or the split | No holdout policy; accuracy-only on 8% positives used as the ship gate |
| High | Decision will be wrong for a real group | Required slice missing; candidate compared on a different week than prod |
| Medium | Card is incomplete but the primary is honest | No interval on a small n; calibration unchecked while thresholding |
| Low | Hygiene | Run name missing; secondary metric unlabeled |

If unsure between two ranks, pick the higher and say why.

## Worked example — accuracy-only ship note

Job (inferred from a README): weekly win-back email. Positive = predicted churn. Primary action: promote candidate `r17` vs prod.

Incoming note (not yours — do not treat as measured here):

```text
Ship r17. Accuracy 94%. Looks better than last week.
```

Someone also said the weekly file is "about 8% churn." That base rate is **their claim**, not a number this skill computed.

Eval card (abridged):

```markdown
## Job
Win-back targeting. Promote r17 only if it beats prod on a named holdout
without leaking future labels.

## Card
- Primary: PR-AUC (imbalance). **Unverified** — the note reported accuracy only.
- Holdout: unnamed. **Unverified**.
- Slices: none. Would want tenure bucket + locale; **unverified**.
- Gate: none written. "Looks better" is not a gate.
- Human review: none.

## Findings
1. **Critical — primary metric.** Accuracy 94% with a claimed 8% positive
   rate is compatible with a majority-class dummy. Remediation: lock PR-AUC
   (or MCC at a pre-declared threshold) *before* scoring r17 and prod on
   the same week.
2. **Critical — held-out policy.** No split unit, no n, no as-of time.
   Remediation: one calendar week after train cutoff; features as-of the
   label timestamp (see `feature-store-lite`).
3. **High — compare.** "Better than last week" is not the same holdout as
   prod. Remediation: score both models on one frozen file; write n.

## Call
**Reject** (cannot promote on an unverified accuracy note).

## Teach
If the positive is rare, accuracy is a base-rate echo until you lock a
metric that can fail.

## Leftovers
Lineage of r17 → `experiment-track` (stub). Live drift after promote →
`model-monitor` (stub). Train/serve parity on the churn features →
`feature-store-lite` (melted).
```

That is an eval card: job, measured vs unverified, severity, a call. Not a fake leaderboard.

## Output shape

```markdown
## Job
[who / decision / what promote means]

## Card
- Primary metric: [name] — [value + set + n | unverified]
- Holdout: [policy + n | unverified]
- Slices: [list + n + same primary | unverified]
- Gate: [absolute and/or vs prod]
- Human review: [sample + owner | none]

## Findings (severity-ranked)
1. **[Critical|High|Medium|Low] — [field].** [what] [why]
   Remediation: [exact next measurement or rule]
2. …

## Call
[Promote | Reject | Hold] — [one sentence]

## Teach
[one reusable sentence]

## Leftovers
- [skill] — [what you did not pretend to finish]
```

## Quality bar

A pass is done when every number is sourced or marked unverified, the primary was named before it was used as a gate, and the call is promote, reject, or hold. Refuse vibe-only notes ("solid model", "SOTA-ish") — translate them through this card or drop them.

## Suite

Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os). Gold Hat: [GOLD_HAT.md](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/GOLD_HAT.md). Sibling Libre*-Grok-Build packs: [README suite footer](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/README.md).
