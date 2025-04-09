# Refined AVS Idea: FIGS - Feline Image Generation Service

## 1. Project Overview
**FIGS** is a decentralized cat image generation service that creates high-quality, unique cat images through LLM inference. The service provides value by offering a reliable source of cat imagery that can be used for various applications like NFTs, web design, social media, and entertainment platforms.

## 2. AVS Purpose
The FIGS AVS ensures that generated cat images meet quality standards through decentralized validation. This approach secures the image generation process by preventing low-quality or inappropriate content, while maintaining decentralization through multiple independent validators.
 
## 3. Name
**FIGS Validator Network**

## 4. Operator Work
The Operators in this AVS have two distinct roles:
- **Generator Operator**: A single designated Operator runs LLM inference to generate cat images based on user requests.
- **Validator Operators**: Multiple independent Operators run separate LLM validation models to assess the quality, accuracy, and appropriateness of the generated cat images.

## 5. Validation
The work is validated through a decentralized rating system:
- Each Validator Operator independently assesses the generated images using their own LLM models
- Validators assign an "accuracy rating" between 0% and 100% based on criteria such as:
  - Image quality
  - Adherence to cat characteristics
  - Absence of inappropriate content
  - Uniqueness of the generated image
- The final validation is determined by averaging the accuracy ratings from all validators
- A consensus threshold of 90% average accuracy is required for the image generation to be considered successful

## 6. Rewards
Rewards distributions to Operators are based on:
- **Validator Operators**: Receive rewards for each validation performed, regardless of the accuracy rating they provide, to incentivize participation and honest assessment.
- **Generator Operator**: Receives rewards only when the average validator accuracy rating exceeds 90%, ensuring high-quality image generation.

This reward structure incentivizes both quality image generation and honest validation, creating a balanced system that produces reliable cat imagery through decentralized means.