# DeFeedMaker: Social Media Feed AVS

This directory contains a complete example of an AVS development process for DeFeedMaker, a decentralized news feed generator for the DeHacker social media platform.

## Development Stages

### Stage 1: Idea Refinement

#### Example Prompt

```
Help me generate a refined idea prompt for my AVS idea using [prompts/stage1-idea-refinement-prompt.md] for guidance. 

* Project Overview:   My project DeHacker is a decentralized social media app that needs help providing a news feed for its users. 
* AVS Purpose: The news feed must be generated in a decentralized way.
* Name: DeFeedMaker
* Operator Work: the operator will pull a list of posts from hacker news https://news.ycombinator.com/, aggregate them into a news feed based on most interesting and most related to web3. The Operator will provide references to the posts they pulled along with their provided news feed.
* Validation: validator will review the generated news feed against the articles to ensure it was generated legitimately.
```

#### Output
The output of this stage is the [defeedmaker-refined-idea-prompt.md](./defeedmaker-refined-idea-prompt.md) file, which defines the core concept, purpose, and operational parameters of the DeFeedMaker AVS.

### Stage 2: Design Generation

#### Example Prompt

```
Help me generate a design tech spec for my using the attached defeedmaker-refined-idea-prompt.md file and prompts/stage2-design-generation-prompt.md file for guidance.
```

#### Output
The output of this stage is the [defeedmaker-design-tech-spec.md](./defeedmaker-design-tech-spec.md) file, which contains a detailed technical specification including task definition, validation mechanisms, rewards distribution, and penalties.

### Stage 3: Prototype Implementation

#### Example Prompt

```
Help me generate a prototype implementation for my AVS using the attached defeedmaker-design-tech-spec.md file and prompts/stage3-prototype-code-generation-prompt.md file for guidance.
```

#### Output
The output of this stage is the [hello-defeedmaker-prototype](./hello-defeedmaker-prototype/) directory, which contains a complete prototype implementation of the DeFeedMaker AVS, including:

- Smart contracts for task creation, feed submission, and validation
- Operator implementation with Hacker News scraping and web3 relevance analysis
- Frontend interface for creating tasks and viewing feeds
- Deployment scripts and configuration files

## Implementation Details

The DeFeedMaker prototype demonstrates:

1. **Decentralized Feed Generation**: Operators pull content from Hacker News, analyze it for web3 relevance, and generate structured feeds.

2. **Multi-Validator Consensus**: Multiple validators check feeds for accuracy and relevance, with rewards based on quality.

3. **EigenLayer Integration**: Leverages EigenLayer for security, staking, and operator management.

## Running the Prototype

See the [hello-defeedmaker-prototype/README.md](./hello-defeedmaker-prototype/README.md) for detailed instructions on setting up and running the prototype.