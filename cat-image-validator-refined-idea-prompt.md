# CatLLM: AI Cat Image Generation and Validation AVS

## 1. Project Overview
CatLLM is a decentralized AI cat image generation platform that produces high-quality, unique cat images for users. It leverages the security and decentralization of EigenLayer to ensure reliable image generation and quality assurance.

## 2. AVS Purpose
This AVS provides a decentralized approach to AI image generation by distributing both the creation and validation of images across multiple operators. This increases reliability, prevents centralized censorship, and ensures consistent quality through a decentralized validation mechanism.

## 3. Name
CatLLM (Cat Large Language Model) Validator

## 4. Operator Work
The AVS consists of two types of operators:
- **Generator Operator**: A single operator that generates cat images using LLM inference
- **Validator Operators**: Multiple operators that evaluate the quality and accuracy of the generated cat images using their own LLMs

## 5. Validation
The work is validated through a decentralized accuracy assessment system where at least two validator operators independently evaluate each generated cat image. Each validator assigns an accuracy rating between 0% and 100% based on criteria such as:
- Image quality
- Recognizability as a cat
- Adherence to any specified attributes or characteristics
- Uniqueness compared to previously generated images

These independent validations create a consensus mechanism that prevents any single validator from having undue influence over the acceptance or rejection of generated images.

## 6. Rewards
Reward distributions to Operators are based on the following criteria:
- Validator operators receive rewards for each completed validation, regardless of the score they assign
- The generator operator receives rewards when the average validator accuracy rating exceeds 90%, incentivizing high-quality image generation
- This reward structure ensures both thorough validation and high-quality image generation

This model incentivizes honest validation (since validators are always rewarded for participation) while also motivating the generator to produce high-quality cat images (since they only receive rewards for exceptional work above the 90% threshold).