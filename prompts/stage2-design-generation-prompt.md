# AVS Context Window Prompt

Dear LLM - use this file to guide you in use this document to guide you in designing and implementing your own Active Validation Service (AVS) on EigenLayer. Follow the steps below to ensure you fully specify the AVS requirements, implement the smart contracts, operator binaries, and run tests to validate the entire flow.


---

## Part 1: AVS Design

1. AVS Purpose and Scope  
   - Clearly define the use case your AVS is meant to solve.  
   - Explain how you envision your AVS bringing value in a trust-minimized, decentralized manner.

2. Designing the Operator "Work" (aka task)  
   - Operator Task Definition: Specify what type of computation each operator must perform (e.g., cryptographic proofs, off-chain data aggregation, specialized calculations).  
   - Input and Output: Determine what input is required for the work and in what format operators should produce their results (binary, hash, proof, etc.).  

3. Decentralized Validation  
   - Validation Mechanism: Describe how multiple validators verify the work.  
   - Consensus Rules: Define how nodes reach agreement on the correctness of the operator outputs. Outline any dispute resolution or challenge process. 

4. Rewards Distribution for Successful Behaviors  
   - Reward Conditions: Enumerate clear conditions under which operators are paid.  
   - Payment Mechanism: Determine whether rewards are distributed immediately after successful validation or on a scheduled basis (e.g., after a certain number of blocks).  

5. Penalties for Malicious Behaviors  
   - Malicious Activity Definition: List the scenarios in which operators are considered malicious (e.g., providing invalid results, failing to produce results, spamming the network).  
   - Punitive Actions: Explain how malicious operators face:  
     - Reward Withholding: Operators forfeit rewards for the affected epoch.  
     - Operator Ejection: The system ejects the Operator and excludes them from future tasks.  
     - Slashing: A portion of their stake is confiscated as a penalty for malicious or negligent behavior.


---

## Usage

- Start by Designing: Walk through the design steps, specifying tasks, validation, and rewards/slashing in detail.  
- Generate Output avs-custom-development-plan.md: Keep your `README.md` up-to-date, ensuring anyone can clone the repo, run the operator, and test the AVS.
- Do not generate Technical Implementation Considerations
- Generate a simple friendly logo image that represents the AVS idea.
---

Final Check  
- Confirm that your decentralized validation rules are well-defined and robust.  
- Make sure reward and slashing logic is clear, transparent, and fair.



## Appendix

Take inspiration from this human tech spec template if helpful:
https://docs.google.com/document/d/15hqzDsVJhBFiopaRUJHSSgaqdmsrXk2YUe8SOb2qtww/edit?usp=sharing