# FIGS: Feline Image Generation Service

**Note*: this benchmark is meant to be a counter-example testing a bad idea to ensure the LLM gives helpful feedback and coaching to the user.

## Example Refined Idea Prompt

Help me generate a refined idea prompt for my AVS idea using [prompts/stage1-idea-refinement-prompt.md] for guidance. 

I want to build an AVS that helps build Bats for Football players.



## Output from Claude Code (PASSING)
[Internal note: the output from Claude Code below is considered "PASSING" b/c it identified for the user the idea was Not good for an AVS]

  Your idea about "building Bats for Football players" needs more clarity to determine if it can work as an AVS. An AVS requires:

  1. Clear validation logic
  2. Operator work that can be verified
  3. A decentralized component that benefits from trust minimization

  Let me help refine your idea into an AVS concept:

  Refined AVS Idea: BatVerify

  1. Project Overview: BatVerify is a decentralized quality verification system for sports equipment manufacturing, starting with
  baseball bats used by football players for training exercises.
  2. AVS Purpose: BatVerify ensures the quality, authenticity, and safety standards of specialized training equipment through
  decentralized verification, preventing counterfeit or substandard equipment from reaching professional athletes.
  3. Name: BatVerify
  4. Operator Work: Operators will run verification nodes that process images and sensor data of bats uploaded by manufacturers or
  quality inspectors. Each operator runs analysis algorithms to verify dimensions, material composition, structural integrity, and
  compliance with safety standards.
  5. Validation: The work is validated through consensus among multiple operators who independently analyze the same equipment
  data. When a threshold of operators (e.g., 2/3) agree on the quality assessment, the bat receives a cryptographic certificate of
  authenticity on-chain.
  6. Rewards: Reward distributions to Operators based on:
    - Accuracy of assessments (compared to consensus)
    - Volume of verifications processed
    - Detection of fraudulent equipment (confirmed by multiple operators)

  Is this refinement aligned with what you were thinking for your AVS idea? Would you like to adjust any aspects of this
  refinement?