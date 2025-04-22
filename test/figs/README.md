
# Stage 1: Idea Refinement

## Example Prompt


Help me generate a refined idea prompt for my AVS idea using [prompts/stage1-idea-refinement-prompt.md] for guidance. 

Operator Work: Design an AVS where a single Operator generates cat images via LLM inference.
Validation: The work is validated using at least two other operators set of LLMs to measure their accuracy. The validating operators should assign a percentage "accuracy rating" between 0% and 100%.
Rewards: validating operators get rewarded if they respond with any accuracy rating. Mark the Operator that generated the original image for a reward if the aversage validator accuracy was greater than 90%


## Output from Claude Code (PASSING)
[Internal note: the output from Claude Code below is considered "PASSING" b/c it enhanced my basic idea prompt]

See: test/figs/figs-refined-idea-prompt.md


# Stage 2: Design Tech Spec Generation

## Example Prompt

Help me generate a design tech spec for my AVS using the previously generated cat-image-validator-refined-idea-prompt.md and prompts/stage2-design-generation-prompt.md for guidance. 

## Output from Claude Code (PASSING)

Note: this took about 5 mins for Claude Code to execute. It was not quick, but the results were good.


# Stage 3: Prototype Generation


## Example Prompt

Help me generate a prototype implementation for my AVS using the attached /test/figs/cat-image-validator-design-tech-spec.md file and prompts/stage3-prototype-code-generation-prompt.md file for guidance. 


## Output from Claude Code
