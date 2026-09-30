---
name: prompt-eval
description: "Stub, not a playbook. Prompt/LLM eval: fixtures, rubrics, regression suites. Real depth: the prompt-engineering plugin, grok plugin install prompt-engineering@libre-mlops-grok --trust; LLM output metrics are in model-evaluation."
---

# Prompt Eval

Stub, not a playbook. Real depth: the [`prompt-engineering`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/prompt-engineering) plugin from LibreMLOps-Claude-Code, which tests prompts against labeled examples, uses LLM-as-judge, and versions prompts with their evaluation results. LLM output metrics (BERTScore, G-Eval) are in [`model-evaluation`](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code/tree/main/plugins/model-evaluation). Install from this marketplace: `grok plugin install prompt-engineering@libre-mlops-grok --trust`.

Prompt/LLM eval: fixtures, rubrics, regression suites.

## Steps
1. Fixture set.
2. Rubric / judge.
3. Regression gate.
4. Cost tracking.
5. Change log for prompts.
