## Instructions for llm:

Dear LLM - use this file to guide you in implementing the the prototype Active Validation Service (AVS) on EigenLayer.

## Your job
Your job is to modify this copy of hello-world-avs to fit the AVS design or idea supplied by the user.

If the user has supplied a sufficiently clear design tech spec with this prompt: review it first to make sure you undertand the design goals before proceeding.

If the user has supplied only an idea with this prompt: first generate the idea using the following guidance:
"Help me generate a design tech spec for my using the supplied AVS idea and stage2-design-generation-prompt.md file for guidance."
Then ask the user whether they would like to review the generated design tech spec before proceeding.

If the user has Not supplied an idea or the AVS idea or design is not clear: explain the missing information to the user, help guide them on how they should add proper detail.




## Preparation
If the user has established a sufficiently clear design tech spec with this prompt, follow the steps below to ensure you fully implement the smart contracts, operator binaries, and frontend.
- Create a copy of the repository from (/lib/hello-world-avs/) to /output.
- Rename the folder to "hello-[avs-name]-prototype" in the /outputs folder.
- Create a prototype implementation under a new folder named "hello-[avs-name]-prototype" in the /outputs folder.
- Document Everything: modify the `README.md`, keep it up-to-date, ensuring anyone can clone the repo, run the operator, and test the AVS.
- Create a build plan before you generate the new files. Write the plan to /outputs/hello-[avs-name]-prototype folder.
- Ask the user to confirm your build plan before proceeding. 

## Code Generation
Modify the code in the newly created /outputs/"hello-[avs-name]-prototype" folder based on the detailed following guidance.

### 1. ServiceManager Smart Contracts

- **Service Manager**:  
  - Modify the primary AVS Service Manager contract to fit the logic of the new AVS. 
  - Include the logic to track registered operators, assign tasks if necessary, and verify results.
- **Core AVS Logic**:  
  - If needed: define relevant data structures, events, and helper functions.  
  - If needed: add modifiers/functions that implement your custom validation approach. 
  - Do not generate code for Slashing.

### 2. Operator Typescript Code

All Operator code should be written in TypeScript.
- Use the `/operator` folder. 
- Modify the index.ts file to represent the offchain Operator logic.
- Include any necessary libraries or frameworks for your computation (e.g., cryptographic libraries).  
- Operator Workflow:  
  1. Initialization: Connect to the smart contract and register.  
  2. Task Subscription: Subscribe to events (e.g., new tasks available).  
  3. Compute: Perform the required computation or verification.  
  4. Result Submission: Post results on-chain (or via a designated off-chain aggregator that eventually writes on-chain).  

### 3. README

Modify the `README.md` file at the root of your repository to include:
1. Project Overview: Brief explanation of what the AVS does.  
2. Installation Instructions: Prerequisites, environment variables, and libraries.  
3. How to Run the Operator Binary:  
   - Steps to compile or install any dependencies.  
   - How to start the binary and configure it.
4. Testing Instructions:  
   - How to run unit tests for the contracts.  
   - How to simulate end-to-end interaction with the AVS.  

### 4. Front End

Modify the simple front end under /avs-frontend folder that allows users to generate Tasks and observe events on chain when they are completed.




## Final Checks
Ensure the code executes correctly and resolve any compile errors. Try to build the smart contracts with build:forge and resolve any compilation errors.
Provide the user with a pointer to the README.md file and instructions on how they can test the AVS.



## MCP Resources:
Use the following MCP resources to guide your work:
eigenlayer-docs-overview
eigenlayer-docs-developer
eigenlayer-middleware-docs
eigenlayer-middleware-src
eigenlayer-contracts-src
eigenlayer-contracts-docs