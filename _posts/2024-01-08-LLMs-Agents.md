---
title: Some Notes of LLM Agents
tags: [Notes, Paper Reading,LLMs]
style: fill
color: primary
description: Some notes of LLM Agents (the blog of Lilian Weng)
---
# LLM Powered Autonomous Agents

## Planning

- Task Decomposition

  - by LLM with simple prompting

    - `What are the subgoals for achieving XYZ?`
  - by using task-specific instructions

    - `Write a story outline.`
  - with human inputs
  - Related Works

    - [Chain of Thought](https://arxiv.org/abs/2201.11903)
    - [Tree of Thoughts](https://arxiv.org/abs/2305.10601)
      - extends CoT by exploring multiple reasoning possibilities at each step
        - first decomposes the problem into multiple thought steps
        - generates multiple thoughts per step, creating a tree structure
    - [LLM+P](https://arxiv.org/abs/2304.11477)
      - an external classical planner to do long-horizon planning
        - translates the problem into “Problem [PDDL](https://en.wikipedia.org/wiki/Planning_Domain_Definition_Language)”
        - requests a classical planner to generate a PDDL plan based on an existing “Domain PDDL”
        - translates the PDDL plan back into natural language
- Self-Reflection

  - Related Works
    - [ReAct](https://arxiv.org/abs/2210.03629)
      - prompting LLM to generate reasoning traces in natural language
      - `Thought: ... Action: ... Observation: ... ... (Repeated many times)`
    - [Reflexion](https://arxiv.org/abs/2303.11366)
      - a standard RL setup
        - reward model provides a simple binary reward
        - the action space follows the setup in ReAct
        - After each action, the agent computes a heuristic
          - The heuristic function determines when the trajectory is inefficient
            - trajectories that take too long without success
          - or contains hallucination and should be stopped
            - encountering a sequence of consecutive identical actions that lead to the same observation in the environment.
    - [Chain of Hindsight](https://arxiv.org/abs/2302.02676)
      - explicitly presenting it with a sequence of past outputs, each annotated with feedback
    - [Algorithm Distillation](https://arxiv.org/abs/2210.14215)
      - an *algorithm* is encapsulated in a long history-conditioned policy
        - learn the process of RL instead of training a task-specific policy itself
      - hypothesizes that any algorithm that generates a set of learning histories can be distilled into a neural network by performing behavioral cloning over actions.

## Memory

- Types of Memory
  - Sensory Memory
    - lasts for up to a few seconds
    - iconic memory (visual), echoic memory (auditory), and haptic memory (touch)
    - learning embedding representations for raw inputs
  - Short-Term Memory/Working Memory
    - 7 items and lasts for 20-30 seconds.
    - in-context learning
  - Long-Term Memory
    - Explicit / declarative memory
    - Implicit / procedural memory
    - the external vector
- Maximum Inner Product Search (MIPS)
  - A standard practice
    - save the embedding representation of information into a vector store database
  - To optimize the retrieval speed
    - the *approximate nearest neighbors (ANN)* algorithm to return approximately top k nearest neighbors
      - MIPS algorithms and performance comparison in [ann-benchmarks.com](https://ann-benchmarks.com/).

## Tool Use

- Related Works
  - [MRKL](https://arxiv.org/abs/2205.00445)
    - a collection of “expert” modules and the general-purpose LLM works as a router to route inquiries to the best suitable expert module.
    - *knowing when to and how to use the tools are crucial*
  - [TALM](https://arxiv.org/abs/2205.12255) & [Toolformer](https://arxiv.org/abs/2302.04761)
    - fine-tune a LM to learn to use external tool APIs
  - [HuggingGPT](https://arxiv.org/abs/2303.17580)
    - task planner to select models available in HuggingFace platform
      - according to the model descriptions and summarize the response based on the execution results.

## Challenges

* **Finite context length**
  * Although vector stores and retrieval can provide access to a larger knowledge pool, their representation power is not as powerful as full attention.
* **Challenges in long-term planning and task decomposition**
  * LLMs struggle to adjust plans when faced with unexpected errors
* **Reliability of natural language interface**
  * LLMs may make formatting errors and occasionally exhibit rebellious behavior (e.g. refuse to follow an instruction).

## References

Weng, Lilian. (Jun 2023). LLM-powered Autonomous Agents". Lil’Log. https://lilianweng.github.io/posts/2023-06-23-agent/.
