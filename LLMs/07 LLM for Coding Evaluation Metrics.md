---
tags:
  - LLM
  - Evaluation
  - Metrics
course: LLM-SE 2026
semester: Fall 2026
---
Evaluating Large Language Models (LLMs) for software engineering involves a spectrum of tasks—from code completion to high-level software development activities such as debugging, test generation, code summarization, and full program synthesis from specifications. 

This page provides a short survey outlining task categories, data sources, widely adopted benchmarks, and evaluation metrics. Each section concludes with a table summarizing datasets, metrics, and leading models.

## 1. **Code Completion (Token/Line/Function-Level)**

This task consists of predicting the next token, line, or entire function given a code context. This is the most basic task for Coding LLMs.

### Data Sources  
- Code scraped from public codebases (Open-source repositories such as GitHub)
- Language-specific datasets (e.g., Python, Java, JavaScript)

### Benchmarks & Evaluation Metrics  
- **HumanEval**: Function-level completion from docstring; pass@k metric (functional correctness via unit tests).  
- **MBPP (Mostly Basic Python Problems)**: Short Python programming problems; evaluated with pass@k.  
- **CodeXGLUE**: Includes Code-Code and Code-Text tasks; uses BLEU, CodeBLEU, and execution-based metrics.  
- **APPS**: Competitive programming problems; uses test-case pass rates (pass@k).  
- **LiveCodeBench**: Real-time coding problems from platforms like LeetCode; execution-based evaluation.

