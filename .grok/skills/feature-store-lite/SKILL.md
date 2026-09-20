---
name: feature-store-lite
description: Lightweight feature store cues: freshness, joins, training-serving skew. Use when writing a train/serve checklist or checking whether a feature is safe to train and serve.
---

# Feature Store Lite

Write a **train/serve checklist** for the features a model actually uses. Same name in training and serving is not the same feature. Point-in-time joins and parity tests beat a vendor logo.

Gold Hat: teach the *why* of each skew. A list of Feast APIs extracts attention; a checklist that leaves the next engineer able to catch leakage empowers the pipeline.

## When to use

- Before the first training run that will be served
- When offline metrics look great and online quality does not
- Adding or renaming a feature
- Reviewing a "feature store" that is really two scripts and a hope

This is not a Feast/Tecton/Hopsworks tutorial and not a full drift platform. Hand live drift and latency to `model-monitor` (stub). Hand schema/PII/backfill to `data-pipeline` (stub). Hand canary/rollback to `model-deploy` (stub). Call the stub; do not invent its depth.

Do not invent PSI, KS, or freshness SLAs. If you did not measure a delta, mark **unverified**.

## Operating steps

1. **Name the model job and the entity.** Who/what is the join key (user, device, claim)? What timestamp is the label as-of?
2. **Inventory features the model consumes** — not the warehouse catalog. Owner, definition, train source, serve source.
3. **Walk the checklist below.** For each item: pass, fail, or **unverified**.
4. **Propose three concrete fixes** (or fewer if the set is already clean). Each fix names the check it serves.
5. **Teach one sentence** — a reusable skew rule.
6. **Hand off leftovers** (`data-pipeline`, `model-monitor`, `model-eval`) instead of writing a fake store.

Stop if you cannot name the entity and the label time. Guessing as-of semantics is extraction.

## Train / serve checklist

### Feature definition

A feature is a named contract: entity, as-of time, dtype, null policy, timezone, window.

| Check | Pass | Fail |
|-------|------|------|
| Written def | Name, entity, window, null/timezone | A column name only |
| Owner | A person or team who can change it | "ML" with no name |
| Version | Breaking changes get a new name or version | Silent rewrite of `purchase_count_30d` |

### Serving-time availability

Every training input must be computable (or fetchable) at serve time for that entity.

| Check | Pass | Fail |
|-------|------|------|
| Source exists online | Same fields available on the request/store path | Train-only warehouse column |
| No future fields | Values known at decision time | Label, post-event refund, "end of day" total used at noon |
| Cost named | Fetch budget acknowledged | Twenty joins discovered in the request path after train |

A feature you cannot serve is not a feature. Drop it or build the path *before* locking the model.

### Point-in-time joins

Training rows join features **as of the label timestamp**, not "latest now."

| Check | Pass | Fail |
|-------|------|------|
| As-of join | Feature time ≤ label time | Latest snapshot joined to last month's labels |
| Entity grouped | Lags/rollups grouped by entity | Global `shift(1)` across users |
| Window closed | "30d" means the 30 days *before* as-of | Window includes the label event |

Leakage here makes `model-eval` lie. If join code is missing, mark **unverified** and do not treat offline metrics as a promote signal.

### Train vs serve parity

The dangerous bug: two implementations of one name.

Look for: different null fill, rounding, timezone, window inclusive/exclusive, "30 days" as calendar vs 30×24h, category maps fitted on different sets.

| Check | Pass | Fail |
|-------|------|------|
| One implementation | Shared lib / same SQL compiled twice | Notebook transform vs hand-rolled serve |
| Parity test | Same entity+as-of compared train vs serve | No sample comparison |
| Transform fit | Scalers/encoders fit on train only, frozen for serve | Re-fit on production traffic |

How to test without inventing a score: pick a small set of entity ids and as-of timestamps that exist on both paths. Compare values. Report n compared, n mismatched, and the mismatched names. If you did not run that compare, **unverified**.

A distribution test (KS/PSI) is optional evidence. Do not emit a PSI number you did not compute. If you did compute it, report the threshold *you already wrote* — do not invent 0.1 / 0.25 as this pack's law.

