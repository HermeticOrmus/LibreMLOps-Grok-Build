# libre-mlops-grok

The Grok-native LibreMLOps plugin. It carries the skills melted for Grok Build, and only those:

| Skill | Job | Melted from (pack plugin) |
|-------|-----|---------------------------|
| `experiment-track` | Run card: params, artifacts, lineage | `experiment-tracking` |
| `model-eval` | Eval card: primary metric, holdout, slices, gate, promote/reject/hold | `model-evaluation` |
| `feature-store-lite` | Train/serve checklist: as-of joins, parity, freshness | `feature-engineering` |

Install:

```bash
grok plugin marketplace add HermeticOrmus/LibreMLOps-Grok-Build
grok plugin install libre-mlops-grok@libre-mlops-grok --trust
```

The five stub skills are not in this plugin. They live in [stubs/](../../stubs/), and each names the pack plugin that holds the real depth. The same marketplace installs those pack plugins.

Manifest: [.grok-plugin/plugin.json](./.grok-plugin/plugin.json). Honest inventory: [docs/DEPTH_MATRIX.md](../../docs/DEPTH_MATRIX.md). The v0 bundle stub this plugin replaces is kept at [stubs/libremlops-core/](../../stubs/libremlops-core/).
