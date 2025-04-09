<div align="center">
<img src="assets/avs-prompt-library-logo.jpg" width="300" />
</div>

# AVS Prompt Library
Purpose: Library of prompts to feed your LLM through stages of [AVS](https://docs.eigenlayer.xyz/developers/Concepts/avs-developer-guide) idea, design and prototype (code) generation. For these tools, please limit your idea and designs to focus on the scope of **one [Operator Set](https://docs.eigenlayer.xyz/eigenlayer/concepts/operator-sets/operator-sets-concept)** at at time.

## Repo Overview

The `/prompts` folder includes refined prompts to guide the LLM at each stage in content generation.

The `/context` folder includes a collection of content that is helpful to add selective context to the LLM. These assets serve as a simple mechanism to enrich the LLM context window with the right information for a given prompt. These are a simpler, low-tech alternative to integrating an MCP server (which we will probably also build at a later stage).


## Stage 1 Idea Refinement

This stage takes a high level AVS idea, validates whether it is a good fit to be built as an AVS and outputs the idea as a refined "idea prompt". 

An example refined idea prompt may include the following:
1. Project Overview: what is the name of _your project_ and what value does it provide for its users?
2. AVS Purpose: what benefit does the AVS provide to securing, validating or decentralizing _your project_.
3. Name: what do you want to name your AVS? Or do you want the LLM to name it for you 😉?
4. Operator Work: which core work or task will the Operators that comprise your AVS do?
5. Validation: The work is validated through [todo - insert a description of your validation logic at a high level]
6. Rewards: send Rewards distributions to Operators based on [todo]


## Stage 2 Design Generation

This stage takes a refined "idea prompt" and generates an AVS Design from it.


## Stage 3 Design Generation



# Benchmarks

In order to continuously improve the AVS prompts in this repository, we will continue to add new example AVSs in the [benchmarks](/benchmarks/) folder. We will regenerate these AVSs from Idea to Design to Prototype to measure our improvements of the prompts at each stage.


# Roadmap

- Continuously ennhance our default prompts for each stage. The initial prompts are
- Add Slashing after it has been launched and fully integrated with eigenlayer-middleware/ECDSAServiceManager and hello-world-avs.


# Appendix

Inspired by [these experiments](https://github.com/wesfloyd/avs-context-prompt?tab=readme-ov-file#eigenlayer-avs-idea-to-prototype-pipeline).
