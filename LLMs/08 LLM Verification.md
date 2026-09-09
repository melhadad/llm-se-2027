---
semester: Fall 2026
course: LLM-SE 2026
tags:
  - LLM
  - Verification
---
### Planning for Verification

When using or developing AI systems, we must take into account the probabilistic nature of AI computations: there are no guarantees that responses are correct or align with our goals. Therefore, verification must be planned from the start:

- Select which tasks should be delegated to AI (make sure they can be verified)
- Specify the prompts and tasks in a way that they can be verified
- Situate the work delegated to AI within existing human workflows that include accountability and verification.


### Measure Performance over Time

AI Systems are dynamic because:

- The underlying foundation models evolve fast (they are re-trained frequently and improved over time)
- The AI system relies on external data which evolves as the system is used (context, episodic memory, RAG reference documents, codebase for coding agents)
- Many AI systems include self-improvement loops as part of execution (DSPy optimizers, frequent fine-tunings on test data collected over time, preference alignment based on RLHF collected over time).

Therefore, it is important to plan for continuous performance verification by:

- Collecting training data over time as the system is used (in our project example, collect students conversations, course documents, quizzes etc).
- Annotating verified performance data over collected data (periodically run tests over the systems output and manually annotate performance).
- Optionally, retrain or optimize the model over collected data to adjust for data drift.
- Put regression tests in place for automation of the periodic validation process.


### Dimensions of AI Trustworthiness

See **Trustworthy AI: From Principles to Practices**, [Bo Li](https://dl.acm.org/doi/10.1145/3555803# "Bo Li"), [Peng Qi](https://dl.acm.org/doi/10.1145/3555803# "Peng Qi"), [Bo Liu](https://dl.acm.org/doi/10.1145/3555803# "Bo Liu"), [Shuai Di](https://dl.acm.org/doi/10.1145/3555803# "Shuai Di"), [Jingen Liu](https://dl.acm.org/doi/10.1145/3555803# "Jingen Liu"), [Jiquan Pei](https://dl.acm.org/doi/10.1145/3555803# "Jiquan Pei"), [Jinfeng Yi](https://dl.acm.org/doi/10.1145/3555803# "Jinfeng Yi"), [Bowen Zhou](https://dl.acm.org/doi/10.1145/3555803# "Bowen Zhou"), ACM Computing Surveys, Jan 2023, [https://dl.acm.org/doi/10.1145/3555803](https://dl.acm.org/doi/10.1145/3555803) 

![[types-of-ai-trustworthiness.jpg]]
Relation between different aspects of AI trustworthiness: performance, security, robustness, generalization, explainability, transparency, accountability and privacy.

Beyond performance, multiple aspects of the AI system behavior contribute to the level of trust users award it:

##### **Robustness**
Ability of the system to deal with execution errors, bad inputs, unseen data (not seen at training time).  A particular aspect of robustness is resisting to adversarial attacks. 

##### Generalization
Capability to distill knowledge from limited training data to make predictions regarding unseen data.  

##### Explainability and Transparency 
Opaque AI systems (made up of multiple large models interacting in complex manner) create a concern that decisions made by the system cannot be validated by human beings. The relation between the proposed decision and the causes of this decision are not transparent. The issue is researched with different perspectives as: **interpretability, explainability, transparency.**  

Transparency refers to the requirement to disclose to users information regarding the entire lifecycle of the system (training data, personal data stored, intervention of humans vs. automated decisions, labeling of generated content as AI generated, traceablility.

Explainability means that the system is designed in such a way that actions and decisions made by the system can be explained to users in a way that reflects the actual decision making process. 

##### Fairness
For systems that operate in medicine, HR, financial risk assessment, face identification, legal decisions, systematic unfairness in decision making can be attributed to the training data (data biases), model biases, or workflow biases. Effort must be put in place to mitigate identified biases. A bias means that depending on protected individual or group attributes (gender, ethnicity, age), the system tends to produce different outcomes. We generally distinguish **fairness of outcome** and **fairness of process** and aim at ensuring both (even though there are mathematical results demonstrating the impossibility to achieve both simultaneously). 

##### Privacy protection
Consists of protecting against unauthorized use of the data that can identify a person. These data include name, age, gender, pictures, biometric data. Regulation (notably GDPR in Europe) impose strong restrictions. Emerging regulations in AI Ethics adapt to the specificity of AI systems. 

##### Accountability
Refers to the regulation on AI systems to follow non-functional requirements as listed above. It requires system designers to justify their design decisions, implementation and operation procedures.  Accountability entails the requirement to produce **auditable** systems (logs of operations must be detailed and allow external regulatory agencies to verify claims about the system).  **Traceability** means that specific decisions made by the system can be traced to concrete code and modules that can be modified as required.

  

### Verification Techniques

##### Consistency Checking
Run the same prompt multiple times through the same model (stochastic responses) or through multiple models (multi-agent consistency).    
Run the prompt multiple times and compare the outcomes. Variants include collecting confidence level in addition to answer. Given the list of outputs (answer, confidence), the system must decide whether the output is reliable, and which one to select.  This can be done with majority voting, weighted vote, decide only if all agree etc.

Variations include generating variability in answers (through decoding parameters - temperature, beam search) and computing intersection across answers.

Important note: experiments show that LLMs tend to produce variable outputs with high correlation in errors - that is, two models often agree on the same errors. That is, the fact that models agree does not always increase confidence in the outcome.

##### Authoritative Verification
Compare answers with trusted sources (encyclopedia, textbooks) or ask LLM to provide attribution for answer and verify linked resource.

##### Validators
In many domains, specific deterministic tools can be used to improve the probability of correctness of an answer.   
Coding examples:  

- No static errors (Linter, Syntax Checking, Type Checking)
- Code runs with no runtime errors.
- All unit tests pass.

Specific examples:

- SQL query validator
- JSON schema validation
- Length validator


##### LLM as a Judge (LLMaaJ)

Collect (prompt, answer) pair from an LLM then pass the pair to another LLM to judge whether the answer is correct or critique it.

Alternatively, as in consistency checking, collect multiple tuple (prompt, answer, confidence) then ask Judge to select best answer or synthesize consensus answer.

See this recent survey: [https://arxiv.org/abs/2411.15594](https://arxiv.org/abs/2411.15594) A survey of LLM-as-a-Judge, Oct 2025.

##### Human in the Middle
System decides to delegate specific steps in workflow to Human Agents.

- Handling complex or ambiguous requests
- Making ethical or value-based decisions (decide that a medical diagnosis applies)
- Steps that require empathy, or effective communication (deal with angry customer)

##### Guardrails

Guardrails are rules and systems that guide LLMs to ensure answers avoid alignment issues (harmful output, risky decisions, biased decisions, irrelevant output).

Guardrails apply both on inputs (provided by users, to detect irrelevant or adversarial attacks, prompt injections) and on the outputs (content moderation, accuracy checks, tone and format adjustments).

See for example [https://www.guardrailsai.com/](https://www.guardrailsai.com/) 

[https://developer.nvidia.com/nemo-guardrails](https://developer.nvidia.com/nemo-guardrails) - Open source systems from nVidia of guardrails.
