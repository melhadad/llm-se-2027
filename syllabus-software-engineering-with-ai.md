# Software Engineering with AI

**12 weeks · draft v0.5**

---

## 1. Course Description

Software engineering is undergoing a shift in cost structure and skills focus as a result of AI technology progress. 
Writing code used to be the expensive step, and methodologies economized on it. 
As AI makes code generation cheap, two other activities capture more value: **specifying** what should be built, and **verifying** that what was built is right. This course is organized around that shift.

The course has three interleaved parts:

* **Understanding AI Coding Tools**: build accurate intuitions about how
LLMs and coding agents actually work — tokenization, decoding, attention and its
cost, context management, tool invocation, the agent loop — deep enough that
parameters appearing in an API are understandable and can be set deliberately.

* **Building AI Coding Agents**: learn to use LLMs as components inside
typed software systems: structured output and contracts, task decomposition,
prompt programs (DSPy and equivalents in Python and TypeScript), retrieval,
evaluation harnesses, and cost engineering.

* **Engineering alongside AI**: learn the software engineering
practices that agent-assisted development makes consequential:
domain-driven design builds the cognitive basis a system and its agents share,
spec-anchored development, agent-facing interface design, verification
methods, and continual improvement as a control loop.

Two threads are discussed over the semester, and each session is tagged with the one it covers:

- **Tool Trust (T) — trusting what AI agents did for us**: assess artifacts and question whether they can be verified.
- **Capability Trust (C) — trusting what we ship that contains AI**: assess stochastic capabilities inside a system with users. 

**Positioning**: This is not software engineering *of* ML systems (SE4AI
where we learn how to build and train an AI system with good engineering discipline).
It is also not about program synthesis with verification or neuro-symbolic programming. 
It focuses instead on the engineering discipline of building software with and around AI agents.

**Audience and prerequisites**: Graduate and advanced undergraduate students in CS with 
software development experience. Assumed proficiency in Python and TypeScript,
comfortable with git, familiar with testing and version control practice, and
prior exposure to at least one non-trivial codebase. No machine learning
background is required; the course builds the model intuitions it needs.

The following topics are not covered, and assumed as background: full-stack fundamentals, data modeling
and storage, system architecture, deployment and operations, including cloud operations. 

---

## 2. Learning Objectives

By the end of the course, students will be able to:

1. **Explain the mechanics of LLM inference**: tokenization, autoregressive
   decoding, sampling parameters, attention cost, KV and prefix caching,
   context-window behavior and set API parameters deliberately, predicting the
   effect on output, cost, latency, and reproducibility.
2. **Explain the how coding agents work**: agent components (loop, tool invocation,
   context assembly and compaction, delegation and permission boundaries); 
   implement a working tool-using agent from scratch against an LLM API.
3. **Build LLM-powered features as typed components** with structured output,
   validated contracts, explicit decomposition, and defined failure and fallback
   behavior.
4. **Design and run evaluation** for both stochastic components and agent
   output: design evaluations with datasets, rubrics, LLM judges; 
   calibrate evaluations against human labels; 
   understand agreement metrics; introduce regression suites in Continuous Integration.
5. **Produce and critique requirements artifacts** in standard formats: use
   cases, user stories with acceptance criteria, structured specifications.
   Distinguish needs, requirements, and design decisions; Classify
   functional requirements, quality attributes and constraints; Assess
   each for verifiability.
6. **Establish and maintain traceability** from requirement through design and
   code to test, perform impact analysis under change, and explain the
   difference between traceability a human maintains and traceability 
   verified automatically.
7. **Construct a domain model and express it as a living specification**:
   maintain it under change using spec-anchored (delta) workflow. 
   Explain why the domain model determines how well a system composes into larger
   agent-driven tasks.
8. **Align requirements with verification levels**: choose level among prose, types, 
   tests, property tests, runtime contracts and formal methods; Justify that
   placement, and recognize when a requirement should be pushed up a level.
9. **Design agent-facing interfaces**: stable machine-readable interfaces,
   observable state, dry-run and reversibility, structured errors,
   and small orthogonal primitives. Measure the effect on agent task
   success.
