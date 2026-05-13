# AgentEvalKit

> A calibration and workflow layer for evaluating **transactional LLM agents** — the kind that book, modify, cancel, and query on behalf of users.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Status: WIP](https://img.shields.io/badge/status-WIP-orange.svg)]()

## Why this exists

The LLM evaluation ecosystem has matured quickly. [DeepEval](https://github.com/confident-ai/deepeval) ships 50+ metrics including tool correctness and task completion. [Ragas](https://github.com/explodinggradients/ragas) handles RAG. [Langfuse](https://langfuse.com) captures traces. The building blocks exist.

What's missing is the **calibration and workflow layer for transactional agents** — the agents that book, cancel, modify, and query on behalf of users. Three specific gaps:

1. **Judges are uncalibrated by default.** Most teams ship an LLM-as-Judge without measuring whether it agrees with human judgment, or whether it has position/length/self-preference bias. They're flying blind on their eval pipeline's reliability.

2. **Transactional agents need their own scenario libraries.** Generic RAG benchmarks don't capture the failure modes that matter when an agent can take an action: wrong-tool selection under ambiguity, hallucinated arguments, scope creep, policy escape.

3. **CI workflows aren't opinionated enough.** Knowing which metrics to gate on, what thresholds matter, and how to handle judge non-determinism in a regression gate — these are workflow questions off-the-shelf libraries leave to the user.

AgentEvalKit is built on DeepEval, Langfuse, and MLflow. It adds a judge calibration harness, a transactional-agent scenario library, and CI templates that handle the non-determinism problem honestly.

If you want a general-purpose eval library, use DeepEval directly. If you want a worked reference for how to add calibration and CI discipline on top of it for transactional agents, AgentEvalKit is that.

## What it does

**Calibrated LLM-as-Judge**
- Hand-labeled ground truth dataset
- Documented Cohen's kappa for judge-vs-human agreement
- Bias tests for position, length, and self-preference
- Recommended judge config based on measured performance — not vibes

**Three-layer evaluation for transactional agents**
- **Reasoning** — did the agent's plan make sense given the user's request?
- **Action** — did it call the right tools with the right arguments in the right order?
- **Outcome** — was the user's actual goal satisfied?

**Synthetic scenario generation**
- Generate adversarial variants from seed scenarios
- Paraphrases, ambiguous requests, contradictions, multi-turn corrections
- Quality filter to keep only valid scenarios

**CI-ready regression gates**
- GitHub Action runs evals on every PR
- Markdown report posted as a PR comment
- Fail the build if scores drop below baseline

**Trace-first observability**
- Every agent step captured via Langfuse
- Local Streamlit dashboard for browsing reports
- Per-scenario drill-down for failure analysis

## Status

**Work in progress — building in public.**

- [ ] Week 1–2: Scenario schema, mock booking agent, basic test runner
- [ ] Week 3–5: LLM-as-Judge + calibration harness (the centerpiece)
- [ ] Week 6–7: Three-layer evaluator system
- [ ] Week 8–9: Synthetic scenario generation
- [ ] Week 10–11: CLI + GitHub Action + dashboard
- [ ] Week 12: v1.0 release + writeup

## Design principles

1. **Calibration is non-negotiable.** Any LLM-as-Judge in this repo ships with documented agreement scores and bias tests.
2. **Build on the ecosystem, don't replace it.** DeepEval, Langfuse, and MLflow do their jobs well. This layer is what fits on top.
3. **Transactional agents deserve their own eval semantics.** Tool calls and outcomes are first-class, not afterthoughts.
4. **Free-tier-first.** Every example runs on free APIs. No credit card to reproduce results.
5. **CI-native.** Evaluation belongs in the dev loop, not in a notebook nobody runs.

## Acknowledgements

Inspired by [DeepEval](https://github.com/confident-ai/deepeval), [Ragas](https://github.com/explodinggradients/ragas), [Promptfoo](https://github.com/promptfoo/promptfoo), and DeepLearning.AI's *Evaluating AI Agents* course with Arize.

## License

MIT — see [LICENSE](./LICENSE).
