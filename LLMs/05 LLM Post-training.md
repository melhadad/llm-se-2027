---
tags:
  - LLM
  - Posttraining
course: LLM-SE 2026
semester: Fall 2026
---

## LLM Post-training

The Language Modeling objectives which we have presented as *pre-training* are complemented with additional stages of training which aim for different goals and use different training sources.

Training a Large Language Model (LLM) thus involves a progression through several distinct stages, **building upon the capabilities acquired in the previous phase** to produce a model that is helpful, harmless, and effective in following instructions and performing complex reasoning. 

The successive stages of training current LLMs (2025) are:

1. **Pre-training** (Next-Token Prediction)
- **Objective:** To provide the model with a broad linguistic foundation and general world knowledge. The initial, randomly initialized model learns basic language structure, grammar, and facts by being exposed to a vast amount of unlabeled text data from the internet, books, and other sources.
- **Method:** The primary method is self-supervised learning, specifically the next-token prediction objective. The model is trained to predict the next word or token in a sequence given the preceding text. 
  

2. **Instruction Fine-Tuning (SFT)**
- **Objective:** To teach the model how to follow instructions and interact in a conversational manner. The model learns compliance with format and style guidelines, bridging the gap between a next-word predictor and a helpful assistant.
- **Method:** The pre-trained model is further trained on a curated dataset of high-quality, human-crafted examples consisting of (instruction, ideal output) pairs in a supervised fashion. 

3. **Alignment (Reinforcement Learning from Human Feedback - RLHF)**
- **Objective:** To align the model's output with human preferences and ethical standards, making it more helpful, honest, and less likely to generate biased or harmful content. This process makes the model safe and contextually appropriate for real-world applications.
- **Method:** This stage involves two main steps:
    - **Reward Model (RM) Training:** Human labelers rank the quality of different model responses to the same prompt. A separate reward model is trained to predict these human preferences.
    - **RL Fine-Tuning:** The main language model is optimized using a policy gradient algorithm (like PPO) to maximize the reward model's output. This iterative loop helps the LLM generate responses that humans find preferable. 

4. **Reasoning RL (Reinforced Reasoning)**
- **Objective:** To enhance the model's capability for complex, multi-step logical thinking. This phase is focused on improving the model's ability to generate coherent, multi-step reasoning processes, often using process-based rewards (e.g., chain-of-thought reasoning) rather than just outcome-based evaluations.  
  Particularly, Reinforcement Learning (RL) or post-training is necessary to make complex prompting strategies effective such as CoT, Tools Usage or PoT. (See [[06 Prompting Strategies]])
- **Method:** Advanced RL methods are applied to guide the model toward better performance on reasoning benchmarks, serving as a pivotal force for advancing model reasoning capabilities.


![[Pasted image 20251109084811.png]]
(From https://blog.dailydoseofds.com/p/4-stages-of-training-llms-from-scratch, Jul 2025)