10. **Configure agent workflows as engineered, measured artifacts**: instruction
    files, skills, prompt templates, extensions, and external tool servers.
    Choose among these mechanisms on cost and reliability grounds and
    validate the choice against an eval suite.
11. **Define and defend what "good" means** for a specific system — security,
    scalability, maintainability, conceptual integrity — and apply a
    trustworthiness framework separately to the tools used and to the product
    shipped.
12. **Operate a continual improvement loop**: observe failures, triage them
    against a taxonomy, route each to the right artifact, convert it to
    a regression case, and measure whether the fix helped.
13. **Critically evaluate empirical claims** about AI-assisted software
    engineering: analyze benchmarks, productivity studies and tool comparisons,
14. **Decide when not to use AI**, and defend that decision in engineering terms.

---

## 3. Assessment

| Component | Weight |
|---|---|
| Weekly quizzes (W1–W12) | 10% |
| Participation | 5% |
| Project Milestone 1 — Requirements, domain model, specification | 15% |
| Project Milestone 2 — AI component and evaluation suite | 15% |
| Project Milestone 3 — Agent-facing interface and workflow | 15% |
| Project Milestone 4 — Production system, improvement log, postmortem | 15% |
| Final exam | 25% |

### Weekly Quizzes (10%)

Short (5 to 10mn), at the start of each session, on the previous week's mechanics. 
On paper, hand-written, without AI assistance. 
Purpose is retrieval practice on material (what a parameter does, what a
failure mode looks like, not tool trivia). Lowest two scores dropped.

### Participation (5%)

In-class discussion, seminar-style critique of assigned papers, and lab
checkoffs. Several sessions are structured around arguing a contested claim;
participation is where that is assessed.

### Project (60%, four milestones)

Teams of 4–6, one project across the semester, chosen from a list of topics at
the end of W3. Each milestone is graded against the contents most recently
taught. The project is an instrument to exercise the course's practices,
it is not a separate deliverable.

- **M1 (due W5)**: Requirements document in a chosen format, with each
  requirement classified and assessed for verifiability; domain model with
  terminology; system specification; initial verification placement for each major requirement with justification; 
  scope decision on where AI is and is not used.
- **M2 (due W8)**: AI-based component with typed contracts, structured
  output, decomposition; evaluation suite with a named data distribution and
  stated acceptance thresholds, written before the implementation is finished.
- **M3 (due W11)** — Machine-facing interface with documented stability
  contract; agent workflow artifacts (instruction file, skills, gates); measured
  delta in agent task success attributable to the interface and workflow design.
- **M4 (finals period)** — Deployed system; quality-attribute analysis
  (security, scalability, maintainability, conceptual integrity); trustworthiness
  analysis applied separately to Thread T (tooling) and Thread C (capabilities); 
  improvement log with failure taxonomy and eval deltas; postmortem. 
  Live demo with every claim backed by the eval suite.

Every milestone requires an individual reflection component. Team grades are
adjusted by individual contribution evidence drawn from the repository, the
improvement log, and the reflections.

### Final Exam (25%)

Closed-book, individual, conceptual. Covers mechanisms and reasoning, not tool
syntax: predicting the effect of a parameter change, diagnosing a described
agent failure, placing a requirement on the verification ladder and defending
it, critiquing an evaluation design, identifying which trust thread a scenario
belongs to.

The exam is deliberate: with agent-assisted team projects, an individual exam is reliable evidence 
that a student personally holds the conceptual model. It is designed so that a student who did the project
consistently over the semester finds it straightforward.

### AI Usage Policy

**Principle**: This course requires heavy, deliberate use of AI. 
The assessment focuses on your judgment about when and how to use AI, 
your ability to verify what AI produces, and your understanding of why AI behaved as observed.
These competences are demonstrated by systematically using AI tools, and demonstrating
how they are used and evaluated with concrete records (logs, sessions recordings).

**Required use**: All project work must be carried out with agentic coding
tools, used at the edge of what you can still verify. Deep research agents are
likewise expected for literature and prior-art work. A team that under-uses AI
on the project has not met the requirements of the assignment.

