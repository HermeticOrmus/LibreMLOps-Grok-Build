# Quick Start — LibreMLOps for Grok Build

> From a clean machine to one eval card (and a train/serve check) in under 5 minutes.

Doctrine first: put [grok-build-reality-os](https://github.com/HermeticOrmus/grok-build-reality-os) `AGENTS.md` on the machine (global Grok doctrine). This pack does not replace it.

## Prerequisites

- `git`
- Grok Build installed and able to see skills under `.grok/skills/` or `~/.grok/skills/`
- A model or data pipeline you own or are authorized to operate, **or** this repo as the working tree

## Layout this file assumes

Verified against this repository (do not invent extra folders):

```
skills/<name>/SKILL.md          # canonical skill bodies (copy these)
AGENTS/mlops-orchestrator.md
docs/DEPTH_MATRIX.md
docs/MELT_RULES.md
.grok/skills/<name>/SKILL.md    # dogfood copy; must match skills/
.grok/plugins/libremlops-core/  # plugin stub; not required for first run
```

Melted (usable now): `skills/model-eval/SKILL.md`, `skills/feature-store-lite/SKILL.md`, `skills/experiment-track/SKILL.md`.
Still stubs: the other five skills + the orchestrator. Honest table: [docs/DEPTH_MATRIX.md](./docs/DEPTH_MATRIX.md).

## Install (pick one)

### A. Dogfood this repo (fastest)

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git
cd LibreMLOps-Grok-Build
# Skills are already at .grok/skills/ — open this folder in Grok Build.
```

### B. Install into your ML project

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git ~/LibreMLOps-Grok-Build
cd /path/to/your-ml-project
mkdir -p .grok/skills
cp -R ~/LibreMLOps-Grok-Build/skills/* .grok/skills/
```

Confirm the copy landed:

```bash
test -f .grok/skills/model-eval/SKILL.md
test -f .grok/skills/feature-store-lite/SKILL.md
test -f .grok/skills/experiment-track/SKILL.md
ls .grok/skills
```

You should see eight skill directories, matching `skills/` in this repo.

### C. User-global

```bash
git clone https://github.com/HermeticOrmus/LibreMLOps-Grok-Build.git ~/LibreMLOps-Grok-Build
mkdir -p ~/.grok/skills
cp -R ~/LibreMLOps-Grok-Build/skills/* ~/.grok/skills/
```

Same three `test -f` checks as B, under `~/.grok/skills/`.

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

- [ ] `model-eval`, `feature-store-lite`, and `experiment-track` files exist at the install path you chose
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