| Dataset           | URL                                                                                                                                        | Description                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| **HumanEval**     | [https://github.com/openai/human-eval](https://github.com/openai/human-eval)                                                               | 164 hand-written Python functions with docstrings and unit tests. |
| **MBPP**          | [https://github.com/google-research/google-research/tree/master/mbpp](https://github.com/google-research/google-research/tree/master/mbpp) | 974 basic Python programming problems with test cases.            |
| **APPS**          | [https://github.com/hendrycks/apps](https://github.com/hendrycks/apps)                                                                     | 10,000 competitive programming problems (introductory to expert). |
| **LiveCodeBench** | [https://livecodebench.github.io](https://livecodebench.github.io)                                                                         | Real-time LeetCode-style problems; avoids data contamination.     |
| **CodeXGLUE**     | [https://github.com/microsoft/CodeXGLUE](https://github/p/microsoft/CodeXGLUE)                                                             | Suite including code completion, clone detection, etc.            |

| Dataset                | URL                                                                                | Evaluation Metric(s)                       | Leading Models (2024–2025)                   |
| ---------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------ | -------------------------------------------- |
| HumanEval              | [Link](https://github.com/openai/human-eval)                                       | pass@1, pass@10, pass@100                  | DeepSeek-Coder-33B, GPT-4, Claude 3.5 Sonnet |
| MBPP                   | [Link](https://github.com/google-research/google-research/tree/master/mbpp)        | pass@1, pass@10                            | Code Llama 34B, WizardCoder, GPT-4           |
| APPS                   | [Link](https://github.com/hendrycks/apps)                                          | pass@1, pass@10 (intro/comp/expert splits) | GPT-4, Magicoder, DeepSeek-Coder             |
| LiveCodeBench          | [Link](https://livecodebench.github.io)                                            | pass@1, runtime-correctness                | GPT-4o, Claude 3.5, CodeShell                |
| CodeXGLUE (Completion) | [Link](https://github.com/microsoft/CodeXGLUE/tree/main/Code-Code/code-completion) | Exact match, CodeBLEU                      | StarCoder2, CodeGen, PolyCoder               |

## 2. **Code Translation & Migration**

### Description  
Translating code from one programming language to another (e.g., Python → Java) or upgrading legacy code (e.g., Python 2 → 3).

### Data Sources  
- TransCoder dataset (Facebook AI)  
- CodeNet (IBM)

### Benchmarks & Evaluation Metrics  
- **TransCoder**: Uses token-level accuracy, compilation success rate, and behavioral equivalence via unit tests.  
- **CodeXGLUE (Code-to-Code)**: BLEU, CodeBLEU, exact match, and compilation success.  
- **CodeNet**: Compilation accuracy, functional equivalence.

### Evaluation Metrics
- **Compilation success rate**: % of translated code that compiles.  
- **Reference match**: exact match or AST match with reference.  
- **Functional equivalence**: via input-output testing (if test cases exist).  
- **CodeBLEU**, **BLEU**

| Dataset                      | URL                                                                                                                      | Description                                                         |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **TransCoder**               | [https://github.com/facebookresearch/TransCoder](https://github.com/facebookresearch/TransCoder)                         | Parallel C++, Java, Python functions from GitHub.                   |
| **CodeXGLUE (Code-to-Code)** | [https://github.com/microsoft/CodeXGLUE/tree/main/Code-Code](https://github.com/microsoft/CodeXGLUE/tree/main/Code-Code) | Includes code translation and clone detection.                      |
| **CodeNet**                  | [https://github.com/IBM/Project_CodeNet](https://github.com/IBM/Project_CodeNet)                                         | 14M code submissions in 50+ languages; includes parallel solutions. |

| Dataset                 | URL                                                                                   | Evaluation Metric(s)                       | Leading Models                    |
| ----------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------ | --------------------------------- |
| TransCoder              | [Link](https://github.com/facebookresearch/TransCoder)                                | Compilability, BLEU, BLEURT, code accuracy | TransCoder++, StarCoder2, CodeT5+ |
| CodeXGLUE (Translation) | [Link](https://github.com/microsoft/CodeXGLUE/tree/main/Code-Code/code-to-code-trans) | CodeBLEU, exact match                      | CodeT5+, DeepSeek-Coder           |
| CodeNet                 | [Link](https://github.com/IBM/Project_CodeNet)                                        | Compilation rate, runtime correctness      | GPT-4, CodeShell                  |


## 3. **Code Understanding & Summarization**

### Description  
Generating natural language descriptions of code (code summarization) or answering questions about code behavior.

### Data Sources  
- GitHub repositories with commit messages and comments  
- CodeSearchNet  
- CONCODE (Java methods with NL descriptions)

### Evaluation Metrics
- **BLEU-4**, **ROUGE-L**, **METEOR** (standard NLG metrics)  
- **CodeBLEU** (for code-aware semantics)  
- **MRR (Mean Reciprocal Rank)** for retrieval-based variants

### Datasets & Benchmarks

| Dataset                      | URL                                                                                                                      | Description                                      |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| **CodeSearchNet**            | [https://github.com/github/CodeSearchNet](https://github.com/github/CodeSearchNet)                                       | 2M functions with NL descriptions (6 languages). |
| **CONCODE**                  | [https://github.com/sriniiyer/concode](https://github.com/sriniiyer/concode)                                             | Java methods with structured NL descriptions.    |
| **CodeXGLUE (Code-to-Text)** | [https://github.com/microsoft/CodeXGLUE/tree/main/Code-Text](https://github.com/microsoft/CodeXGLUE/tree/main/Code-Text) | Summarization in Python/Java/JavaScript.         |

| Dataset          | URL                                                                             | Evaluation Metric(s)     | Leading Models            |
| ---------------- | ------------------------------------------------------------------------------- | ------------------------ | ------------------------- |
| CodeSearchNet    | [Link](https://github.com/github/CodeSearchNet)                                 | BLEU, ROUGE, METEOR, MRR | CodeT5+, StarCoder2       |
| CONCODE          | [Link](https://github.com/sriniiyer/concode)                                    | BLEU, ROUGE              | GraphCodeBERT, CodeT5     |
| CodeXGLUE (Text) | [Link](https://github.com/microsoft/CodeXGLUE/tree/main/Code-Text/code-to-text) | BLEU, CodeBLEU           | DeepSeek-Coder, Magicoder |

## 4. **Test Case Generation**

### Description  
Automatically generating unit tests for given code functions or modules.

### Data Sources  
- Open-source projects with test suites (e.g., Defects4J, QuixBugs)  
- Code with existing test cases (e.g., CodeNet, ETH Py150)

### Benchmarks & Evaluation Metrics  
- **Defects4J**: Mutation score, branch coverage, test pass/fail consistency.  
- **TestPilot**: Human evaluation + coverage metrics.  
- **CodeEval (test-gen subset)**: Coverage, pass/fail accuracy, and bug detection rate.

### Evaluation Metrics
- **Line/Branch Coverage**: Measured via coverage tools (e.g., JaCoCo).  
- **Mutation Score**: % of mutants detected.  
- **Test Validity**: % of generated tests that pass on correct code and fail on buggy code.  
- **Human evaluation**: correctness, readability (TestPilot).

### Datasets & Benchmarks

| Dataset                | URL                                                                                                                            | Description                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| **Defects4J**          | [https://github.com/rjust/defects4j](https://github.com/rjust/defects4j)                                                       | 835 real-world Java bugs with test suites.       |
| **TestPilot**          | [https://github.com/ise-uiuc/testpilot](https://github.com/ise-uiuc/testpilot)                                                 | Human-labeled test generation benchmark.         |
| **CodeEval (TestGen)** | [https://github.com/bigcode-project/bigcode-evaluation-harness](https://github.com/bigcode-project/bigcode-evaluation-harness) | Includes test generation subset from HumanEval+. |

| Dataset            | URL                                                                   | Evaluation Metric(s)               | Leading Models            |
| ------------------ | --------------------------------------------------------------------- | ---------------------------------- | ------------------------- |
| Defects4J          | [Link](https://github.com/rjust/defects4j)                            | Branch coverage, mutation score    | GPT-4, TestGen-LLM        |
| TestPilot          | [Link](https://github.com/ise-uiuc/testpilot)                         | Human eval (5-pt Likert), coverage | Claude 3.5, GPT-4o        |
| CodeEval (TestGen) | [Link](https://github.com/bigcode-project/bigcode-evaluation-harness) | Test validity, coverage            | DeepSeek-Coder, Magicoder |


## 5. **Bug Detection & Repair**

### Description  
Identifying bugs in code and/or generating patches to fix them.

### Data Sources  
- Defects4J, QuixBugs, Bugs.jar  
- Code with bug-fixing commits (e.g., GitHub commit histories)

### Evaluation Metrics
- **Plausible patch**: passes all existing test cases.  
- **Correct patch**: also passes external validation (e.g., new tests, human judgment).  
- **Precision/Recall/F1** for bug localization.

### Datasets & Benchmarks

| Dataset          | URL                                                                                      | Description                                     |
| ---------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------- |
| **Defects4J**    | [https://github.com/rjust/defects4j](https://github.com/rjust/defects4j)                 | Widely used for APR (Automated Program Repair). |
| **QuixBugs**     | [https://github.com/jkoppel/QuixBugs](https://github.com/jkoppel/QuixBugs)               | 40 bugs in Python/Java with test cases.         |
| **ManySStuBs4J** | [https://github.com/mast-group/ManySStuBs4J](https://github.com/mast-group/ManySStuBs4J) | Single-statement Java bugs from GitHub.         |

| Dataset      | URL                                                | Evaluation Metric(s)            | Leading Models               |
| ------------ | -------------------------------------------------- | ------------------------------- | ---------------------------- |
| Defects4J    | [Link](https://github.com/rjust/defects4j)         | Plausible/Correct patch %       | GPT-4 + agent, Coder-Planner |
| QuixBugs     | [Link](https://github.com/jkoppel/QuixBugs)        | Exact patch match, test success | AlphaCode 2, DeepSeek-Coder  |
| ManySStuBs4J | [Link](https://github.com/mast-group/ManySStuBs4J) | F1 for bug localization         | CodeBERT, GraphCodeBERT      |

---

### Table: Bug Detection & Repair

| Dataset        | Evaluation Metric(s)                        | Leading Models                     |
|----------------|---------------------------------------------|------------------------------------|
| Defects4J      | Plausible/Correct patches, test pass rate   | GPT-4, Coder-Planner, CodeT5+      |
| QuixBugs       | Exact match, test success                   | DeepSeek-Coder, AlphaCode 2        |
| ManySStuBs4J   | Precision, Recall, F1                       | CodeBERT, GraphCodeBERT            |

---

## 6. **Repository-Level Tasks (Multi-File / Whole-Project)**

### Description  
Tasks that require understanding or generating code across multiple files, e.g., adding a feature, refactoring, or integrating APIs.

### Data Sources  
- GitHub repositories  
- RepoBench, SWE-bench, DevBench  
- CodeNet (multi-file subsets)

### Benchmarks & Evaluation Metrics  
- **SWE-bench**: Resolved issues % (based on matching gold patches in real GitHub issues).  
- **RepoBench**: File-edit accuracy, dependency consistency, build success.  
- **CrossCodeEval**: Build success, test pass, functional correctness across files.

### Evaluation Metrics
- **Issue resolution rate**: % of issues where patch matches dev solution.  
- **Build success**: `make` or `pip install` succeeds.  
- **Edit consistency**: no broken imports or references.

### Datasets & Benchmarks

| Dataset           | URL                                                                | Description                                          |
| ----------------- | ------------------------------------------------------------------ | ---------------------------------------------------- |
| **SWE-bench**     | [https://www.swebench.com](https://www.swebench.com)               | 2,294 real GitHub issues with environment snapshots. |
| **RepoBench**     | [https://repobench.github.io](https://repobench.github.io)         | Multi-file editing with dependency awareness.        |
| **CrossCodeEval** | [https://crosscodeeval.github.io](https://crosscodeeval.github.io) | Multi-file program synthesis & editing.              |

| Dataset       | URL                                     | Evaluation Metric(s)                 | Leading Models                                 |
| ------------- | --------------------------------------- | ------------------------------------ | ---------------------------------------------- |
| SWE-bench     | [Link](https://www.swebench.com)        | Issue resolution %                   | Devin (Cognition), GPT-4 + SWE-agent, OpenHand |
| RepoBench     | [Link](https://repobench.github.io)     | Build success, reference consistency | Claude 3.5 + tools                             |
| CrossCodeEval | [Link](https://crosscodeeval.github.io) | Multi-file test pass, build success  | GPT-4o, DeepSeek-Coder + RAG                   |

## 7. **Program Synthesis from Specification**

### Description  
Generating full programs from high-level natural language or formal specifications.

### Data Sources  
- Competitive programming (APPS, CodeContests)  
- Text-to-SQL (Spider, BIRD)  
- Domain-specific synthesis (e.g., robotics, data science)

### Evaluation Metrics
- **Execution accuracy** (Spider, BIRD, DS-1000)  
- **Test-case pass rate** (APPS, CodeContests)  
- **Exact match** (less common due to multiple valid solutions)

### Datasets & Benchmarks

| Dataset          | URL                                                                                    | Description                               |
| ---------------- | -------------------------------------------------------------------------------------- | ----------------------------------------- |
| **APPS**         | [https://github.com/hendrycks/apps](https://github.com/hendrycks/apps)                 | Competitive programming problems.         |
| **CodeContests** | [https://github.com/deepmind/code_contests](https://github.com/deepmind/code_contests) | 13k problems from Codeforces, etc.        |
| **Spider**       | [https://yale-lily.github.io/spider](https://yale-lily.github.io/spider)               | Text-to-SQL with complex queries.         |
| **BIRD**         | [https://bird-bench.github.io](https://bird-bench.github.io)                           | Realistic SQL with database environments. |
| **DS-1000**      | [https://ds1000-code-gen.github.io](https://ds1000-code-gen.github.io)                 | Data science code generation in Python.   |
### Table: Program Synthesis

| Dataset      | URL                                               | Evaluation Metric(s)                 | Leading Models         |
| ------------ | ------------------------------------------------- | ------------------------------------ | ---------------------- |
| APPS         | [Link](https://github.com/hendrycks/apps)         | pass@k                               | GPT-4, AlphaCode 2     |
| CodeContests | [Link](https://github.com/deepmind/code_contests) | Hidden test pass rate                | GPT-4o, DeepSeek-Coder |
| Spider 1.0   | [Link](https://yale-lily.github.io/spider)        | Execution accuracy                   | SQLCoder, GPT-4        |
| BIRD         | [Link](https://bird-bench.github.io)              | Execution accuracy, efficiency score | GPT-4, Claude 3.5      |
| DS-1000      | [Link](https://ds1000-code-gen.github.io)         | Output match, execution success      | GPT-4, Code Llama 34B  |

## Summary

The evaluation landscape for LLMs in software engineering is rapidly evolving from isolated, file-level tasks toward holistic, repository-aware development. While early benchmarks focused on syntactic and functional correctness in narrow contexts (e.g., HumanEval), recent efforts (e.g., SWE-bench, RepoBench) demand real-world engineering capabilities—build systems, debugging across files, and tool integration.

**Key Trends (2024–2025):**  
- Shift from pass@k to **realistic developer workflows** (tool-augmented agents).  
- Emphasis on **multi-file consistency**, **buildability**, and **test coverage**.  
- Rise of **execution-based evaluation** over lexical similarity metrics.  
- Leading models increasingly combine **strong base LLMs** (e.g., GPT-4, Claude 3.5) with **tool use**, **RAG**, and **agent frameworks**.


## **8. Evaluation Metrics: Definitions and Formulas**

In this section, we summarize core evaluation metrics used across tasks.

### **8.1. pass@k (Pass at $k$)**
Measures the probability that **at least one** of **$k$** independently generated samples passes all unit tests.

**Formula**:  
  Given $n$ total samples $n \geq k$, and $c$ correct samples (that pass all tests):  
  
  $\text{pass}@k = 1 - \frac{\binom{n - c}{k}}{\binom{n}{k}}$
  
  In practice, often approximated via Monte Carlo: generate $n$ samples, randomly select $k$ without replacement many times, and estimate the success rate.

**Used in**: HumanEval, MBPP, APPS, LiveCodeBench.

> **Note**: $pass@1$ is equivalent to accuracy when only one sample is generated.

```python
def pass_at_k(n, c, k): 
	""" 
	:param n: total number of samples 
	:param c: number of correct samples 
	:param k: k in pass@$k$ 
	""" 
	if n - c < k: 
		return 1.0 
	return 1.0 - np.prod(1.0 - k / np.arange(n - c + 1, n + 1)) 
	
# Figure 3. A numerically stable script for calculating an unbiased estimate of pass@k.

```

### **8.2. CodeBLEU**
Extension of BLEU that incorporates **syntactic and semantic** structures via abstract syntax trees (ASTs) and data-flow matches.

**Formula**:  
  $\text{CodeBLEU} = \alpha \cdot \text{BLEU} + \beta \cdot \text{Syntax-Match} + \gamma \cdot \text{Data-Flow-Match} + \delta \cdot \text{AST-Match}$
  where $\alpha + \beta + \gamma + \delta = 1$ \)$. Default weights: $(0.25, 0.25, 0.25, 0.25)$.

**Used in**: Code summarization, translation, generation.

See details in https://github.com/microsoft/CodeXGLUE/blob/main/Code-Code/code-to-code-trans/CodeBLEU.MD 

### **8.3. BLEU / ROUGE / METEOR**
- **BLEU**: Precision-based n-gram overlap between candidate and reference.  
- **ROUGE**: Recall-based (e.g., ROUGE-L uses longest common subsequence).  
- **METEOR**: Harmonic mean of precision/recall with synonymy and stemming.

### **8.4. Execution Accuracy**
Fraction of generated programs that **compile and produce correct output** on given test inputs.

**Formula**:  
$\text{Exec Acc} = \frac{1}{N} \sum_{i=1}^{N} \mathbb{1}\left[ \text{exec}(y_i) = \text{exec}(y^*_i) \right]$

where $y_i$ is generated code, $y^*_i$ is reference, and $\text{exec}$ denotes program execution.

### **8.5. Mutation Score / Coverage**
**Line/Branch Coverage**: % of source code lines/branches executed by test suite.  
**Mutation Score**: % of artificial mutants (e.g., operator changes) killed (i.e., cause test failure).

### **8.6. Issue Resolution Rate (SWE-bench)**
Binary success if the model’s patch **exactly matches** or is **functionally equivalent** to a developer-provided fix for a GitHub issue.

- Evaluated via automated test replay and patch validation.


## 9. Examples from Datasets

### 9.1 HumanEval

```python
def longest(strings: List[str]) -> Optional[str]: 
	""" 
	Out of list of strings, return the longest one. 
	Return the first one in case of multiple strings of the same length. 
	Return None in case the input list is empty. 
	>>> longest([]) 
	
	>>> longest([’a’, ’b’, ’c’]) 
	’a’ 
	>>> longest([’a’, ’bb’, ’ccc’]) 
	’ccc’ 
	""" 
	### COMPLETION 1 (CORRECT): ### 
	return max(strings, key=len) if strings else None 
	
	### COMPLETION 2 (WRONG): ### 
	if not strings: return None  
	strings = sorted(strings, key=len) 
	return strings[-1] 
	
	### COMPLETION 3 (WRONG): ### 
	longest_string = strings[0] 
	# if len(strings) > 1: 
	# # longest_string = ’’ 
	# longest_string = max(strings, key=lambda a: len(a)) 
	# else: 
	# return strings[0] 
	for s in strings: 
		if len(s) > len(longest_string): 
			longest_string = s 
	return longest_string 
	
	### COMPLETION 4 (WRONG): ### 
	if not strings: 
		return None 
	else: 
		return sorted(strings)[-1] 
		
	### COMPLETION 5 (CORRECT): ### 
	lengths = [len(string) for string in strings] 
	if len(lengths) == 0: 
		return None 
	max_length = max(lengths) 
	max_indices = [i for i, v in enumerate(lengths) if v == max_length] 
	if len(max_indices) == 1: 
		return strings[max_indices[0]] 
	else: 
		return strings[max_indices[0]] 
		
	### COMPLETION 6 (CORRECT): ### 
	if not strings: 
		return None 
	return max(strings, key=len) 
	
	### COMPLETION 7 (WRONG): ###
	# CODE GOES HERE 
	lens = [len(s) for s in strings] 
	i = max(lens) 
	return strings[lens.index(i)] if lens else None 
	
	### COMPLETION 8 (WRONG): ### 
	if strings is None: 
		return None 
	longest_so_far = strings[0] 
	for s in strings: 
		if len(s) > len(longest_so_far): 
			longest_so_far = s 
	return longest_so_far
```

### 9.2. Example from APPS

**Problem** 
You are given a string $s = s_1s_2 \dots s_n$ of length $n$, which only contains digits $1, 2,..., 9$. A substring $s[l \dots r]$ of $s$ is a string $s_ls_{l+1}s_{l+2} \dots s_r$. 
A substring $s[l \dots r]$ of $s$ is called "even" if the number represented by it is even. Find the number of even substrings of $s$. 
Note, that even if some substrings are equal as strings, but have different $l$ and $r$, they are counted as different substrings. 
The first line contains an integer $n$  $(1 ≤ n ≤ 65000)$ — the length of the string $s$. The second line contains a string $s$ of length $n$. 
The string $s$ consists only of digits $1, 2,..., 9$. 
Print the number of even substrings of $s$. 
```python
n = int(input()) 
s = input() 
ans = 0 
for i in range(n): 
	for j in range(i, n): 
		if int(s[i:j+1]) % 2 == 0: 
			ans += 1 
print(ans)
```

