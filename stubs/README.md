# Stubs

A stub is a thin cue: a name, a one-line job, and five steps. It is a reminder, not a playbook, so nothing here installs. Each stub names the pack plugin that holds the real depth, and that plugin installs from this repo's marketplace.

| Stub | Job | Real depth (pack plugin) | Install |
|------|-----|--------------------------|---------|
| [rag-architecture](./rag-architecture/SKILL.md) | Chunking, retrieval, citation, eval cue | [`rag-architecture`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/rag-architecture) | `grok plugin install rag-architecture@libre-mlops-grok --trust` |
| [model-deploy](./model-deploy/SKILL.md) | Batch vs online, canary, rollback cue | [`model-deployment`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/model-deployment) | `grok plugin install model-deployment@libre-mlops-grok --trust` |
| [data-pipeline](./data-pipeline/SKILL.md) | Schema, validation, versioning cue | [`data-pipelines`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/data-pipelines) | `grok plugin install data-pipelines@libre-mlops-grok --trust` |
| [model-monitor](./model-monitor/SKILL.md) | Drift, latency, quality SLO cue | [`model-monitoring`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/model-monitoring) | `grok plugin install model-monitoring@libre-mlops-grok --trust` |
| [prompt-eval](./prompt-eval/SKILL.md) | Fixtures, rubrics, regression suites cue | [`prompt-engineering`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/prompt-engineering) (prompt tests against labeled examples, LLM-as-judge, versioning with eval results); LLM output metrics in [`model-evaluation`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/model-evaluation) | `grok plugin install prompt-engineering@libre-mlops-grok --trust` |

No pack plugin matches `prompt-eval` one to one: `prompt-engineering` holds the prompt testing and versioning, and `model-evaluation` holds the LLM output metrics. Install both if that is your job.

Also here: [libremlops-core/](./libremlops-core/), the v0 plugin bundle stub. It had no manifest and installed only a copy of the stub orchestrator. It is kept as the record; [plugins/libre-mlops-grok](../plugins/libre-mlops-grok/) replaces it.

The suite agent [AGENTS/mlops-orchestrator.md](../AGENTS/mlops-orchestrator.md) is also a stub coordinator. It stays where [AGENTS.md](../AGENTS.md) points, and nothing installs it: you merge it by hand.

## Melt a stub

1. Write the skill to the melted bar in [docs/MELT_RULES.md](../docs/MELT_RULES.md): when to use, steps, measurable checks, a worked example, an output shape. No invented metrics.
2. `git mv stubs/<name> plugins/libre-mlops-grok/skills/<name>`, drop the stub line, and give the frontmatter a routing description (`Use when ...`).
3. Copy it to `.grok/skills/<name>/SKILL.md` (CI checks the copy matches).
4. Update [docs/DEPTH_MATRIX.md](../docs/DEPTH_MATRIX.md), this table, and the README skills table.

The dogfood copies of these stubs in `.grok/skills/` match the files here, so a session opened in this repo sees them described as stubs. One name is shared: the `rag-architecture` stub and the skill inside the pack's `rag-architecture` plugin are both called `rag-architecture` (see [LEDGER.md](../LEDGER.md)).
