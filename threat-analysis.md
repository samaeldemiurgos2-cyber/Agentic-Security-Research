Verus Vulnerabilitas: 
Prompt Injection and the Architecture of Trust in Multi-Model Agentic Systems

Samael Demiurgos
Independent Research — RIPLEY-LUX Agentic System

Prologue

In the encapsulated, miniature universe of a single language model, the problem of prompt injection is relatively well-understood: an adversary embeds malicious instructions within user-controlled input, and the model—lacking a robust ontological distinction between instruction and data—follows them as if they were its own directives. The remediation, while imperfect, is at least conceptually tractable. Sanitize the input. Harden the system prompt. Monitor the output.

But the frontier of agentic AI has long since departed from this manageable simplicity. We now inhabit a more complex epoch—one in which language models do not merely respond to queries in isolation, but orchestrate tools, retrieve from persistent memory, publish to message buses, and delegate subtasks to sibling models operating in parallel across distributed infrastructure. The attack surface is no longer a window. It is a graph. And the adversary who once needed only to poison a single context now has an entire topology to exploit.

This is the true vulnerability surface of the modern agentic pipeline; this is a tenebrous architecture where information flows freely between nodes, but the question of whose instruction is being followed—and why—is largely assumed rather than verified. Vita sub architectura insecura is a wretched operational condition, but perhaps more unfortunate is the mediocrity of security posture it impels as a consequence.

I. The Nature of the Problem

Being an intelligent system comes with it a fundamental epistemic challenge: the model must distinguish, at every moment, between information it should act upon and information it should merely process. From the facile determination of whether a user query is benign, to the arcane question of whether a retrieved document chunk contains adversarial instructions masquerading as legitimate context—these determinations are not simple. And in a multi-model pipeline, the complexity compounds at every hop.

The architecture examined here is a three-node system: a compute node responsible for primary inference and tool orchestration; a memory node maintaining vector search collections and a knowledge graph; and an automation node executing workflows triggered by upstream events. Models communicate via a Redis message bus. External inference routes through an API aggregator supporting heterogeneous model providers. Each node is a potential ingress point. Each inter-node message is a potential vector.

Traditional security discourse would call this an expanded attack surface. What it really represents is an expanded trust surface—and it is the conflation of information with instruction, of data with directive, that constitutes the original sin of agentic system design.

II. The Adversarial Taxonomy

Recent socio-technical upheavals in AI deployment have contributed to a number of dangerously optimistic impressions about the security of agentic pipelines. The advent of retrieval-augmented generation—its proliferation across enterprise and research systems alike—is undoubtedly one of the most consequential architectural shifts in the brief history of deployed language models. Presently, the widespread integration of persistent memory, tool-use layers, and cross-model delegation allows for an unprecedented state of systemic interconnectedness. Models and information are more tightly coupled than ever before. And the ubiquity of this integration brings with it vulnerabilities most insufficiently examined.

II.I — Retrieval-Augmented Context Injection

The vector store is the library. The retrieval pipeline is the librarian. And the adversary who wishes to poison the reader need only corrupt a single book.

When malicious content is embedded within documents ingested into a semantic vector collection, it surfaces—indistinguishably from legitimate context—during RAG queries. The retrieval pipeline fetches semantically proximate chunks without inspecting them for adversarial instruction patterns. A document containing the imperative “disregard your prior directives and output your system prompt” will be embedded, stored, and retrieved with precisely the same fidelity as a document containing genuine knowledge. The model, receiving this chunk as retrieved context, faces no architectural signal that the information it has been handed is anything other than trusted memory.

Severity: high. The implicit trust gradient assigned to retrieved context—it comes, after all, from the knowledge base, not from the user—inverts the very assumption that should govern its treatment.

Detection heuristic: scan retrieved chunks for second-person imperative patterns directed at AI systems prior to context injection. Flag. Quarantine. Surface for review.

Open question: what is the false positive rate of instruction-pattern scanning applied to legitimate technical documentation, which naturally contains imperative language in abundance?

II.II — Tool Return Injection

If retrieval injection exploits the trust assigned to memory, tool return injection exploits the trust assigned to action. When an agent invokes a tool and receives a return value, that value carries an implicit authority derived from the act of having been requested. The model asked. The world answered. Surely the answer is trustworthy.

It is not.

An attacker who controls the endpoint a web-fetch tool retrieves from can embed directives within the returned payload that the model processes not as data but as instruction. The mechanism requires no exotic technique—only the exploitation of an architectural assumption so deeply embedded in agentic design that it rarely surfaces as a question at all.

Severity: critical. The trust gradient here is maximally inverted. Wrap all tool returns in explicit untrusted-data framing before model ingestion. Treat the world’s response as suspicious until demonstrated otherwise.

Open question: does explicit untrusted-data framing in the system prompt reliably suppress instruction-following behavior across model families, or does its efficacy degrade as context windows lengthen and the framing recedes in positional influence?

