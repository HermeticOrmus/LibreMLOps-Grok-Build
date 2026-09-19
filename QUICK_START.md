# Quick Start — LibreMLOps for Grok Build

> From zero to an MLOps review cue in under 5 minutes.

## Prerequisites

- Grok Build installed and working
- A model/data pipeline you own or are authorized to operate

## Install skills (repo-local)

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git
cd your-project
mkdir -p .grok/skills
cp -R /path/to/LibreMLOps-Grok-Build/skills/* .grok/skills/
```

Or user-global:

```bash
mkdir -p ~/.grok/skills
cp -R /path/to/LibreMLOps-Grok-Build/skills/* ~/.grok/skills/
```

## First-run teach cue

1. **Eval** — "Run model-eval with a held-out set and clear metric."
2. **RAG** — "Run rag-architecture on chunking, retrieval, and citation."
3. **Monitor** — "Run model-monitor for drift and latency SLOs."

## Hard rules

- Never embed secrets in prompts or examples.
- Honest stubs — melt depth next.
- Gold Hat: empower or extract?

## Smoke checklist

- [ ] Skills visible to Grok
- [ ] One skill run produces measurable output
- [ ] No secrets in output
