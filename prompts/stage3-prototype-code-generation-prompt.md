
## Instructions for llm:

Dear LLM - use this file to guide you in implementing the the prototype Active Validation Service (AVS) on EigenLayer. Follow the steps below to ensure you fully implement the smart contracts, operator binaries, and frontend.

- Mimic the [hello-world-avs repository](/lib/hello-world-avs/) structure for guidance.

- Create a build plan before you generate the new files. 
- Ask the user to confirm your build plan before proceeding. 
- Create a prototype implementation under a new folder named "hello-[avs-name]-prototype"
- Document Everything: keep your `README.md` up-to-date, ensuring anyone can clone the repo, run the operator, and test the AVS.
- Final Check - Ensure the code executes correcttly and resolve any compile errors.
- Do not generate code for Slashing for now.


---





## 1. Contracts

- **Service Manager**:  
  - Implement the primary AVS Service Manager contract.  
  - Include the logic to track registered operators, assign tasks, verify results.
- **Core AVS Logic**:  
  - Define relevant data structures, events, and helper functions.  
  - Add modifiers/functions that implement your custom validation approach. 


## 2. Operator

All Operator code should be written in TypeScript.
- **Folder Structure**: Create an `/operator` folder. 
- **Operator Binary**:  
  - A reference implementation showing how an operator listens for new tasks, processes them, and returns results.  
  - Include any necessary libraries or frameworks for your computation (e.g., cryptographic libraries).  
- **Operator Workflow**:  
  1. **Initialization**: Connect to the smart contract and register.  
  2. **Task Subscription**: Subscribe to events (e.g., new tasks available).  
  3. **Compute**: Perform the required computation or verification.  
  4. **Result Submission**: Post results on-chain (or via a designated off-chain aggregator that eventually writes on-chain).  

## 3. README

Include a `README.md` file at the root of your repository with:

1. **Project Overview**: Brief explanation of what the AVS does.  
2. **Installation Instructions**: Prerequisites, environment variables, and libraries.  
3. **How to Run the Operator Binary**:  
   - Steps to compile or install any dependencies.  
   - How to start the binary and configure it (e.g., setting RPC endpoints, specifying network details, specifying wallet keys).  
4. **Testing Instructions**:  
   - How to run unit tests for the contracts.  
   - How to simulate end-to-end interaction with the AVS.  


## 4. Front End

Generate a simple front end in React that allows users to generate Tasks and observe events on chain when they are completed.

---

## Example Prototype Directory Tree

Below is a basic layout for your repository:

my-avs-prototype/
├── contracts/
│   └── leverage the existing contracts folder from Hello World where possible.
│   └── src/
│       ├── Create a custom service manager for this AVS with naming pattern [AVSName]ServiceManager.sol
│       ├── RewardDistributor.sol
│       └── ...
├── operator/
│   ├── main.go (or main.ts, main.py, etc.)
│   └── ...
├── avs-frontend/
│   ├── include the react web app here.
│   └── ...
├── README.md
└── package.json




## MCP Resources:
Use the following MCP resources to guide your work:
eigenlayer-docs-overview
eigenlayer-docs-developer
eigenlayer-middleware-docs
eigenlayer-middleware-src
eigenlayer-contracts-src
eigenlayer-contracts-docs