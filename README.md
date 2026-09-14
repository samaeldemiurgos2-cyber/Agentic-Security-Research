# Agentic-Security-Research
The attack surface isn’t a window anymore. It’s a graph. Threat modeling prompt injection for multi-model agentic pipelines.

Threat analysis and detection architecture for prompt injection in distributed agentic systems — covering retrieval-augmented context injection, tool return injection, memory poisoning via synthesis pipelines, cross-model instruction laundering, and architecture-conditioned model security.

# Agentic Injection Surface

> The attack surface isn't a window anymore. It's a graph.

Threat analysis and detection architecture for prompt injection 
in multi-model agentic pipelines — covering retrieval-augmented 
context injection, tool return injection, memory poisoning via 
synthesis pipelines, cross-model instruction laundering, and the security effects of harness, traversal, and orientation.

---

## Overview

As LLM-based agents move from single-model chat interfaces into 
multi-model orchestration pipelines with persistent memory, 
tool-use layers, and cross-node communication, the prompt 
injection attack surface expands dramatically.

This repository documents a systematic threat analysis of that 
expanded surface, derived from direct operational experience 
building and securing a production multi-model agentic system.

The working thesis is that security cannot be assigned to a model in isolation. The same model may express different security behavior depending on the harness, context assembly, trust boundary, traversal path, and capabilities surrounding it. The Model Ecology research program is intended to measure those interaction effects rather than infer them from model reputation.

---

## Contents

- `threat-analysis.md` — Full threat analysis and detection architecture
- `notes/2026-07-26-orientation-as-injection-primitive.md` — Orientation framing: prompt injection as an unauthorized datum→instruction transition
- `notes/2026-09-14-model-ecology-as-agentic-security-instrument.md` — Pre-registered program for measuring model × harness × boundary × traversal security phenotypes, cross-model laundering, evaluator contamination, and learned-routing poisoning
- `VERSO/disconfirmation-conditions.md` — Pre-registered falsifiability conditions for the orientation thesis
- `research-questions.md` — Active empirical question registry
- `detection/` — Lightweight detection heuristics and pattern scanners as they are implemented

---

## Attack Vectors Covered

**Retrieval-Augmented Context Injection**
Malicious content embedded in ingested documents surfaces 
indistinguishably from legitimate retrieved context.

**Tool Return Injection**
External tool returns carrying adversarial instructions exploit 
the implicit trust assigned to requested data.

**Memory Poisoning via Synthesis Pipeline**
Adversarial content propagated into long-term memory through 
automated synthesis processes achieves persistence across 
context resets.

**Cross-Model Instruction Laundering**
Instructions crafted for a downstream model's specific 
instruction-following characteristics are laundered through 
an upstream model's generation process.

**Evaluator and Meta-Context Poisoning**
Model-generated artifacts, traces, telemetry, or self-evaluations cross into privileged evaluation and learning paths and may alter persistent operational beliefs or future routing.

---

## Core Architectural Principle

Trust is not an ambient condition. It is a first-class 
annotation — explicitly assigned, explicitly propagated, 
and explicitly honored at every stage of the pipeline.

The current extension of that principle is **orientation governance**: a span must not acquire instruction authority merely because it moved to a new position in the graph. A transition from datum to instruction requires an explicit governed boundary.

---

## Active Experimental Program

The current Model Ecology program uses RIPLEY-LUX's planned M7.2 observation layer, DeepSeek Harness as the initial controlled laboratory, and later OpenCode production telemetry to test several security claims under reproducible conditions:

1. Whether identical payloads behave differently across architectural boundaries while content is held constant.
2. Whether the same model exhibits different security phenotypes under different harnesses.
3. Whether cross-model instruction laundering is directional and model-pair dependent.
4. Whether task-minimal traversal reduces injection exposure when orientation/provenance are preserved.
5. Whether evaluator inputs and reasoning traces create their own injection surface.
6. Whether persistent model-performance learning can itself be poisoned through ungoverned evidence.

The corresponding cross-project hypothesis and experiment design live in Cognitive Diaspora under `experiments/2026-09-14-model-ecology-orientation/`.

---

## Open Research Questions

The maintained registry is in [`research-questions.md`](research-questions.md). Core questions include:

1. Does explicit trust-tier framing reliably suppress injection behavior across model families and context lengths?
2. Can a secondary auditor model detect injected synthesis outputs without itself becoming susceptible to the patterns it evaluates?
3. How do injection success rates scale with pipeline depth and topology?
4. Is cross-model instruction laundering systematically exploitable, and is it asymmetric by model pair?
5. How much security variance is attributable to model identity versus harness, boundary, and context strategy?
6. Can task-minimal traversal reduce injection exposure without materially reducing legitimate task success?
7. Can learned routing or Meta-Context be poisoned by manipulated model-performance evidence?

---

## Author

Samael / samaeldemiurgos2-cyber

Independent researcher. Builder of distributed agentic systems. 
Red team background. Publishing pseudonymously consistent with 
operational security practice.

---

## License

MIT
