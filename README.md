<div align="center">
<img src="assets/avs-prompt-library-logo.jpg" width="300" />
</div>

# AVS Prompt Library
Purpose: Library of prompts to feed your LLM through stages of [AVS](https://docs.eigenlayer.xyz/developers/Concepts/avs-developer-guide) idea, design and prototype (code) generation. For these tools, please limit your idea and designs to focus on the scope of **one [Operator Set](https://docs.eigenlayer.xyz/eigenlayer/concepts/operator-sets/operator-sets-concept)** at at time.

## Repo Overview

The `/prompts` folder includes refined prompts to guide the LLM at each stage in content generation.

The `/context` folder includes a collection of content that is helpful to add selective context to the LLM. These assets serve as a simple mechanism to enrich the LLM context window with the right information for a given prompt. These are a simpler, low-tech alternative to integrating an MCP server (which we will probably also build at a later stage).

**Note:** Claude Code currently has a limit of 256KB per file. Each file in the context folder is broken down intentionally to help address this limitation.

## Stage 1 Idea Refinement

This stage takes a high level AVS idea, validates whether it is a good fit to be built as an AVS and outputs the idea as a refined "idea prompt". 

## Stage 2 Design Generation

This stage takes a refined "idea prompt" and generates an AVS Design from it.


## Stage 3 Design Generation

This stage takes a refined "Design Tech Spec" and generates an a functional modified https://github.com/Layr-Labs/hello-world-avs prototype from it.


# Testing

In order to continuously improve the AVS prompts in this repository, we will continue to add new example AVSs in the [test](/test/) folder. We will regenerate these AVSs from Idea to Design to Prototype to measure our improvements of the prompts at each stage.




# Roadmap

- Continuously enhance our default prompts for each stage. The initial prompts are minimally viable and can easily be improved with more time and iteration.
- Add Slashing after it has been launched and fully integrated with eigenlayer-middleware/ECDSAServiceManager and hello-world-avs.


# Appendix

Inspired by [these experiments](https://github.com/wesfloyd/avs-context-prompt?tab=readme-ov-file#eigenlayer-avs-idea-to-prototype-pipeline).
