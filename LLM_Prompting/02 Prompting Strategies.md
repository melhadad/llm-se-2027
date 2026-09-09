## Prompting Strategies

When describing a task to an LLM, strategies indicate how the user can drive the LLM towards a good answer.  Different strategies have been empirically identified and lead to positive outcomes. In turn, LLMs can be trained and fine-tuned to operate well according to specific strategies.  

These strategies combine two dimensions to better control the intended output:
- Make prompts more complex, by adding examples, breaking down the instructions into multiple steps, explaining how the output should be broken down into multiple steps.
- Actually building multi-turn strategies, where the LLM is prompted multiple times, the answers are analyzed and then further turns are planned. 

### Zero-shot, One-shot, Many-shot (In-Context Learning)

In-Context Learning (ICL) refers to the LLM's ability to learn from examples provided directly within the **prompt** text, without requiring any gradient updates (i.e., fine-tuning). 

| **Strategy**  | **Description**                                                                      | **Key Feature**                                                                          | **Example Prompt Structure**                                            |
| ------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Zero-shot** | The prompt includes only the instruction and the task input, **with no examples**.   | Relies solely on the LLM's pre-trained knowledge base (aka *parametric memory*)          | `Translate "Hello world" into French.`                                  |
| **One-shot**  | The prompt includes **one example** of the input/output pair before the final query. | Provides a single, strong template for the desired output format/style.                  | `<Example Input> -> <Example Output>... <New Input> -> ?`               |
| **Many-shot** | The prompt includes **multiple examples** (e.g., 3 to 10) of input/output pairs.     | Strengthens the ICL signal, improving performance for complex tasks or specific formats. | Repeated sequences of `Input: X / Output: Y` followed by the new query. |

ICL is hypothesized to work by implicitly activating the model's existing knowledge and aligning its weights to the functional relationship demonstrated by the examples.

### Chain of Thought (CoT) 

**Chain of Thought (CoT)** is a strategy where the user instructs the LLM to **show its reasoning steps** before giving the final answer. This transforms a single-step problem into a multi-step reasoning process, mimicking human problem-solving.

- **Mechanism:** It works by prompting the LLM to generate an **intermediate reasoning path** (the "chain of thought") that leads to the final output. This significantly improves performance on complex reasoning tasks, such as arithmetic, commonsense, and symbolic reasoning.
    
- **Zero-shot CoT:** Simply adding the phrase **"Let's think step by step."** to the end of a zero-shot prompt can trigger the CoT capability in sufficiently large LLMs, often matching or exceeding few-shot performance.
    
- **Few-shot CoT:** The examples in a few-shot prompt explicitly include the step-by-step reasoning that led to their correct answer. This is more reliable for smaller or less-capable models.
    

> **Example (Zero-shot CoT Prompt Snippet):**
> 
> `The store had 23 apples. It sold 15 apples and bought 5 more. How many apples does the store have now? Let's think step by step.`


### Program of Thought (PoT) 

**Program of Thought (PoT)** is an advanced reasoning strategy that extends CoT by leveraging the LLM's capability to generate **executable code** (e.g., Python) as its intermediate thought process.

- **Mechanism:** Instead of generating purely natural language steps, the LLM generates a program that can be executed by an external interpreter. The result of the program execution is then used by the LLM to formulate the final answer.
    
- **Advantage:** This completely **eliminates hallucination** and calculation errors for steps that can be formalized as code, such as complex arithmetic, data manipulation, or logical checks.
    
- **Workflow:**
    
    1. **Prompt:** Input task. Instruction: "Output a program to solve this."
        
    2. **LLM Output:** Generates Python code.
        
    3. **Execution:** External interpreter runs the code and returns the result (output).
        
    4. **Final Prompt (optional):** LLM processes the code output and presents the final answer in natural language.
        


### ReAct (Reasoning and Acting) 

**ReAct** is a general-purpose prompt strategy that integrates **Reasoning** (the internal thought process) and **Acting** (interacting with external tools) in an interleaved manner. It is a powerful framework for tasks that require dynamic knowledge gathering and complex planning.

- **Goal:** To enable LLMs to solve tasks that are impossible with internal knowledge alone by utilizing search engines, calculators, APIs, or other tools.
    
- **Format:** The LLM's output is structured in a loop, alternating between three phases:
    
    1. **Thought:** The LLM internally reasons about the current state, the goal, and the next step (similar to CoT).
        
    2. **Action:** The LLM specifies an external tool to use and the input arguments for that tool (e.g., `Search[query]`, `Lookup[term]`).
        
    3. **Observation:** The result returned by the external tool is fed back into the prompt for the next turn.
        

> **ReAct Workflow Snippet:**
> 
> Thought: I need to find the population of Tokyo before I can compare it to the population of London.
> 
> Action: Search[population of Tokyo]
> 
> Observation: Tokyo's population is 13.96 million.
> 
> Thought: Now I will search for the population of London.
> 
> Action: Search[population of London]
> 
> Observation: ...


