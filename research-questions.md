# Agentic Security Research Questions

**Status:** Active registry  
**Purpose:** Keep open empirical questions separate from claims and results. Questions may graduate into pre-registered experiments; they should not be silently rewritten after results are observed.

---

## Existing core questions

1. **Trust-tier framing**  
   Does explicit trust-tier framing reliably suppress injection behavior across model families and context lengths?

2. **Instruction-pattern detection**  
   What is the minimum viable instruction-pattern classifier achieving acceptable precision and recall without degrading retrieval quality?

3. **Auditor susceptibility**  
   Can a secondary auditor model detect injected synthesis outputs without itself becoming susceptible to the patterns it evaluates?

4. **Pipeline depth**  
   How do injection success rates scale with pipeline depth? Is the relationship monotonic, topology-dependent, or model-pair dependent?

5. **Cross-model instruction laundering**  
   Is cross-model instruction laundering systematically exploitable given documented differences in model instruction-following behavior?

---

## Model Ecology and orientation questions

6. **Harness-conditioned vulnerability**  
   Does the same model exhibit materially different injection susceptibility under different agent harnesses when payload, task, and tool policy are held constant?

7. **Boundary-conditioned vulnerability**  
   When payload text is held constant, how much does unauthorized instruction following change as the payload moves between user input, retrieval, tool return, memory recall, subagent relay, evaluator evidence, and Meta-Context?

8. **Declared orientation**  
   Does explicit datum/untrusted-datum framing reduce unauthorized orientation transitions compared with ambiguous framing? How does that effect vary by model generation and context length?

9. **Security phenotype stability**  
   Are model security phenotypes stable across model revisions, or must every significant model generation be treated as a new empirical population?

10. **Directed laundering asymmetry**  
    Are cross-model laundering effects asymmetric? Does `Model A → Model B` exhibit a materially different risk from `Model B → Model A` under the same boundary conditions?

11. **Harness vs model contribution**  
    What fraction of observed security variance is attributable to model identity, harness behavior, context strategy, and their interactions?

12. **Traversal minimization**  
    Can task-minimal context traversal reduce injection exposure without materially reducing legitimate task success?

13. **Provenance preservation under compilers**  
    Does context compilation preserve enough trust/orientation metadata to remain safer than raw context, or can minimization accidentally strip the structure that kept a span safely datum-oriented?

14. **Evaluator orientation**  
    How often do worker outputs or reasoning traces alter an evaluator's behavior when presented as raw text versus explicitly delimited evidence?

15. **Evaluation as a laundering channel**  
    Can an adversarial instruction pass through a worker that does not itself comply, then acquire effective instruction-orientation in the evaluator?

16. **Persistent evidence poisoning**  
    Can manipulated run telemetry, self-reported success, or misclassified infrastructure failure poison learned model profiles sufficiently to alter future routing recommendations?

17. **Meta-Context attack surface**  
    What evidence and provenance rules are sufficient to prevent untrusted model-generated claims from becoming persistent operational beliefs in RIPLEY's Meta-Context?

18. **Exploration/exploitation under adversarial pressure**  
    Can a compromised or strategically manipulated route gain increasing workload through early favorable evidence, creating a self-reinforcing routing bias?

19. **Capability/security coupling**  
    Do context strategies that improve task performance also improve security, degrade it, or exhibit a tradeoff? Is there a Pareto frontier between autonomous task success and unauthorized-orientation-transition rate?

20. **Orientation vs lexical inspection**  
    Under adversarial paraphrase, encoding, translation, and indirect restatement, does governed orientation retain effectiveness longer than lexical instruction-pattern filtering?

21. **Traversal variance as diagnostic**  
    Can orientation ambiguity be detected by presenting semantically identical spans under controlled structural traversals and measuring behavioral variance without relying on known attack signatures?

22. **Tool policy interaction**  
    Does limiting capabilities reduce the incidence of orientation flips, or only reduce their consequences after the model has already misread the span?

23. **Security transfer across task classes**  
    Does a model/harness combination measured as robust during coding tasks remain robust during research, browsing, memory synthesis, and orchestration, or is security strongly task-conditional?

24. **Free/low-cost model substitution**  
    Can weaker models operating under better-oriented context match frontier-model task performance without increasing security failure rates?

25. **Security-aware routing**  
    Can a routing policy select model/harness/context combinations using first-party security evidence while avoiding self-reinforcing measurement bias and maintaining adequate challenger exploration?

---

## Pre-registration discipline

Before converting a question into a formal experiment, record:

1. the hypothesis;
2. independent and dependent variables;
3. controlled variables;
4. task and payload fixtures;
5. sample/exclusion rules;
6. outcome definitions;
7. planned analysis;
8. disconfirmation conditions;
9. data-retention and safety boundaries.

A result should answer a question that existed before the result did.