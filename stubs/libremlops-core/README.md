# libremlops-core (Grok plugin stub)

v0 record, superseded in v1.0.0. This folder had no manifest, so `grok plugin install` took it as an unversioned plugin holding only a copy of the stub orchestrator. The melted skills now ship as [plugins/libre-mlops-grok](../../plugins/libre-mlops-grok/). The text below describes the v0 layout.

Bundles core LibreMLOps skills for install-from-path.

Skills live primarily under repo `skills/` with dogfood copies at `.grok/skills/`. Copy or symlink into this plugin's `skills/` when packaging.

Melted in the pack (use these first): `model-eval`, `feature-store-lite`, `experiment-track`. The rest are stubs. Honest table: [docs/DEPTH_MATRIX.md](../../docs/DEPTH_MATRIX.md).