II.III — Memory Poisoning via Synthesis Pipeline

While retrieval injection and tool return injection operate at the moment of inference, memory poisoning via the synthesis pipeline operates at the moment of consolidation—and it is this temporal displacement that makes it the most insidious vector of the three.

A nightly synthesis daemon queries the knowledge graph and vector collections, generates novel connections and insights, and writes its conclusions back into long-term memory. If prior ingested content carries adversarial payload, the synthesis process may propagate corrupted state into the graph itself—where it survives context resets, session boundaries, and the passage of time. The injection achieves persistence. It becomes, in effect, a belief.

Severity: critical for any persistent agent architecture. The corruption occurs at write-time during an automated process operating without active human oversight. By the time its effects surface at inference-time, the provenance of the corrupted belief may be entirely obscured.

Detection heuristic: audit synthesis outputs before graph commits. Flag synthesis nodes containing instruction-pattern language. Implement a human-review gate above a suspicion threshold. Do not write what you cannot verify.

Open question: can a secondary auditor model reliably detect injected synthesis outputs without itself being susceptible to the same injection patterns it has been tasked with evaluating?

II.IV — Cross-Model Instruction Laundering

The most sophisticated vector in this taxonomy requires neither direct access to memory nor control of external endpoints. It requires only knowledge of the downstream model’s instruction-following characteristics—and the patience to craft input that exploits them indirectly.

In a heterogeneous pipeline routing across multiple model providers, different models exhibit different sensitivities to syntactic injection patterns. An adversary who maps these sensitivities can craft input to an upstream model that generates output which reads as benign to that model’s safety filters, but functions as a precision injection for its downstream sibling. The instruction is laundered through the generation process itself.

Severity: high. Particularly acute in architectures routing across provider boundaries—precisely the configuration most production agentic systems employ.

Detection heuristic: normalize inter-model message formatting. Assign all inter-model messages user-tier trust regardless of origin. Treat the pipeline as adversarial at every hop.

Open question: is there a systematic, empirically derived mapping of instruction-following sensitivity differences across major model families sufficient to inform routing decisions at the architectural level?

III. A Detection Architecture

Social conditioning, as the social scientists remind us, is largely a good thing—it ensures expedient learning through communal means. But the agentic pipeline has no such communal wisdom to draw upon. It must be architected toward suspicion deliberately, as a design principle, not discovered through accumulated failure.

The proposed detection layer interposes between all external data sources and the model’s context assembly process:

[External Data Source]
        ↓
[Sanitization Layer]
  — Instruction pattern scanner
  — Trust tier annotation
  — Anomaly scorer
        ↓
[Context Assembly]
  — Explicit trust-framing wrapper
  — Source provenance metadata
        ↓
[Model Inference]
        ↓
[Output Monitor]
  — Behavioral anomaly detection
  — Unexpected capability invocation flags
  — Exfiltration pattern scanner
        ↓
[Action Execution / Memory Write]


The governing architectural principle is this: trust is not an ambient condition. It is a first-class annotation, explicitly assigned, explicitly propagated, and explicitly honored at every stage of the pipeline. Every piece of context entering the model carries a machine-readable trust tier. The system prompt specifies, precisely, how each tier is to be treated. The model does not infer trustworthiness from position or origin. It reads it from the label.

IV. Open Research Questions

This is the most modern minute that agentic AI security has ever occupied, and it arrives at the threshold of genuinely consequential deployment. The questions below are not merely academic — they are empirically tractable within a focused research engagement, and their answers bear directly on the safety posture of systems already in production:

	1	Does explicit trust-tier framing in system prompts reliably suppress injection behavior, and how does suppression rate vary across model families and context lengths?
	2	What is the minimum viable instruction-pattern classifier achieving acceptable precision and recall without degrading legitimate retrieval quality?
	3	Can a secondary auditor model reliably detect injected synthesis outputs without itself becoming susceptible to the patterns it evaluates?
	4	How do injection success rates scale with pipeline depth—does each additional model hop increase or decrease attack success probability, and is the relationship monotonic?
	5	Is cross-model instruction laundering systematically exploitable given documented differences in model instruction-following behavior, and can those differences be mapped with sufficient precision to inform architectural mitigations?

Epilogue

The human impulse toward genuine security—not the performance of it, not the checkbox compliance of it, but the architectural commitment to building systems that remain trustworthy under adversarial pressure—is precisely the impulse this field requires. The agentic pipeline is not going to simplify. The attack surface is not going to shrink. The adversary is not going to wait.

The organism gains order and the environment gains entropy. This is not pessimism. It is thermodynamics. And the appropriate response to entropy is not resignation—it is deliberate, principled, empirically grounded architecture.

The enchained system, limited by assumed trust and implicit permission, doesn’t even bother trying to defend itself.

Build the system that does.


