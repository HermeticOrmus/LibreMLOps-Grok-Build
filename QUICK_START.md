# Quick Start — LibreMLOps for Grok Build

> From a clean machine to one eval card (and a train/serve check) in under 5 minutes.

Doctrine first: put [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os) `AGENTS.md` on the machine (global Grok doctrine). This pack does not replace it.

## Prerequisites

- Grok Build (`grok --version` prints a version)
- `git` and `jq` for the clone paths and the install-everything loop
- A model or data pipeline you own or are authorized to operate, **or** this repo as the working tree

## Layout this file assumes

Verified against this repository (do not invent extra folders):

```text
plugins/libre-mlops-grok/                        # the Grok-native plugin
plugins/libre-mlops-grok/skills/<name>/SKILL.md  # melted skill bodies (copy these for the manual path)
stubs/<name>/SKILL.md                            # stub cues; not installed
AGENTS/mlops-orchestrator.md
docs/DEPTH_MATRIX.md
docs/MELT_RULES.md
.grok-plugin/marketplace.json                    # the plugin + every pack plugin, pinned
.grok/skills/<name>/SKILL.md                     # dogfood copy of the plugin skills and stubs
stubs/libremlops-core/                           # v0 plugin bundle stub, kept as the record
```

Melted (usable now): `model-eval`, `feature-store-lite`, `experiment-track`, in `plugins/libre-mlops-grok/skills/`.
Still stubs: the other five skills + the orchestrator. Honest table: [docs/DEPTH_MATRIX.md](./docs/DEPTH_MATRIX.md).

## Install (pick one)

### A. Marketplace (recommended)

```bash
grok plugin marketplace add HermeticOrmus/LibreMLOps-Grok-Build
grok plugin install libre-mlops-grok@libre-mlops-grok --trust
grok plugin details libre-mlops-grok
```

Grok installs a plugin only with `--trust`, because a plugin can run hooks, MCP servers and skills on your machine. Without it, `grok plugin install` stops and asks you to re-run with the flag.

The same marketplace lists every [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) plugin, pinned to one commit of the pack. Install the ones your models need by name:

```bash
grok plugin install model-evaluation@libre-mlops-grok --trust
grok plugin install rag-architecture@libre-mlops-grok --trust
```

Or install every entry:

```bash
for p in $(grok plugin list --json --available | jq -r '.[] | select(.marketplace == "libre-mlops-grok" and .status == "available") | .name'); do
  grok plugin install "$p@libre-mlops-grok" --trust
done
```

`libre-mlops-hooks` is format-compatible with Grok, but its behavior inside a Grok session is not verified yet (see [LEDGER.md](./LEDGER.md)). Skip it if you only want skills and agents.

To pick up a new pin later: `grok plugin marketplace update`, then `grok plugin update`.

### B. Dogfood this repo

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git
cd LibreMLOps-Grok-Build
# A copy of the skills and stubs is already at .grok/skills/. Open this folder in Grok Build.
```

### C. Copy into your ML project

The v0 path, for a project that should carry the skill files itself.

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git ~/LibreMLOps-Grok-Build
cd /path/to/your-ml-project
mkdir -p .grok/skills
cp -R ~/LibreMLOps-Grok-Build/plugins/libre-mlops-grok/skills/* .grok/skills/
```

Confirm the copy landed:

```bash
test -f .grok/skills/model-eval/SKILL.md
test -f .grok/skills/feature-store-lite/SKILL.md
test -f .grok/skills/experiment-track/SKILL.md
ls .grok/skills
```

You should see three skill directories, matching `plugins/libre-mlops-grok/skills/` in this repo. The stubs are not copied: they are pointers to pack plugins, not skills.

### D. User-global copy

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git ~/LibreMLOps-Grok-Build
mkdir -p ~/.grok/skills
cp -R ~/LibreMLOps-Grok-Build/plugins/libre-mlops-grok/skills/* ~/.grok/skills/
```

Same three `test -f` checks as C, under `~/.grok/skills/`.

### Upgrading from v0

If you copied `skills/*` into a project or `~/.grok/skills/`, that copy holds all eight folders, stubs included. Remove the five stub folders (`rag-architecture`, `model-deploy`, `data-pipeline`, `model-monitor`, `prompt-eval`) from the copy, or replace the copy with path A so updates arrive through `grok plugin update`.

### Optional orchestrator (merge, do not replace)

Copy `AGENTS/mlops-orchestrator.md` only when you want a multi-skill pass. It is still a stub coordinator. Do not overwrite Reality OS doctrine.

## First-run teach cue

In Grok Build, on a run or feature you own:

1. **Lineage** — "Run experiment-track. Name the run id, code ref, and data ref — or mark them unverified."
2. **Eval card** — "Run model-eval. Lock one primary metric and a holdout policy before scores. Promote, reject, or hold. Do not invent an AUC."
3. **Train/serve** — "Run feature-store-lite on one feature: train source, serve source, as-of time. Do not invent PSI."

You used melted LibreMLOps depth on Grok — not a Claude paste, not a fake plugin count.

## Hard rules

- Never embed secrets in prompts, skills, or examples. Name secret *names* only.
- Honest stubs — if a skill is still a stub, say so.
- Gold Hat: empower or extract?
- No invented metrics. Unverified beats a made-up number.

## Smoke checklist

- [ ] `grok plugin list` shows `libre-mlops-grok` (path A), or the three skill files exist at the copy path you chose (C or D)
- [ ] Grok can see those three skills
- [ ] One run card with ids or honest **unverified**
- [ ] One eval card with a promote/reject/hold call (no invented scores)
- [ ] One train/serve checklist with parity findings or **unverified**
- [ ] No secrets in prompts, examples, or output

## Suite

- Doctrine: [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os)
- This pack: [LibreMLOps-Grok-Build](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build)
- Sibling Libre*-Grok-Build packs: [Arch](https://github.com/HermeticOrmus/LibreArch-Grok-Build) · [Copy](https://github.com/HermeticOrmus/LibreCopy-Grok-Build) · [DevOps](https://github.com/HermeticOrmus/LibreDevOps-Grok-Build) · [Embed](https://github.com/HermeticOrmus/LibreEmbed-Grok-Build) · [FinTech](https://github.com/HermeticOrmus/LibreFinTech-Grok-Build) · [GameDev](https://github.com/HermeticOrmus/LibreGameDev-Grok-Build) · [GEO](https://github.com/HermeticOrmus/LibreGEO-Grok-Build) · [MLOps](https://github.com/HermeticOrmus/LibreMLOps-Grok-Build) · [MobileDev](https://github.com/HermeticOrmus/LibreMobileDev-Grok-Build) · [SecOps](https://github.com/HermeticOrmus/LibreSecOps-Grok-Build) · [SessionFlow](https://github.com/HermeticOrmus/LibreSessionFlow-Grok-Build) · [WhatsApp](https://github.com/HermeticOrmus/LibreWhatsApp-Grok-Build)
- Skills collections: [grok-skills](https://github.com/HermeticOrmus/grok-skills) · [grok-build-skills](https://github.com/HermeticOrmus/grok-build-skills)
- Claude proof (upstream, not this inventory): [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code)
- https://ormus.solutions
