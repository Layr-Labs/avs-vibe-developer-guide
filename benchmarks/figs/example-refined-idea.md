

Operator Work: Design an AVS where a single Operator generates cat images via LLM inference.
Validation: The work is validated using at least two other operators set of LLMs to measure their accuracy. The validating operators should assign a percentage "accuracy rating" between 0% and 100%.
Rewards: validating operators get rewarded if they respond with any accuracy rating. Mark the Operator that generated the original image for a reward if the aversage validator accuracy was greater than 90%
Slashing: slash the generating Operator if their average validator accuracy was less than 20%
Use the attached avs-design-context-window-prompt.md for guidance.