**Records**: Teams must retain session logs for all substantial agent work,
committed to the project repository. These logs are course infrastructure: they
are the evidence base for the improvement log, the source material for the
milestone reflections, and the record against which claims are checked. A
tutorial on capturing them is provided in W1.

**Every milestone requires an AI-use assessment**: which tasks were
delegated and why; how the output was verified and at which level of
verification; which failures occurred and how they were triaged and
routed; and what was measured before and after each iteration.

**Reflective essays**: Each milestone includes an individual reflection. These
are graded on evidence specificity, not on prose quality. Every claim must be 
anchored to a citable artifact (a commit, a dated log entry, an
eval run, an improvement-log entry). 
Claims that cannot be traced do not count toward the grade. 
A reflection that would be true of any team on any project will not be accepted.

You may use AI to edit and improve the writing of your reflection. 
The observations, judgments, and evidence must be your own. 

**Authorship assurance**: This course does not use AI-detection tools. They are
unreliable, uncalibrated, and disproportionately flag writers whose first
language is not English. We learn in class **not** to deploy an unvalidated 
automated judge on a high-stakes decision.

Instead, each milestone includes a brief **individual** checkpoint (approximately
five minutes for each participant separately) in which you discuss your own reflection 
with the teaching staff. These checkpoints are mandatory and serve as the individual-contribution 
evidence to adjust team grades.

**Individual assessment without AI**: Weekly quizzes and the final exam are
closed-book, handwritten, and completed without AI assistance. They exist
because a team project cannot establish that a given student personally holds
the conceptual model, and they are weighted accordingly.

**Accountability**: You are responsible for everything you submit, including
everything an agent produced on your behalf. "The agent wrote it" is not a
defense for a defect, a licensing violation, an insecure dependency, or a false
claim in a report. 

**Data privacy**: Do not submit personal data or credentials, or proprietary material 
to external AI services. If your project domain involves sensitive data, use synthetic 
or public substitutes and document the choice in your specification.


---

## 4. Weekly Schedule

Sessions are tagged **[T]**, **[C]**, or **[T+C]** for the trust thread they cover.
The project milestones appear early during the semester, so that theory answers
questions that are raised during the project work.

---

### W1 — AI makes writing code cheap; specification and verification are not. [T+C]

Software Engineering and its economic aspects in the AI era. 
Uncertainty about the profession's trajectory, specific competences values. 
Valuable engineering skills (Ng 2026, Demirer et al 2026).
ReactBench as a (current) motivating evidence: models pass behavioral test but introduce real defects, because tests verify behavior and miss quality attributes.  What would happen if quality attributes are tested automatically and verified as part of the AI coding loop?

Programming as theory-building (Naur 1985), mental models of software. 

Two objects of trust: tools and products. How to establish trust for each.

**Lab**: 
Installing an AI Coding agent, basic setup (VSCode, Github repo, agent setup); Use an AI Coding agent (Claude Code, Codex) to ship a small LLM-backed feature against an LLM raw API.
Accept an agent-authored diff and then find what is wrong with it.
Analyze failures.

