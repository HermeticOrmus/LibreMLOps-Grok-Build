# LibreMLOps-Grok-Build

**MLOps / LLMOps skills for Grok Build** — ported and melted from [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code), not a dumb copy.

> Status: **v0 public scaffold** — honest stubs. Melt depth next.

## Why this exists

Models without ops rot. LibreMLOps owns experiments, eval, RAG, deploy, monitor — melted for Grok Build. Overlaps Reality OS carefully; honest stubs first.

## Install (<5 min)

See [QUICK_START.md](./QUICK_START.md).

```bash
mkdir -p .grok/skills
cp -R skills/* .grok/skills/
```

Doctrine: install [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os) `AGENTS.md` on the machine first.

## Depth matrix (honest)

| Artifact | v0 scaffold | Upstream Claude (proof) |
|----------|-------------|-------------------------|
| Skills (melted bodies) | 8 stubs → fill next | see upstream suite |
| Agents | 1 (`mlops-orchestrator`) | upstream agents |

Counts on the right are **upstream proof**, not this repo's claim until melted.

## First skills

| Skill | Job |
|-------|-----|
| experiment-track | Experiment tracking: params, metrics, artifacts, lineage |
| model-eval | Offline/online eval harness — metrics, slices, gates |
| rag-architecture | RAG design: chunking, embeddings, retrieval, citation, eval |
| model-deploy | Deploy patterns: batch vs online, canary, rollback |
| data-pipeline | Data pipeline hygiene: schema, validation, versioning |
| model-monitor | Drift, latency, quality SLOs — alert without panic |
| prompt-eval | Prompt/LLM eval: fixtures, rubrics, regression suites |
| feature-store-lite | Lightweight feature store cues: freshness, joins, training-serving skew |

Agent: `AGENTS/mlops-orchestrator.md` — full suite pass.

## Layout (Grok Build)

```
skills/                 # install into .grok/skills or ~/.grok/skills
AGENTS/                 # suite agents
.grok/plugins/          # optional plugin bundle
docs/                   # DEPTH_MATRIX, MELT_RULES
```

## Gold Hat

[GOLD_HAT.md](./GOLD_HAT.md) — empower or extract?

## Suite

- Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os)
- Sibling: [LibreUIUX-Grok-Build](https://github.com/HermeticOrmus/LibreUIUX-Grok-Build) · [LibreSessionFlow-Grok-Build](https://github.com/HermeticOrmus/LibreSessionFlow-Grok-Build) · [LibreGEO-Grok-Build](https://github.com/HermeticOrmus/LibreGEO-Grok-Build)
- Skills packs: [grok-skills](https://github.com/HermeticOrmus/grok-skills) · [grok-build-skills](https://github.com/HermeticOrmus/grok-build-skills)
- Claude proof: [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code)
- https://ormus.solutions

## License

MIT — see [LICENSE](./LICENSE).
