### What is a Prompt

1. A prompt is an input submitted to an LLM in order to generate an expected output.
	- We have seen previously that **Instruction Fine-tuned LLMs** can generalize and learn how to answer new tasks when the input includes a description of the task to be performed.
	- For example:
	   ```
	   Translate this sentence from English to French:
	   
	   My tailor is rich.
	   
	   ==>
	   
	   The French translation for "My tailor is rich" is:
	   
	   Mon tailleur est riche.
	   ```
	- Change the instruction, the LLM adopts its behavior:
	   ```
	   Translate this sentence from English to Hebrew:
	   
	   My tailor is rich.
	   
	   ==>
	   
	   The Hebrew translation for "My tailor is rich" is:
		החייט שלי עשיר.
		(Transliteration: _Ha-chayát shelí ashír._)

	   ```
	   - Note how the exact structure of the output is different in the two tasks.
	     
2. It has been shown in many experiments that the phrasing of the prompt (the way we ask the LLM to perform a task) impacts highly on the output of the LLM (and whether the outpout can be interpreted as a "correct" answer to the prompt):
   
   #### Quantifying Language Models' Sensitivity to Spurious Features in Prompt Design or: How I learned to start worrying about prompt formatting
   https://arxiv.org/abs/2310.11324
   Jul 2024
   [Melanie Sclar](https://arxiv.org/search/cs?searchtype=author&query=Sclar,+M), [Yejin Choi](https://arxiv.org/search/cs?searchtype=author&query=Choi,+Y), [Yulia Tsvetkov](https://arxiv.org/search/cs?searchtype=author&query=Tsvetkov,+Y), [Alane Suhr](https://arxiv.org/search/cs?searchtype=author&query=Suhr,+A)
   > As large language models (LLMs) are adopted as a fundamental component of language technologies, it is crucial to accurately characterize their performance. Because choices in prompt design can strongly influence model behavior, this design process is critical in effectively using any modern pre-trained generative language model. In this work, we focus on LLM sensitivity to a quintessential class of meaning-preserving design choices: prompt formatting. We find that several widely used open-source LLMs are extremely sensitive to subtle changes in prompt formatting in few-shot settings, with performance differences of up to 76 accuracy points when evaluated using LLaMA-2-13B. Sensitivity remains even when increasing model size, the number of few-shot examples, or performing instruction tuning. Our analysis suggests that work evaluating LLMs with prompting-based methods would benefit from reporting a range of performance across plausible prompt formats, instead of the currently-standard practice of reporting performance on a single format. We also show that format performance only weakly correlates between models, which puts into question the methodological validity of comparing models with an arbitrarily chosen, fixed prompt format. To facilitate systematic analysis we propose FormatSpread, an algorithm that rapidly evaluates a sampled set of plausible prompt formats for a given task, and reports the interval of expected performance without accessing model weights. Furthermore, we present a suite of analyses that characterize the nature of this sensitivity, including exploring the influence of particular atomic perturbations and the internal representation of particular formats.


### What Should be in a Prompt

A prompt includes different components, each with its importance on how to drive the LLM towards the expected answer:

At the minimum, the following two components must appear:
1. **Instruction**: describes what is the task we want to execute. (In the example above "Translate this sentence from English to French").
2. **Input**: the data on which the task should be executed. (in the example above, the sentence "My tailor is rich"). 

Experimental studies have established that the following additional components improve the quality of prompts:
1. **Persona**: describe "who is asking the question", which determines the level of expertise you expect in the answer, the style of the answer.  For example, "*I am an experienced software developer with expertise in Python*." will lead to different answers than "*I am a student in an intro to CS course learning Python*" even if the rest of the question is the same.
2. **Output format**: provide explicit details on how the answer should be formatted, what fields it should or should not include.  For example, we observe in the examples above that when asked to translate from English to Hebrew, the LLM provided a repetition of the input, the translated sentence and the transliteration of the translation.  If we are interested in these fields, it helps to specify a format that would help a program to extract them (for example, *answer with the translated sentence in Hebrew and the transliteration of the translation formatted as a JSON output* ).  

It is important to specify the instruction in a **detailed, specific and unambiguous** manner in order to obtain consistent and quality answers. 

For example, using a persona and an output format specification, the translation example leads to the following output:

```
I am a professional translator with expertise in Hebrew linguistics.
Translate this sentence from English to Hebrew:
	   
	   My tailor is rich.

Return only the translation.  
Generate multiple versions of the translation (at least three).
Format the list of translations in a JSON format.

==>

{
  "translations": [
    {
      "version": 1,
      "translation": "החייט שלי עשיר."
    },
    {
      "version": 2,
      "translation": "החייט שלי הוא עשיר."
    },
    {
      "version": 3,
      "translation": "התופר שלי עשיר."
    }
  ]
}
```


### System and User Prompts

Many LLMs have been trained on structured input specifications for multi-turn chat conversations. In this setting, a whole conversation is organized as a set of *general instructions* followed by a sequence of pairs *user input / assistant answer*.  

By convention, many LLMs call the *general instructions* part the **system prompt**. It is typically used to specify aspects of the prompt that should remain constant for the whole conversation, for example, the expected tone, persona, response style, response format.

See for example: https://platform.openai.com/chat/edit?models=gpt-4.1 

### Conversation History Structure

When a user has a conversation with an LLM with multiple turns, the whole history of the conversation is actually submitted to the LLM at each turn.  The exact structure is different for each provider, but conceptually it is similar to the following structure:

```
{
  "contents": [
    {
      "role": "user",
      "parts": [
        {
          "text": "Hello, I'm planning a trip. Can you suggest three countries in Europe with a rich history?"
        }
      ]
    },
    {
      "role": "model",
      "parts": [
        {
          "text": "Certainly! Three European countries with a rich history are **Italy**, **Greece**, and **France**. They all offer incredible ancient, medieval, and modern historical sites."
        }
      ]
    },
    {
      "role": "user",
      "parts": [
        {
          "text": "Which of those has the best food?"
        }
      ]
    }
    // The next turn (model's response) would be generated by Gemini based on this context
  ],
  "config": {
    // Other configuration parameters
    "systemInstruction": "You are a helpful travel assistant.",
    "temp": 0.7,
    "top-p": 1.0,
    "max-tokens": 1024
  }
}
```

Remember that the LLM server operates in a **stateless** manner - each time a new turn is generated, the whole conversation history is resubmitted.