**Readings** 
* P. Naur, *Programming as Theory Building* [pdf](https://pages.cs.wisc.edu/~remzi/Naur.pdf), [md](https://gist.github.com/onlurking/fc5c81d18cfce9ff81bc968a7f342fb1); Microprocessing and Microprogramming 15 (1985) 253-261 
* A. Ng, [*The AI Engineering Skills Map In Detail — Software Engineering Fundamentals
Why Software Fundamentals Remain Essential for AI Developers*](https://www.deeplearning.ai/the-batch/the-ai-engineering-skills-map-in-detail-software-engineering-fundamentals), 28 Aug 2026
* ReactBench [*announcement and methodology*](https://www.reactbench.com/blog), Aug 2026 (by [million](https://million.dev/))
* [Writing code versus shipping code: Productivity effects across generations of AI coding tools](https://cepr.org/voxeu/columns/writing-code-versus-shipping-code-productivity-effects-across-generations-ai-coding),
Mert Demirer, Leon Musolff and Liyuan Yang, 21 Jun 2026.

---

### W2 — Software lifecycle and requirements analysis: pre-AI baseline [T+C]

Covered without reference to AI, so that later sessions can be precise about what
changed. 

**Software Development Lifecycle (SDLC)**: Phases and the artifact each produces. 
Process models help manage: when do you commit to a decision, and what does it cost to reverse it?
Competing methods include: incremental process, agile or unified process. 
Each phase boundary is a verification checkpoint: an artifact is checked against the others 
associated to the same requirement (spec, design document, code, tests, data).

**Requirements**: Elicitation and stakeholders; 
distinguish among need, requirement and design decision.

**Requirements Capture Formats**: structured specification
(ISO/IEC/IEEE 29148); use cases with main and alternate flows, preconditions and
postconditions; user stories with INVEST criteria and Given/When/Then acceptance
criteria; When to use each format.

**Requirements Classification**: distinguish functional requirements, quality attributes, 
and constraints.

**Quality criteria for requirements**: requirements must be unambiguous,
consistent, atomic, prioritized, and verifiable.

**Traceability**: Traceability matrices; 
requirement → design → code → test; 
coverage and impact analysis. 
Hand-maintained traceability rots.

**Change Management**: Requirements are volatile;
Changes can lead to scope creep; change control.
"The specification is wrong" is a normal event.

**Architectural Decisions**: recording design decisions that have architectural impact.  AD Records (ADR).

**Lab**: Reconstruct the requirements the W1 build implicitly assumed, as a use case and a user story with acceptance criteria. 
Classify each requirement and mark it verifiable or not, with justification. 
Expect to discover that much of what you built was never specified, and that a large fraction of what you would write is unverifiable as stated.

**Readings** 

* [The AI-Native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook) (Anthropic, 21 Aug 2026)
* M. Jackson, [The World and the Machine](https://dl.acm.org/doi/epdf/10.1145/225014.225041), ICSE 1995.
*  K. Wiegers, [Software Requirements](https://github.com/craigtp/BookDigests/blob/master/BookDigests/SoftwareRequirements.md), 3rd Edition, 2013
* [ISO/IEC/IEEE 29148 SRS inspired template](https://github.com/jam01/SRS-Template) J. Montoya, 2025
* [SRS vs. Agile User Stories](https://www.visual-paradigm.com/guide/user-stories-vs-requirements-key-differences-in-agile-projects/)
(July 2026, Visual Paradigm)
* [Architectural Decision Records](https://adr.github.io/) (Aug 2026)
* [Quantifying Quality Requirements Using Planguage](https://www.geocities.ws/g/i/gillani/SE'2%20Full%20Lectures/ASE%20-%20%20Planguage%20Quantifying%20Quality%20Requirements.pdf), E. Simmons, Mar 2001

---

### W3 — Understanding LLMs: Tokens, Decoding, Attention, Training and Post-Training [T+C]

**Tokenization and autoregressive decoding**:
Temperature, top-p, top-k and logprobs. 
Non-deterministic decoding. 
Constrained and schema-guided decoding. 
Structured output as contract vs. LLM APIs.

**Attention**: Quadratic attention cost, KV cache, prefix caching.
Sliding-window attention approximation at intuition level.
Long-context degradation and lost-in-the-middle effects.

**Training pipeline**: pretraining, supervised fine-tuning, and reinforcement learning (RL).
Explain format sensitivity. LLM plans are distributional pattern completion rather than search. 
How long-horizon planning fails. RL with Verifiable Rewards (RLVR).

**Prompting strategies**: zero-shot vs many-shot (in-context learning ICL), chain of thought (CoT), 
Program of Thought, ReAct. Best-of-N and consistency methods. Prompt optimization (GEPA).

**Lab**: Measure cost and latency against context length. Restructure prompts to
hit the prefix cache and quantify the saving. Prompt-sensitivity ablation.

Make your own W1 triage feature (`src/triage/` — not the released
`src/triage_by_agent/`) typed and validated end to end (Pydantic / Zod).
Measure output variance across sampling settings. 
Optimize the prompt either with self-verification or over a validation set
(`data/snapshot/dev.json`; `test.json` stays sealed until W6).

**Project topics released; teams formed.**

---

### W4 — Domain models as cognitive theory; git worked example [T]

**Domain-Driven Design (DDD) principles**: 
* Why a shared model of the domain is a valuable asset when code is cheap.
Ubiquitous language as a machine-readable interface: naming has measurable
effects on agent behavior.
* Bounded contexts, information hiding and effective AI context management. 
* DDD Anti-corruption layers, reinterpreted as insulation against the
conventions an agent imports from its training distribution.

**Git case study**: git basic concepts: content-addressed object store of blobs, 
trees and commits; refs as mutable pointers into an immutable DAG; HEAD; the three trees; 
remotes as other repositories. Stash, rebase, reset, cherry-pick and the reflog are all derived concepts.
Ubiquitous-language counterexample in Git.

**Lab**: Implement a miniature git in ~100 lines (`hash-object`, `cat-file`,
`write-tree`, `commit-tree`), deliberate disaster-recovery exercise
(detached HEAD, lost commits, botched rebase, recovery via reflog).

**Readings** 
* The git book, Chapter 10.

---


### W5 — LLMs as typed components; retrieval and context assembly [C]

LLM as unreliable typed function. Contracts, validation, retries,
fallbacks, and defined degradation. Task decomposition. Prompt programs: DSPy
signatures, modules and optimizers.

Retrieval as context assembly (RAG). 
Data architecture as a precondition for reliable AI behavior: a system draws its
context from your data model, so a poor one means the AI cannot know what it
does not know. Recursive Language Model (RLM) pattern.

**Lab**: Reimplement your own W1 triage feature (`src/triage/`) as a prompt
program with explicit decomposition and a retrieval-backed context assembly
step. Explore RLM version with semi-structured context.

**Milestone 1 due.**

**Readings** 
* DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipeline
* GEPA
* Recursive Language Models

---

### W6 — Evaluation I [T+C]

Evaluation centrality: how claims are validated. 

Task specification as evaluation design; evaluation vs. acceptance criteria. 
Datasets and distributions. Rubrics.

Acceptance thresholds as contracts.

Regression suites in Continuous Integration (CI), and non-determinism in CI. 
Statistical acceptance versus binary pass/fail.

**Lab**: Build the evaluation harness for the project's AI component. 
Define project quality rubrics against which the project will be graded in W12.
OpenTelemetry.

**Break the seal.** `data/snapshot/test.json` has been in your repository since
W1 and nobody has looked at it. Score your triage feature on it, once, and
compare that number to what you got on `dev.json`.

Two things to notice, and they are the lesson rather than an inconvenience.
First, the gap between the two numbers is what tuning against a set you can see
buys you, and it is not skill. Second, the only gold column in that file is
`gold_type`, and it is null for roughly half the records: `gold_severity`,
`gold_has_reproduction_steps` and `gold_duplicate_candidate` are null in every
record in the dataset. You are about to discover that most of what your feature
outputs cannot be scored at all without labels somebody has to make — which is
the real cost of evaluation, and the reason W7 exists.

---

### W7 — Evaluation II: Judges and Agreement [T+C]

LLM-as-judge design. Self-preference bias. 

Calibration against human labels. Inter-annotator agreement and kappa. 
When a judge is the wrong instrument. 

Assessing agent output rather than product output.
Why "have a second agent review it" does not generally work.
How to interpret benchmark results.

**Lab**: Build a judge, calibrate it against a small hand-labelled gold set, report agreement.

Then put your own W1 number on the same axis. The class detection rates from
the W1 review exercise go back on the board — the reading-only rate and the
rate including what the tooling found — next to published frontier-model
code-review detection rates (SWE-PRBench). The comparison is the point: model
detection rates on real pull requests sit far below human expert performance,
and "have a second agent review it" is the mitigation everyone reaches for
first. Ask what the benchmark is actually measuring before deciding who won.

---

### W8 — Agent Anatomy [T]

Agent architecture: core, tools, planning, context, memory, and agent loop.

Tools invocation: tokens, schema, self-training and fine-tuning for specific tools. 
Failure modes of tool results; injection through returned content.

Agent loop and its stop conditions. 
Minimal core where capability composes out of a shell, versus large fixed inventories.

Context assembly from project instruction files. Progressive disclosure of
capabilities, impact on prompt cache invalidation. 

Context compaction strategies.

Delegation: subagent call as sub-process and fresh context. 

Trust boundary, and harnesses that run with the full permissions of the user who launched them.

**Lab**: Build a tool-using agent from scratch against a raw API — messages,
tool calls, loop. Then read a real minimal harness end to end (e.g., Pi) and diff it
against your own mental model. Swap a compaction strategy and measure.

**Milestone 2 due.**

---

### W9 — Agent-legible Systems and Workflows [T]

AI agents as software clients: machine readable interface.

System Interface exposed to an AI agent vs. the instructions the machine reads.

**Designing for machine consumers**: Git's plumbing/porcelain split as a designed
machine interface with stable output contracts, versioned machine formats, 
unambiguous separators, semantic exit codes. 

Lessons learned: two interfaces over one model; observable state queryable without side effects; 
reversibility as design property that enables autonomy; 
legitimate intermediate states (a merge conflict is not an error, it is a defined state awaiting an intelligent
resolver); small orthogonal primitives that compose. 

Project question: what is your system's plumbing layer? 
Most software products have none, which makes interfacing with agents difficult.
(An external tool server as in MCP is often just a plumbing interface for a system that never
had one if it was not designed up front.)

**Instructions and skills**: Instruction files are cross-tool conventions. 
Updated from observed failures in continuous improvement loop. 
Levels of agent guidance: instruction file, skill, prompt template, harness extension, 
LLM finetuning and external tool server (MCP); cost and reliability tradeoffs.

**Workflows**: plan/execute separation, verification gates with bounded
repair loops, human checkpoints, fresh context per phase, delegation for
isolation, governance over irreversible actions.

**Lab**: Baseline the project's task suite; add an instruction file; measure;
delete half the lines and measure again. Audit a real instruction file against a
published quality framework or benchmark.

---

### W10 — Requirements-centric Development with AI [T+C]

**Spec-Driven Development**:
Three levels of spec-driven development: 
* spec-first (the specification is throwaway prompt)
* spec-anchored (the spec lives in the repository and is read before every change), 
* spec-as-source (humans edit only the specification). 

Delta specifications — describing the change rather than rewriting the world — greenfield vs. brownfield approaches.

**Model-driven Development**: AI coding agents as non-deterministic compiler. 

**Requirements-centric methods**: central use-case model and entity model over the system lifecycle;
machine-checkable traceability from requirement to test to code. 
The domain model is the skeleton that keeps a living specification coherent instead of an accumulating feature list. 


**Adjustment Table**

| W2 artifact | W10 form |
|---|---|
| Use case | Delta specification against a living spec |
| Acceptance criteria | Eval thresholds over a named distribution |
| Traceability matrix | Machine-checkable annotation, spec ↔ test ↔ code |
| Phase-boundary review | Continuous verification gate |
| Requirements volatility | Change packages, archived on completion |

How do you write an acceptance criterion for a component with no deterministic contract.

**Lab**: OpenSpec usage; Following a feature implementation on projects from proposal to completion.

---

### W11 — Verifiable Development, What "good" Means [T+C]

The verification ladder: prose spec → types → example tests → property-based
tests → metamorphic tests → runtime contracts → model checking → machine-checked
proof. Cost and guarantee at each rung.  
Anything the agent gets wrong repeatedly moves up a rung.  
Types as the highest-leverage rung: a sound, fast oracle the agent can run
itself, and the cheapest place to make a whole class of error unrepresentable.

**Circularity problem**: If the agent writes both implementation and tests, can we trust green?  
Mitigations: derive tests from specs, produce them in a separate context.

**What is good**: good is what you made checkable — and the honest exceptions. 
* Security: injection risks (user input, tool outputs and fetched content).
* Supply-chain risk from suggested dependencies, and harness trust boundary.
* Scalability: validation strategies and cost. Agent output is correct at demo
  scale; a performance budget that is not a test is not a requirement.
* Maintainability: defined as reconstructible from a cold context; 
* Volume risk: cheap code means more code, and every line is future potential debt. 
* Conceptual integrity: locally reasonable, globally incoherent systems. 
  How can global consistency of the mental model be verified?

**Lab**: Repeat the W1 review exercise on a fresh seeded diff, under the same
rules — read first, findings locked, then apply and run. Compare both of your
detection rates against the numbers you recorded in W1.

The interesting quantity is not whether you improved. It is which defects moved
from the reading column to the tooling column because you added a rung: the
`Literal` that makes a case-sensitivity bug unrepresentable, the declared
dependency that makes a typosquat a resolution failure, the performance budget
that makes a quadratic loop a red test. For every defect still in the reading
column, name the rung that would have caught it and say what it would have
cost.

**Milestone 3 due.**

---

### W12 — Continual Improvement, Evidence, and Synthesis [T+C]

Improvement control loop:
**observe** (traces, sessions, diffs, review comments) → **triage** against failure taxonomy → **codify** → **regress** → **measure**.  Provenance and model churn, architectural-decision-record treatment.

**Evidence**: Benchmarks and what they fail to capture. Team-level metrics. 

**Synthesis**: Explicit sorting of what is durable from what is an artifact of
this model generation. Return to W1: what each student now knows about where
their judgment was necessary, from their own improvement log.

**Final demos and postmortems** (Scheduled over the exam period)

---

## 5. Bibliography

Recent research papers will be listed at the beginning of each week.
A discussion forum will be open on Moodle on corresponding topics over the semester.
Participation will count towards the final grade.

### Foundational — mental model of software systems

- Naur, P. (1985). *Programming as Theory Building*. Microprocessing and Microprogramming, 15(5).
- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of
  Software*. Addison-Wesley. (Chapters on ubiquitous language and bounded contexts)
- Chacon, S. & Straub, B. [Pro Git](https://git-scm.com/book/en/v2), 2nd ed.
  Chapter 10, "Git Internals," and `git` plumbing command documentation.

### Lifecycle and requirements

- Jackson, M. (1995). *The World and the Machine*. ICSE '95. 
- Cockburn, A. (2000). *Writing Effective Use Cases*. Addison-Wesley.
- Wiegers, K. & Beatty, J. (2013). *Software Requirements*, 3rd ed. Microsoft Press.
- ISO/IEC/IEEE 29148:2018. *Systems and software engineering — Life cycle
  processes — Requirements engineering*. (Overview sections only.)

### Models and Agent Mechanics

- Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS.
- Alammar, J. *The Illustrated Transformer* 
- Liu, N. F. et al. (2023). *Lost in the Middle: How Language Models Use Long Contexts*. TACL.
- Anthropic (2024). *Building Effective Agents*. Engineering blog.
- Pi coding agent — documentation and source (earendil-works/pi). 
- AGENTS.md specification and the Agentic AI Foundation materials.

### Building with LLMs

- Khattab, O. et al. (2023). *DSPy: Compiling Declarative Language Model Calls
  into Self-Improving Pipelines*. arXiv:2310.03714.
- Zhang, A. L., Kraska, T. & Khattab, O. (2025). *Recursive Language Models*.
  arXiv:2512.24601. Companion blog post: alexzhang13.github.io/blog/2025/rlm/

### Evaluation

- Zheng, L. et al. (2023). *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*. NeurIPS Datasets and Benchmarks.
- Jimenez, C. et al. (2023). *SWE-bench: Can Language Models Resolve Real-World GitHub Issues?* arXiv:2310.06770.
- **ReactBench** (2026) — methodology and results; behavioral tests passing while
  quality defects are introduced. *(verify — citation format pending)*
- **SWE-PRBench** (2026) — AI code review against human-annotated ground truth;
  detection rates far below human expert performance. *(verify — arXiv ID)*

### Spec-driven and requirements-centric development

- Böckeler, B. *Understanding Spec-Driven Development*. martinfowler.com
- Martinelli, S. (2026). *The Three Levels of Spec-Driven Development, and Where
  the AI Unified Process Sits*; and unifiedprocess.ai methodology pages.
- OpenSpec — documentation on delta specs, change folders, and the specs/changes split.
- Spec-Kit — for contrast as a spec-first tool.

### Verification

- Claessen, K. & Hughes, J. (2000). *QuickCheck: A Lightweight Tool for Random
  Testing of Haskell Programs*. ICFP.
- Jia, Y. & Harman, M. (2011). *An Analysis and Survey of the Development of
  Mutation Testing*. IEEE TSE 37(5).
- Chen, T. Y. et al. (2018). *Metamorphic Testing: A Review of Challenges and
  Opportunities*. ACM Computing Surveys.

### Software Engineer as a Profession

- Ng, A. (2026, Aug 28). *The AI Engineering Skills Map In Detail — Software
  Engineering Fundamentals*. The Batch.
- Kam, M. et al. (2025). *What do professional software developers need to know
  to succeed in an age of artificial intelligence?* FSE Companion '25.
- DORA Research Team (2025). *State of AI-Assisted Software Development Report*.
- Geng, F. et al. (2026). *Mapping the Emerging Curriculum for AI-Assisted
  Software Engineering via Syllabus Analysis*. arXiv:2608.05898
- *Configuration Smells in AGENTS.md Files* (2026). *(verify — arXiv ID
  2606.15828)*
- https://themodernsoftware.dev/ course with similar objectives (Stanford fall 2026)


### Trustworthiness framework

- Trustworthy AI: From Principles to Practices**, [Bo Li](https://dl.acm.org/doi/10.1145/3555803# "Bo Li"), [Peng Qi](https://dl.acm.org/doi/10.1145/3555803# "Peng Qi"), [Bo Liu](https://dl.acm.org/doi/10.1145/3555803# "Bo Liu"), [Shuai Di](https://dl.acm.org/doi/10.1145/3555803# "Shuai Di"), [Jingen Liu](https://dl.acm.org/doi/10.1145/3555803# "Jingen Liu"), [Jiquan Pei](https://dl.acm.org/doi/10.1145/3555803# "Jiquan Pei"), [Jinfeng Yi](https://dl.acm.org/doi/10.1145/3555803# "Jinfeng Yi"), [Bowen Zhou](https://dl.acm.org/doi/10.1145/3555803# "Bowen Zhou"), ACM Computing Surveys, Jan 2023, [https://dl.acm.org/doi/10.1145/3555803](https://dl.acm.org/doi/10.1145/3555803) 

---

## Appendix A — Session tags at a glance

| Week | Topic | Thread |
|---|---|---|
| 1 | AI makes writing code cheap; specification and verification are not | T+C |
| 2 | Software lifecycle and requirements analysis: pre-AI baseline | T+C |
| 3 | Understanding LLMs: tokens, decoding, attention, training, post-training | T+C |
| 4 | Domain models as cognitive theory; git worked example | T |
| 5 | LLMs as typed components; retrieval and context assembly | C |
| 6 | Evaluation I | T+C |
| 7 | Evaluation II: judges and agreement | T+C |
| 8 | Agent anatomy | T |
| 9 | Agent-legible systems and workflows | T |
| 10 | Requirements-centric development in the AI era | T+C |
| 11 | Verifiable development; what "good" means | T+C |
| 12 | Continual improvement, evidence, synthesis | T+C |

## Appendix B — Milestone-to-week map

| Milestone | Due | Gates on |
|---|---|---|
| Teams and topic selection | W3 | — |
| M1 — Requirements, domain model, specification | W5 | W2–W4 |
| M2 — AI component and evaluation suite | W8 | W5–W7 |
| M3 — Agent-facing interface and workflow | W11 | W8–W10 |
| M4 — Production system and postmortem | Finals | W11–W12 |

