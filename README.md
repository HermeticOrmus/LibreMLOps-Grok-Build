<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_eye.gif" alt="LibreMLOps Grok Build" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">LibreMLOps Grok Build</h1>

<p align="center">
  <em>MLOps and LLMOps depth for Grok Build: Grok-native skills plus every LibreMLOps pack plugin, from one marketplace</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/LibreMLOps-Grok-Build?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/LibreMLOps-Grok-Build?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/LibreMLOps-Grok-Build?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/MLOps-aa8142?style=flat-square&logo=pytorch&logoColor=white" alt="MLOps" />
  <img src="https://img.shields.io/badge/Grok_Build-aa8142?style=flat-square&logo=x&logoColor=white" alt="Grok Build" />
</p>

---

**MLOps / LLMOps depth for [Grok Build](https://github.com/HermeticOrmus/grok-build-reality-os)** — ported and melted from [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code), not a dumb copy.

> Status: **v1.0.0**. Three skills are melted into the Grok-native plugin `libre-mlops-grok` (`model-eval`, `feature-store-lite`, `experiment-track`). The other five are honest stubs in [stubs/](./stubs/), each pointing at the pack plugin that holds the real depth. See [docs/DEPTH_MATRIX.md](./docs/DEPTH_MATRIX.md) and the [kintsugi ledger](./LEDGER.md).

## Why this exists

Models without ops rot. LibreMLOps owns the *job* of experiments, eval, features, RAG, deploy, monitor — melted for Grok Build. This repo counts only what it has melted. Overlaps Reality OS carefully.

## Install

One marketplace brings both layers: the Grok-native plugin melted here, and every plugin of the [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) pack, pinned to one commit of the pack. Grok reads the pack plugins as they are.

```bash
grok plugin marketplace add HermeticOrmus/LibreMLOps-Grok-Build
grok plugin install libre-mlops-grok@libre-mlops-grok --trust
```

Grok installs a plugin only with `--trust`, because a plugin can run hooks, MCP servers and skills on your machine. Read what you trust: each entry's source is linked in [.grok-plugin/marketplace.json](./.grok-plugin/marketplace.json).

Then add the pack plugins your models need, for example:

```bash
grok plugin install model-evaluation@libre-mlops-grok --trust
grok plugin install model-monitoring@libre-mlops-grok --trust
```

[QUICK_START.md](./QUICK_START.md) has the loop that installs every entry, the dogfood clone, and the manual copy path. From a clone:

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git
cd LibreMLOps-Grok-Build
# Dogfood: .grok/skills/ holds a copy of the skill bodies and stubs.
# Other project: cp -R plugins/libre-mlops-grok/skills/* /path/to/your-ml-project/.grok/skills/
```

Doctrine: install [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os) `AGENTS.md` on the machine first. Do not replace it with this pack.

## Depth (honest)

| Artifact | This repo now | Where |
|----------|---------------|-------|
| Grok-native skills | 3 melted | `plugins/libre-mlops-grok/skills/` |
| Stub skills | 5, not installed | `stubs/`, each names its pack plugin |
| Agents | 1 stub (`mlops-orchestrator`), not installed | `AGENTS/` |
| Plugins in the marketplace | 1 Grok-native + 21 pack entries | `.grok-plugin/marketplace.json`, pinned to one pack commit |

Pack entries are LibreMLOps-Claude-Code plugins installed through this marketplace. They are not melted here, and they are not counted as this repo's skills. Do not paste Claude plugin/agent/command totals here. Update [docs/DEPTH_MATRIX.md](./docs/DEPTH_MATRIX.md) when something melts, and run `scripts/pin-pack.sh` when the pack changes.

## Skills

| Skill | Status | Job | Pack plugin |
|-------|--------|-----|-------------|
| experiment-track | melted | Run card: params, artifacts, lineage | melted from `experiment-tracking` |
| model-eval | melted | Eval card: metrics, slices, gates | melted from `model-evaluation` |
| feature-store-lite | melted | Train/serve checklist: freshness, joins, skew | melted from `feature-engineering` |
| rag-architecture | stub | Chunking, retrieval, citation, eval | real depth in `rag-architecture` |
| model-deploy | stub | Batch vs online, canary, rollback | real depth in `model-deployment` |
| data-pipeline | stub | Schema, validation, versioning | real depth in `data-pipelines` |
| model-monitor | stub | Drift, latency, quality SLOs | real depth in `model-monitoring` |
| prompt-eval | stub | Fixtures, rubrics, regression suites | real depth in `prompt-engineering`, with LLM output metrics in `model-evaluation` |

Agent: `AGENTS/mlops-orchestrator.md` — stub coordinator for a multi-skill pass.

## Layout (Grok Build)

```text
plugins/libre-mlops-grok/  # the Grok-native plugin: manifest + melted SKILL.md bodies
stubs/                     # stub skills + the v0 bundle stub; not installed
AGENTS/                    # suite agents
docs/                      # DEPTH_MATRIX, MELT_RULES
scripts/pin-pack.sh        # re-pins the pack entries to the pack's main
.grok-plugin/              # marketplace: the plugin + every pack plugin, pinned
.grok/skills/              # dogfood copy of the plugin skills and stubs (CI checks it)
```

Claude's `.claude/` maps to Grok skills + `AGENTS.md` + `.grok/`. Melt rules: [docs/MELT_RULES.md](./docs/MELT_RULES.md).

## Kintsugi ledger

Every crack found in v0 and how this release seals it, with the file that shows the seal: [LEDGER.md](./LEDGER.md).

## Feedback and contributing

Tell us what worked and what is missing with the [feedback form](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/issues/new?template=feedback.yml). When Grok picks the wrong skill, file a [routing miss](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build/issues/new?template=routing-miss.yml). Ways to contribute are in [CONTRIBUTING.md](./CONTRIBUTING.md).

## Gold Hat

[GOLD_HAT.md](./GOLD_HAT.md) — empower or extract? No invented metrics.

## Suite

- Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os)
- This pack: [LibreMLOps-Grok-Build](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build)
- Sibling Libre*-Grok-Build packs: [Arch](https://github.com/HermeticOrmus/LibreArch-Grok-Build) · [Copy](https://github.com/HermeticOrmus/LibreCopy-Grok-Build) · [DevOps](https://github.com/HermeticOrmus/LibreDevOps-Grok-Build) · [Embed](https://github.com/HermeticOrmus/LibreEmbed-Grok-Build) · [FinTech](https://github.com/HermeticOrmus/LibreFinTech-Grok-Build) · [GameDev](https://github.com/HermeticOrmus/LibreGameDev-Grok-Build) · [GEO](https://github.com/HermeticOrmus/LibreGEO-Grok-Build) · [MLOps](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build) · [MobileDev](https://github.com/HermeticOrmus/LibreMobileDev-Grok-Build) · [SecOps](https://github.com/HermeticOrmus/LibreSecOps-Grok-Build) · [SessionFlow](https://github.com/HermeticOrmus/LibreSessionFlow-Grok-Build) · [WhatsApp](https://github.com/HermeticOrmus/LibreWhatsApp-Grok-Build)
- Skills collections: [grok-skills](https://github.com/HermeticOrmus/grok-skills) · [grok-build-skills](https://github.com/HermeticOrmus/grok-build-skills)
- Claude proof: [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code)
- https://ormus.solutions

## License

MIT — see [LICENSE](./LICENSE).