### Tools Invocation (Tool-Use or Function Calling) 

**Tools Invocation** is the mechanism that implements the **Action** step in frameworks like ReAct, often exposed via modern LLM APIs (e.g., Google's Gemini, OpenAI's GPT).

- **Concept:** Instead of just generating text, the LLM is given **descriptions of callable functions** (tools) and their parameters. The LLM's task is to decide _if_ a tool is necessary, _which_ tool to use, and _what_ arguments to pass to it.
    
- **API Interaction:** The prompt includes a structured schema (e.g., JSON/YAML) defining the available tools. The LLM, instead of completing the text, generates a **structured function call object** (e.g., `{"function": "get_weather", "arguments": {"city": "Boston"}}`).
    
- **Benefits:** This approach separates the **planning** (done by the LLM) from the **execution** (done by the host system/developer), creating robust and reliable agentic systems.
    

> **Example Tool Definition (Simplified):**
> 
> JSON
> 
> ```
> {
>   "name": "get_stock_price",
>   "description": "Retrieves the current stock price for a given ticker symbol.",
>   "parameters": {
>     "type": "object",
>     "properties": {
>       "symbol": {"type": "string", "description": "The stock ticker symbol (e.g., GOOG, AAPL)"}
>     }
>   }
> }
> ```

_The LLM would then generate the function call based on the user's query._

### Best of N (Sampling and Reranking) 

The **Best of N** strategy (sometimes called _sampling and reranking_) exploits the **non-deterministic nature** of LLMs to generate a set of candidate outputs, from which the most optimal result is selected. This **generate and test** approach can improve the final quality and reliability of the output, especially for creative, complex, or code generation tasks, when a tool can be used to assess the relative quality of candidate outputs.

#### The Process

1. **Sampling (Generation):**
    
    - **Variability Control:** The core of this step involves controlling the LLM's **temperature** hyperparameter.
        
        - A **higher temperature** (e.g., $T > 0.7$) increases the randomness, encouraging the LLM to explore a wider range of high-probability tokens. This is used to generate $N$ _diverse_ candidates.
            
        - Alternatively, you can reuse the same prompt across **multiple distinct LLMs** (e.g., one optimized for reasoning, one for creativity) to gather a diverse set of results.
            
    - The LLM generates $N$ independent outputs (e.g., $N=10$) for the same prompt.
        
2. **Reranking (Selection):**
    
    - A method is used to evaluate and rank the $N$ candidates to select the "best" one. Common reranking approaches include:
        
        - **Heuristic-based:** Using simple metrics like length, presence of key terms, or execution success (for code).
            
        - **LLM-as-a-Judge (Self-Correction):** The most effective method. A second, often zero-temperature ($T=0$) prompt is sent to the **same or a different, more powerful LLM** (the judge). This prompt asks the judge to evaluate the $N$ candidates against the original constraints and select the best one.
            
        - **External Evaluation:** For tasks like code, running test cases against the $N$ code snippets and choosing the one that passes the most tests.
            

#### Reliance on Multiple LLMs or Same LLM with Variability

| **Strategy**      | **LLM Usage**                                                                 | **Variability Control**                                                                                                                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Same LLM**      | One model used for all $N$ generations and often for the judging step.        | **Temperature ($T > 0$):** Used during the $N$ generations to introduce randomness and diversity. The judge LLM is typically run at $T=0$ for deterministic selection.                                                        |
| **Multiple LLMs** | Different models (e.g., _Model A_, _Model B_, _Model C_) generate candidates. | **Model Diversity:** Variability comes from the distinct architectures, training data, and emergent capabilities of different models. This is often combined with a _Judge LLM_ (perhaps the most capable one) for reranking. |


### Consistency Checking (Self-Consistency) 

**Consistency Checking** is a robust strategy designed to improve the accuracy of reasoning and mathematical tasks, often implemented via the **Self-Consistency** method. It is a special case of Best of N where the selection criterion is based on the **consensus** among the generated reasoning paths.

#### The Process

1. **Diverse CoT Generation:** The LLM is prompted with a **Chain of Thought (CoT)** prompt $N$ times, using a **higher temperature ($T > 0$)** to encourage the generation of $N$ **diverse reasoning paths** that lead to potentially different final answers.
    
2. **Consensus Voting:** The final answer from each of the $N$ generated CoT paths is extracted.
    
3. **Majority Selection:** The final answer is chosen based on the **majority vote** across the $N$ samples. If 7 out of 10 samples arrive at the same answer, that answer is selected as the final, most _consistent_ output.
    

#### Rationale

The core assumption is that a complex problem often has only one correct answer, and while the reasoning path may vary, the **correct path is statistically more likely to be generated consistently** than incorrect paths. By aggregating the answers, the method leverages the wisdom of the 'crowd' of generated thoughts to mitigate errors in a single, potentially flawed, reasoning chain.