### Freshness

Serving features go stale. Training features go stale too.

| Check | Pass | Fail |
|-------|------|------|
| SLA written | Max age the model can tolerate | "We materialize sometimes" |
| Observed age | Last successful build/materialize time | Unknown |
| Failure mode | What the model does if the store is stale (reject, default, hold) | Silent nulls |

If no SLA exists, write a proposed one from the job (not a fake "99.9"). Do not claim current freshness you did not read from a log or table.

### Ownership and change

Who can edit the def, who reviews a breaking change, and how `model-eval` is re-run after a feature change. Unowned features drift.

## Severity

| Rank | Meaning | Example |
|------|---------|---------|
| Critical | Leakage or unservable feature in the model | Future label in a window; train-only column in the vector |
| High | Same name, different meaning | UTC midnight vs local `now()` for `days_since_*` |
| Medium | Missing proof | No parity sample; freshness SLA unwritten |
| Low | Hygiene | Owner field empty; version suffix missing |

If unsure between two ranks, pick the higher and say why.

## Worked example — `days_since_last_purchase`

Job: same win-back model as `model-eval`. Entity: `user_id`. Label time: week-start when the email would send.

Found in two places (illustrative — not your warehouse):

```sql
-- training warehouse
SELECT user_id,
       DATE_DIFF('day', last_purchase_date, CURRENT_DATE) AS days_since_last_purchase
FROM user_mart;
```

```python
# serving request path
days_since_last_purchase = (datetime.now() - last_purchase).days
```

Checklist (abridged):

```markdown
## Job
Win-back at email send time. Entity `user_id`. As-of = send timestamp.

## Inventory
- `days_since_last_purchase` — owner unknown — train: `user_mart` + `CURRENT_DATE`
  — serve: app `datetime.now()` — **parity unverified by measurement**

## Findings
1. **Critical — point-in-time.** Training uses `CURRENT_DATE` (warehouse
   "today"), not the label/send timestamp. Historical rows see *today's*
   recency, not recency as-of the label. Remediation: as-of join
   `DATE_DIFF('day', last_purchase_date, label_ts)`.
2. **High — parity.** Serve uses local `datetime.now()` (timezone + clock)
   vs warehouse UTC date. Same name, different clocks. Remediation: one
   function, one timezone (write it), both paths call it.
3. **Medium — freshness / owner.** No SLA, no owner. Remediation: name an
   owner; write max age before the model should hold.

## Fixes now
1. As-of join on label/send time — serves point-in-time.
2. Shared recency function + timezone — serves parity.
3. Compare n entities at frozen as-of times; report mismatches — do not
   invent PSI.

## Teach
If train and serve do not share a clock and an as-of time, they do not
share a feature.

## Leftovers
Holdout + promote gate → `model-eval` (melted). Warehouse schema/PII →
`data-pipeline` (stub). Live drift after ship → `model-monitor` (stub).
```

That is a train/serve checklist: defs, skew, remediations, leftovers. Not a vendor walkthrough.

## Output shape

```markdown
## Job
[model decision / entity / label as-of time]

## Inventory
| Feature | Owner | Train source | Serve source | Notes |
|---------|-------|--------------|--------------|-------|
| … | … | … | … | [ok / skew / unverified] |

## Checklist
- Definitions: [pass / fail / unverified]
- Serving availability: …
- Point-in-time joins: …
- Train/serve parity (n compared, n mismatch): …
- Freshness SLA + last success: …
- Ownership: …

## Findings (severity-ranked)
1. **[Critical|High|Medium|Low] — [check].** [what] [why]
   Remediation: [exact change]
2. …

## Fixes now
1. …
2. …
3. …

## Teach
[one reusable sentence]

## Leftovers
- [skill] — [what you did not pretend to finish]
```

## Quality bar

A pass is done when every model feature has a train source and a serve source, as-of time is named, and any number (mismatch count, age, PSI) is measured or marked unverified. Refuse "we use a feature store" as a substitute for the checklist.

## Suite

Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os). Gold Hat: [GOLD_HAT.md](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/GOLD_HAT.md). Sibling Libre*-Grok-Build packs: [README suite footer](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/README.md).
