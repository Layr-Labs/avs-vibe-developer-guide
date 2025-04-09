# AVS coding context window prompt

Dear LLM - use this file to guide you in implementing the own Active Validation Service (AVS) on EigenLayer. Follow the steps below to ensure you fully specify the AVS requirements, implement the smart contracts, operator binaries, and run tests to validate the entire flow.

Mimic the [hello-world-avs repository](https://github.com/Layr-Labs/hello-world-avs) structure for guidance.

### 1. Contracts

- **Folder Structure**: Create a `contracts/src` folder.  
- **Service Manager**:  
  - Implement the primary AVS Service Manager contract.  
  - Include the logic to track registered operators, assign tasks, verify results, distribute rewards, and manage slashing.  
- **Core AVS Logic**:  
  - Define relevant data structures, events, and helper functions.  
  - Add modifiers/functions that implement your custom validation approach (e.g., multi-operator voting, challenge periods, or multi-step dispute processes).  
- **Rewards & Slashing**:  
  - Implement functions that handle reward distribution, operator ejections, and slashing conditions.

### 2. Operator

- **Folder Structure**: Create an `/operator` folder.  
- **Operator Binary**:  
  - A reference implementation showing how an operator listens for new tasks, processes them, and returns results.  
  - Include any necessary libraries or frameworks for your computation (e.g., cryptographic libraries).  
- **Operator Workflow**:  
  1. **Initialization**: Connect to the smart contract and register.  
  2. **Task Subscription**: Subscribe to events (e.g., new tasks available).  
  3. **Compute**: Perform the required computation or verification.  
  4. **Result Submission**: Post results on-chain (or via a designated off-chain aggregator that eventually writes on-chain).  

### 3. Tests

- **Folder Structure**: Create a `test` folder.  
- **Unit Tests**:  
  - Write tests for each component of the contract logic (registration, task assignment, validations, reward distribution, slashing).  
- **Integration Tests**:  
  - Simulate the entire AVS flow:  
    1. Deploy and initialize the contracts.  
    2. Register multiple operators.  
    3. Trigger a sample task.  
    4. Submit valid results from an operator.  
    5. Confirm reward payout.  
    6. Submit invalid results from another operator.  
    7. Confirm detection and slashing.  

### 4. README

Include a `README.md` file at the root of your repository with:

1. **Project Overview**: Brief explanation of what the AVS does.  
2. **Installation Instructions**: Prerequisites, environment variables, and libraries.  
3. **How to Run the Operator Binary**:  
   - Steps to compile or install any dependencies.  
   - How to start the binary and configure it (e.g., setting RPC endpoints, specifying network details, specifying wallet keys).  
4. **Testing Instructions**:  
   - How to run unit tests for the contracts.  
   - How to simulate end-to-end interaction with the AVS.  

---

## Example Directory Tree

Below is a basic layout for your repository:

my-avs/
├── contracts/
│   └── src/
│       ├── My-AVS-ServiceManager.sol
│       ├── RewardDistributor.sol
│       └── ...
├── operator/
│   ├── main.go (or main.ts, main.py, etc.)
│   └── ...
├── test/
│   ├── test_service.js
│   └── ...
├── README.md
└── package.json (or Cargo.toml, go.mod, etc.)




---

# Usage

2. **Implement Contracts**: Write and deploy the necessary solidity (or other smart contract language) files under `contracts/src`.  
3. **Develop Operator Code**: Implement an operator binary that can request, compute, and submit your AVS tasks.  
4. **Write Tests**: Create tests that cover normal and malicious scenarios.  
5. **Document Everything**: Keep your `README.md` up-to-date, ensuring anyone can clone the repo, run the operator, and test the AVS.

---

**Final Check**  
- Ensure all roles and flows are covered (operators, watchers, end-users).  
- Confirm that your decentralized validation rules are well-defined and robust.  
- Make sure reward and slashing logic is clear, transparent, and fair.

Use this prompt as a comprehensive reference whenever you build or modify the AVS. This ensures all crucial aspects of your decentralized service—design, implementation, validation, and testing—are addressed thoroughly.