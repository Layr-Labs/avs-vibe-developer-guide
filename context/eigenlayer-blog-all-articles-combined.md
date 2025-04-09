EigenLabs is an independent publication launched in March 2023 by Brianna Montgomery. If you subscribe today, you'll get full access to the website as well as email newsletters about new content when it's available. Your subscription makes this site possible, and allows EigenLabs to continue to exist. Thank you!

### Access all areas

By signing up, you'll get access to the full archive of everything that's been published before and everything that's still to come. Your very own private library.

### Fresh content, delivered

Stay up to date with new content sent straight to your inbox! No more worrying about whether you missed something because of a pesky algorithm or news feed.

### Meet people like you

Join a community of other subscribers who share the same interests.

* * *

### Start your own thing

Enjoying the experience? Get started for free and set up your very own subscription business using [Ghost](https://ghost.org/), the same platform that powers this website.Teams are launching high performance, low cost rollups and rollup services on EigenDA. In this post we’ll take a look at some of them and learn how you can get started building your own rollup.

* * *

Rollups as a Service (RaaS) provide everything you need to build, customize, and deploy a new chain.

They are drastically lowering the barrier to entry for building and deploying customized blockchains and are reshaping the landscape of web3 infrastructure.

### Why RaaS?

1. **Customization and flexibility**. RaaS platforms provide a range of customization options, allowing you to tailor chain to specific use cases and needs. You can choose the underlying rollup architecture (e.g., Optimistic, ZK, or others), select the native token, configure gas, configure the sequencing mechanism, and integrate with infrastructure providers like coprocessors, oracles, bridges, indexers, and explorers.

TLDR: they enable teams to more easily solve specialized problems.
2. **Reduced technical overhead**. Building and maintaining a rollup network from scratch is complex and resource-intensive, requiring a lot of specialized knowledge and ongoing maintenance. RaaS abstract away much of this complexity, handling tasks like node operation, software updates, and infrastructure management. This reduced technical overhead allows teams to focus on building their applications and core business logic.
3. **Ease of deployment.** Many RaaS providers allow you to deploy a fully-functional rollup with just a few clicks, eliminating the need for extensive technical expertise or complex setup processes. This empowers more teams and individuals to experiment with and build on top of rollup technology.

Before diving in to the ways you can get started building, let’s talk about EigenDA and data availability, the primitive that unlocks these capabilities.

### Data availability and EigenDA

Data availability networks are dedicated systems designed to provide a secure, decentralized, and scalable way to store and retrieve transaction data, enabling rollups to offload data availability requirements and focus on execution, improving throughput and reducing costs.

[EigenDA](https://docs.eigenlayer.xyz/eigenda/overview) is a data availability store for rollups made by Eigen Labs, built on top of EigenLayer as an [AVS](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-developer-guide), enabling high throughput and low cost transactions at scale.

EigenDA is:

- **Scalable.** Write throughput scales linearly with number of operators. At launch EigenDA will provide 10 MB/s of write throughput, around 5x greater than the nearest competitor. EigenDA write throughput will continue to increase by scaling linearly with its number of operators.
- **Secure.** EigenDA is decentralized, and made up of thousands of operators registered in EigenLayer whose delegated stake imposes an economic cost to misbehavior, and will have billions of dollars of economic security at launch.
- **Cheap.** EigenDA blockspace pricing is primarily constrained by cost-of-security, which is shared with Ethereum and other EigenLayer AVSs. As a result, we expect EigenDA to be orders of magnitude cheaper than competitors.
- **Ethereum-centric.** EigenDA blob writes are registered with contracts on Ethereum, which natively holds operators to promise of attestations. Ethereum L2s using EigenDA avoid any trust assumption on another chain's light client, which can be fooled by dishonest validator sets.

Some teams who’ve chosen EigenDA for data availability include [Mantle](https://www.mantle.xyz/), [Polymer](https://www.polymerlabs.org/), [LayerN](https://www.layern.com/), [Versatus](https://versatus.io/), [Movement Labs](https://movementlabs.xyz/), [Cyber](https://cyber.co/), and [MegaETH](https://twitter.com/megaeth_labs). You can see some of the teams and products building rollups and rollup services on EigenDA [here](https://www.eigenlayer.xyz/ecosystem?category=Rollup%2CAVS).

### Customizing and deploying a rollup on EigenDA

Let’s take a look at some of ways to get started building your next rollup, and the different features RaaS providers offer.

* * *

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F144a2595-6643-402c-bee3-d7a0c1a7ad80_2000x400.png)

## AltLayer with EigenDA

[AltLayer](https://altlayer.io/) is a decentralized protocol that facilitates the launch of rollups with both optimistic and zk rollup architectures. You can choose between rollup stacks like Optimism, Arbitrum, Polygon CDK, or ZKSync.

They provide [integrations](https://altlayer.io/#ecosystem) with block explorers, oracles, bridges, on-ramps, indexers, and more.

![](https://substackcdn.com/image/fetch/w_2400,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc978477f-0ef2-49b3-948a-caf8d3cfb90c_1172x626.jpeg)

They offer two types of Rollups: Native rollups as a service and Restaked rollups as a service.

#### Restaked rollups

[Restaked rollup](https://altlayer.io/restaked-rollup) s are different in that they leverage EigenLayer’s restaking mechanism to make the rollups more decentralized, secure, interoperable, and faster

Restaked rollups are rollups that depend on one or more of 3 AVSs built on EigenLayer for [verification](https://altlayer.io/restaked-rollup#vital), [fast finality](https://altlayer.io/restaked-rollup#mach), and [decentralized sequencing](https://altlayer.io/restaked-rollup#squad).

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf3582dd-0708-4c98-92c1-c25e10377392_1456x818.png)

AltLayer offers white glove, 1 on 1 support in setting up everything you need to deploy a custom rollup. The best way to get started building with AltLayer is to [reach out to their team](https://docs.altlayer.io/altlayer-documentation/community-and-support/support).

* * *

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2bffe1a2-2638-498e-9bc2-6ad92c10afff_1456x291.png)

## Conduit with EigenDA

[Conduit](https://conduit.xyz/) enables developers to deploy fully-managed, production-grade rollups on Ethereum. They offer a way to deploy rollups directly through their UI and dashboard in just a few clicks and with no code required.

Conduit supports launching chains using both the OP stack as well as Arbitrum Orbit, and have deployed with customers like [Zora](https://zora.co/), [Aevo](https://www.aevo.xyz/), and [Gitcoin](https://www.gitcoin.co/).

In addition to choosing a custom framework and data availability layer, you can also choose a custom settlement layer (like Ethereum, Base, Zora, or Mode).

Along with your deployments you will also get access to logs, monitoring, a Blockscout explorer, and [samczsun's](https://twitter.com/samczsun) [transaction tracer](https://tx.eth.samczsun.com/). You'll also automatically get access to new components as Conduit ships upgrades.

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8c02525-c160-4ea3-919d-ee3a9d740dc7_1030x977.png)

You can get started building on Conduit with EigenDA [here](https://conduit.xyz/).

* * *

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6df6f69c-8a96-4d21-a17d-d478d1aa77ab_1456x291.png)

## Caldera with EigenDA

With [Caldera](https://www.caldera.xyz/) you can deploy EVM compatible rollups in one click, launching with OP Stack, Arbitrum Orbit, and Polygon CDK.

They allow choosing between shared or decentralized sequencing and allow you to choose any token as the native, fee-paying token of your chain.

Caldera rollups come with performant RPC nodes, a block explorer, a data indexer, and a bridge interface to the settlement chain. Caldera rollups also benefit from [integrations](https://caldera.xyz/integrations/) with 40+ infrastructure providers.

Caldera will soon support EigenDA as an option when configuring your rollup with custom data availability.

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedf03e85-7b7f-48f4-b97e-73b5be428e34_1423x1197.png)

* * *

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4748a29e-d5a9-4b82-966d-2d4ea5becbfd_1456x291.png)

## Gelato with EigenDA

[Gelato](https://www.gelato.network/) offers an all-in-one Ethereum [Rollup as a Service Platform](https://www.gelato.network/raas).

With it, you can deploy production-grade & fully-serviced L2 and L3 rollups at a pace natively integrated with [25+ infrastructure providers](https://www.gelato.network/raas-marketplace) and developer tooling like oracles, bridges, indexers, block explorers, fiat on and off ramps, and account abstraction.

You can customize your chain specification to suit your project's unique requirements without any coding required, such as flexible sequencer design, native token, and alternative settlement layers.

Choose to deploy with Optimism OP stack, Arbitrum Orbit, Polygon CDK, or zkSync ZKStack with L2 and L3 rollups like [Astar](https://astar.network/), [Lisk](https://lisk.com/), [Playnance](https://www.playnance.com/), [Re.al](http://re.al/) and [Reya](https://reya.network/) in production today.

Gelato will soon enable rollup deployments with EigenDA on test & mainnet and rollups restaking as part of their product offering.

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd2340096-6c28-4c93-9fc4-cb8b690406fa_2880x2454.png)

* * *

## What next?

With rollups becoming easier to customize and deploy, a completely new design space is opening up. We’ll continue to see more growth and experimentation, with teams building chains for specialized and specific use cases that in the past would not have been feasible.

There is also a new [ecosystem of protocols on EigenLayer](https://www.eigenlayer.xyz/ecosystem?category=AVS) popping up like oracles, sequencers, coprocessors, and more catering to these new networks.

Like smart contract blockchains ushered in a new wave of innovation for app developers, rollups are doing the same for developers and teams interested in building their own networks. **Rollups are the new smart contracts**.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/#/portal)

Subscribe# 404

Page not found

[Go to the front page →](https://www.blog.eigenlayer.xyz/)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/2024/02/photo_5096171691515686419_y-1.jpg)

We founded Eigen Labs in 2021 with the mission to empower _open innovation_ across the blockchain infrastructure stack via building the [EigenLayer](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/eigenlayer.xyz) protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and applications with strong assurances of reliability and scalability. This supercharges the entire crypto ecosystem with both a higher quantity and quality of use cases, which ultimately delivers to end users safer and more useful crypto products, while supporting decentralization.

A canonical example of a service built on EigenLayer that benefits from renting crypto-economic security and decentralized validators from the Ethereum network is [EigenDA](https://docs.eigenlayer.xyz/eigenda/overview/). EigenDA is a secure and decentralized data availability layer that enables Ethereum developers to achieve high transaction speeds and low transaction costs that have never before been possible on Ethereum. EigenDA is only one example of a new EigenLayer-powered protocol that is bringing previously impossible use cases to reality. Many developers are actively creating new categories of protocols that will launch on EigenLayer – these protocols range across infrastructure verticals including cross-chain messaging, transaction ordering, off-chain data access, AI inference, fully homomorphic encryption, secret sharing, secure hardware enclave networks, and more.

We are thrilled to be partners with this incredible community of developers in the EigenLayer ecosystem. They are eager to experiment and build on Ethereum without the security and decentralization barriers that have gated them in the past. We are all here in the shared interest of maximizing open innovation for the benefit of the future of crypto. To further empower this mission we are excited to announce that Eigen Labs, the core development team contributing to EigenLayer and EigenDA, has raised $100 million from [a16z crypto](https://a16zcrypto.com/).

There are many reasons we are excited to partner with a16z crypto, but here are a few of the foundational ones:

### Commitment to research, including open source and public goods contributions:

a16z crypto’s research team is led by widely recognized and highly regarded researchers such as Tim Roughgarden and Dan Boneh. At Eigen Labs, our DNA is rooted in academic research, with founder Sreeram Kannan previously teaching at the University of Washington and leading the UW Blockchain Lab. We believe that a focus on quality research best enables us to achieve our mission.

Moreover, a16z crypto has demonstrated a commitment to open source contributions: for example, a16z crypto research often makes its work public, and a16z crypto engineers have made important contributions to the industry such as [Helios](https://a16zcrypto.com/posts/article/building-helios-ethereum-light-client/) (an Ethereum light client) and in the field of formal verification.

### Long-term engagement and support across the crypto industry stack:

a16z has been committing funds to the crypto industry for a long time, and began publishing its perspective on [Bitcoin](https://a16z.com/bringing-bitcoin-to-everyone/) and [Coinbase](https://cdixon.org/2013/12/12/coinbase) as early as 2013. In the decade-plus since then, a16z has continued to be a supporter of the industry in a number of important ways. In addition, a16z is advocating for a [positive technological future](https://a16z.com/politics-and-the-future/) with policymakers and regulators, and supporting [American Dynamism](https://a16z.com/american-dynamism/).

This long-term commitment to the industry, and forward-thinking work to bridge industry and government, captures much of why we are excited to work with a16z crypto: we are also dedicated to long-termism; we are focused on doing things the right way; we would like to serve our industry as an example of trust, safety, integrity, and respect.

### The definition of “Eigen”:

The name “Eigen Labs'' derives from the German word _Eigen_, which means “your own”. We chose this name because we believe that by making crypto-economic trust programmable and accessible to all, we can empower builders to more easily and smoothly launch their “own” protocols and applications, thereby enabling more stepping stones towards open innovation.

By chance, Chris Dixon, founder and managing partner of a16z crypto, recently released a book with his thesis on the future of blockchains: [_Read Write Own_](https://readwriteown.com/). This synergy, as well, could not be ignored!

Our mission is to maximize open innovationin blockchain infrastructure. This mission has a beginning, but no end. We consider ourselves players in an infinite sum, infinite horizon game. We are pleased to join with a16z crypto on this mission. The future is bold, and yet to be written. We’re excited to make it _your_ _eigen_.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/#/portal)

SubscribeWe are excited to share that the six LSTs, garnering over 15k ETH in support during the [LST election contest](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/), will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.

### New LSTs in the EigenLayer ecosystem

During the period spanning from October 27th to November 1st, EigenLayer's active restakers and LST contestant holders participated in an on-chain voting process. This voting mechanism allocated one vote for every 1 ETH in restaking or contestant LST capital, resulting in substantial engagement, with over 12.8k wallets and an impressive total of >500k ETH in holdings taking part. The outcomes of this significant participation are detailed in the table below.

![](https://lh7-us.googleusercontent.com/JXmpL8ZEQPzuBhjnLh5ymk0CNZDiDVGcqXjPypsW54rKZaOYYX1eLiogvnMPtH89aOcUYDVy_-nyCY7wNOQ_HblkLV7E98J5D6musbepbTnYcWAHgEoAbKPPyd-50BonNa2PrYb04YNaX_M3PnpqMM4)

These LSTs are now ready to be incorporated as restaking assets within the EigenLayer ecosystem. The [operations multisig](https://etherscan.io/tx/0x460180086ac4ee169ff805e638118b7d60b6c96ae10b80db9524c7bef9b577f5) has initiated the transaction to resume protocol activities. Following the standard 10-day waiting period, the assets will be smoothly incorporated into the system, along with an appropriate adjustment in the cap limits.

Mark your calendars, as this exciting development is scheduled for **December 18th at 10 am Pacific time.** It's worth noting that this approach deviates from the initial plan of sequentially adding each LST. Instead, all six LSTs will be introduced simultaneously, a decision driven by engineering efficiency and the aim to expedite onboarding. This streamlined approach stands to benefit not only the LST protocols, but also the holders and the EigenLayer community. Below are the LSTs set to be added:

1. [wBETH](https://etherscan.io/token/0xa2e3356610840701bdf5611a53974510ae27e2e1) (Binance)
2. [osETH](https://etherscan.io/address/0xf1C9acDc66974dFB6dEcB12aA385b9cD01190E38)(Stakewise)
3. [swETH](https://etherscan.io/token/0xf951E335afb289353dc249e82926178EaC7DEd78) (Swell)
4. [AnkrETH](https://etherscan.io/token/0xE95A203B1a91a908F9B9CE46459d101078c2c3cb) (Ankr)
5. [EthX](https://etherscan.io/token/0xA35b1B31Ce002FBF2058D22F30f95D405200A15b) (Stader)
6. [oETH](https://etherscan.io/token/0x856c4Efb76C1D1AE02e20CEB03A2A6a08b0b8dC3) (Origin ETH)

After the public contest, several LST teams have shown interest in being added to the ecosystem. **Liquid Collective with LsETH**, **Frax with sfrxETH** and **mETH by the Mantle** team are conducting audits and executing plans to onboard to EigenLayer in early 2024.

### Raising the restaking caps

As we introduce the new LST additions to the mainnet, the caps for all current LSTs (rETH, stETH, cbETH) **will simultaneously increase.** These updated caps, crafted with security as our top priority and in alignment with our gradual rollout strategy, will establish a uniform **200k cap** for each LST, increasing from the 100k cap set on cbETH, rETH and stETH. A global pause mechanism will come into effect as the total LST TVL approaches roughly **500k ETH**.

This means that:

- When the cumulative restaked capital for LSTs hits the 500k ETH milestone, the multisig governance will enact a global pause, temporarily halting deposits.
- If an individual LST reaches the 200k ETH threshold, its respective pool will be temporarily closed to new deposits.
- As long as the global 500k ETH limit remains unmet, other LSTs will continue to accept deposits.

As always, [**native restaking**](https://docs.eigenlayer.xyz/restaking-guides/restaking-user-guide/native-restaking) **will remain uncapped.**

The upcoming cap increase on December 18th marks the fourth opportunity this year for our community to restake on EigenLayer with LSTs. We initiated this journey six months ago and have since been profoundly grateful for the community's unwavering support, resulting in the current achievement of 184k ETH in restaked capital across native and LST restaking.

We eagerly anticipate the future, where we will open the doors for trust capital to flow freely into the ecosystem without caps, bringing us closer to the Stage 2 mainnet launch of [Operators and EigenDA](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/) in the first half of 2024.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/#/portal)

Subscribe## **Overview**

The internet's evolution has diverged from its original promise. What once represented limitless possibilities for freedom, knowledge, and human connection now operates differently. Today, powerful technology platforms dominate much of our online discovery, content, and conversations. These systems, often operating with opaque models, underpin significant economic activity and increasingly serve as sources of truth.

Inference Labs was formed to address this challenge, working to democratize and widely distribute Artificial Intelligence by integrating the transformative properties of Web3. However, they faced a fundamental obstacle: today's blockchains do not natively support AI in a verifiable way.

Their solution: a Zero-Knowledge Verified Inference Network (ZK-VIN). By submitting zero-knowledge proofs on-chain, operators can verifiably confirm that AI models were properly implemented, transforming black-box AI outputs into trustworthy, on-chain truth.

Inference Labs has already demonstrated Web3's power to advance machine learning through their [Omron subnet](https://omron.ai/) on Bittensor. The incentive mechanisms built into the Bittensor ecosystem helped reduce median zero-knowledge proving times from 15 seconds to just 5 seconds. Now, with [Sertn AVS](https://public.inferencelabs.com/sertn-whitepaper.pdf) (Autonomous Verifiable Service) on EigenLayer, they're scaling even further, activating an ecosystem where verified AI isn't just possible, it's inevitable.

## **The Challenge: Verifiable AI on Blockchain**

AI verification poses a significant technical challenge for blockchain systems. Traditional approaches face a fundamental dilemma: verifying AI outputs on-chain requires prohibitively expensive computation. Continuous proof generation for every AI interaction would make practical applications financially unfeasible and catastrophically slow.

The primary existing solution, zero-knowledge machine learning (zkML), offers cryptographic certainty but at extreme computational cost. Traditional zkML proofs are resource-intensive and time-consuming, making them impractical for real-world applications at scale. The computational overhead and time delays would effectively prevent widespread adoption of verifiable AI on blockchain.

## **Inference Labs' Solution: Zero-Knowledge Verified Inference Network**

Inference Labs emerged to solve this verification challenge with their Zero Knowledge Verified Inference Network (ZK-VIN). Their breakthrough approach enables off-chain AI execution while providing on-chain proof of correctness through a hybrid system of random cryptographic verification combined with substantial economic penalties for incorrect execution.

This model delivers the security benefits of zkML without its prohibitive costs, creating a practical path to verifiable AI at scale. By intelligently combining cryptographic verification with economic incentives, Inference Labs created a system that makes dishonest behavior mathematically unprofitable.

## **The Implementation Challenge: Economic Security at Scale**

Implementing this solution presented its own challenge: establishing sufficient economic security. For their hybrid verification system to work effectively, Inference Labs needed a mechanism to make dishonest behavior mathematically unprofitable through financial penalties. Building this security layer from scratch would require massive capital investment and infrastructure development, potentially delaying their solution by years.

The core challenge for any new AVS is bootstrapping security. Without enough stake to secure the network, the system remains vulnerable to attacks. Traditionally, ETH stakers hesitate to allocate capital to new services due to lost staking rewards and the inherent slashing risks of unproven infrastructure.

## **EigenLayer's Role: Enabling the Economic Security Layer**

This is where EigenLayer provided the critical infrastructure. By leveraging EigenLayer's restaking protocol, Inference Labs gained access to Ethereum's robust economic security without needing to bootstrap their own security network. This allowed them to implement their slashing mechanics efficiently, making their random verification system viable at scale while dramatically accelerating their time to market.

EigenLayer fundamentally changes the security equation and brings the economic might of the Ethereum Virtual Machine (EVM) to Inference Labs' Sertn AVS. By enabling capital reuse across multiple services, EigenLayer removes the opportunity cost of staking in Ethereum. Restakers no longer have to choose between securing Ethereum and participating in a new AVS. Instead, they can extend security to multiple services, effectively making the capital cost of securing a trustworthy operator theoretically zero while unlocking new revenue streams.

This represents a paradigm shift. AVSs no longer have to compete with Ethereum's staking yield to attract security. Instead, they can focus on building robust, verifiable systems with market-driven economic incentives.

## **How Sertn AVS Works: The Technical Architecture**

Sertn AVS transforms EigenLayer's economic security into an adaptive, probabilistic verification model that balances security and efficiency. Here's how it works:

### **1\. Staker-Aligned Task Allocation**

Operators stake ETH relative to a task's value and risk; high-staked operators handle high-value tasks.

- **STEP 1:** Users submit tasks with fees based on urgency and risk.
- **STEP 2:** Tasks go to operators who meet the deadline and stake enough to secure them.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe7MchqUxv9folejnvJgtywmyxvbtUkbN4jo5M6jHkjH4BzwyORZ4ceaDySZ5JoG1VUo5S4VV-JmsORu115OH6fEhwMjDfO-2paqQJErKVamb12KMzyqPUcJ_E9uo5lHaw-antuaw?key=5U7lQ20IjqjO2JwN8pn5haJ-)

_Note: A "task" refers to running a provable computation which may take the form of inference for a model or other structured computation._

### **2\. Dynamic Verification Mechanism**

Instead of verifying every task (which would be computationally prohibitive), the system employs randomized proof requests. Operators must be able to prove correctness at any time. Note that there's support for different proof systems, but the task is 1:1 at the model level, meaning proving systems can't be switched after inference.

- **STEP 3:** Operators may be randomly asked for proof, and users can request proof for a fee. Fees rise with repeated requests to prevent spam and reward valid proofs.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdeNFvIiy_60gsWZ9rXCx_jU5yCTqYtPhr8w9XSboW3Nf-1M9r6LFDFaD3J2hnRx_HcvXv9w-G7GO21kIfinZCIURA3dY4HDmhgt4gdao31z8I4DqhgEWD62WMnUmk_CrVdmuAsyw?key=5U7lQ20IjqjO2JwN8pn5haJ-)

### **3\. User-Triggered Audits**

If a user doubts a result, they can request additional verification for a fee. This fee scales dynamically to prevent griefing attacks and ensures that operators are fairly compensated for computational costs.

- **STEP 4:** If an operator fails to prove their work, they get slashed, and the user is compensated. Cheating isn't profitable, and slashing always outweighs corrupt gains. Unreliable operators face more proof checks and risk further slashing.

![A diagram of a task  Description automatically generated](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfuP4sM7bhiY4SzIpv-n81p9NN-yU13YlkUl3QJIrkSdnvrl-qGwFPEmzndbOT2OiTnHiG8GcrSdG5gjIz3BEIKhZf6DzsHzCKepyHK3qkEPQpS3ynenUxJ0Eoj1dLZQPzz83XDiw?key=5U7lQ20IjqjO2JwN8pn5haJ-)

### **4\. Mathematically Unprofitable Dishonesty**

If an operator fails a proof request, they are slashed significantly. The slashed funds are redistributed to affected users. Operators with a history of dishonesty face increased verification rates and retrospective audits of past work.

## **The Economic Flywheel Effect**

In practice, users submit tasks with fees based on urgency and corruption risk. Via an orchestrator, operators take on tasks they can secure– e.g., a $10M-staked operator won't process a task requiring just $10 of security. This keeps high-value tasks in trusted hands while ensuring smaller tasks remain accessible.

Through this model, EigenLayer enables a self-reinforcing economic flywheel where the best models and most reliable operators thrive. Instead of relying on trust, the system is hardened by continuous cryptographic verification and high-stakes economic penalties.

## **Results and Impact**

### **Technical Achievements through the Omron Subnet**

Inference Labs showcased the power of its approach through the Omron subnet:

- 76% faster than the industry standard for zkML proof generation
- Reduced median proving times from 15s to 5s
- Seamless verification across all EVM-compatible blockchains
- Lowered barriers for new operators

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcwUJGgnv4KE5CA3tG1H816xMgIZLEcKWd9-6Z2qjTcogfi_s37zanNlHAfRc95iJlst0BNOZ46ra1OK9OcV4GhwileHYs7BPueZK_lMgMMM0NbQDzN9JYm0sOY8H0JbrL3hg8g?key=5U7lQ20IjqjO2JwN8pn5haJ-)

### **EigenLayer Integration Benefits**

Key achievements through EigenLayer integration:

- Achieved sustainable economic security without requiring massive upfront capital
- Developed a scalable verification system, striking a balance between security and performance
- Created a competitive marketplace, fueling ongoing improvements and innovation
- Fostered an open ecosystem, driving progress in both AI and verification

## **Industry Recognition**

"The Omron subnet by Inference Labs is putting Bittensor's excellent incentives mechanism on the map and making zkML applications possible, while also continuously improving the Bittensor ecosystem to be a better place," notes Tensorplex Labs, highlighting the transformative impact of this approach.

Bagel, another key player in the space, reinforces this vision: "Verifiable ML isn't just an ideal - it's essential for the future of AI. We're pleased to announce our strategic partnership with @inference\_labs, integrating advanced verification protocols into decentralized fine-tuning."

## **Conclusion**

The age of centralized control over AI is coming to an end. History has shown that open-source ecosystems consistently outperform their centralized counterparts, and Inference Labs is leading the charge for the next evolution. The future of AI will be an open, borderless marketplace—accessible to all and controlled by none.

Through EigenLayer's powerful integration, Inference Labs is establishing the gold standard for secure, decentralized AI: a protocol that empowers individuals to deploy, interact, and innovate with AI while maintaining complete privacy and sovereignty.

Just as HTTPS became the foundation for secure web traffic, Inference Labs is building the backbone for secure AI operations across decentralized networks.

**If you're intrigued by the transformative potential of AI and its decentralized future, join the** [**Inference Labs community**](https://t.me/omronai) **and** [**follow along**](https://x.com/inference_labs) **as they push the boundaries of what's possible.**

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/#/portal)

SubscribeWe built EigenDA to help rollups scale, and thus help Ethereum scale. Rollups need somewhere decentralized, scalable, and secure to post transaction data so they can scale their systems to the next billion users. To achieve this mission we intend to make it as easy as possible for the next generation of rollups to deploy on Ethereum using EigenDA.

Many new rollup projects are choosing to build on top of Optimism's [OP Stack](https://stack.optimism.io/#the-op-stack-tomorrow), which allows them to quickly launch rollups using the same software that powers Optimism. **Now that EigenDA is live on testnet, we're excited to open-source our fork of OP Stack with integrated EigenDA support.**

Rollups that use this fork can support bigger L2 block sizes and cheaper DA costs, all without affecting finality times. The result will be negligible fees and fast confirms for the end user, all without relying on a trusted third party.

**Overview of OP Stack**

The OP Stack is a collection of software components that power Optimism, the flagship rollup of OP Labs, and which can be deployed independently to run third-party rollups. The components of OP Stack fall into three categories:

1. **L1 Contracts:** Today, these contracts manage bridging to and from Ethereum. In the future, they will include logic to verify fraud proofs.
2. **Node Components:** OP Stack nodes are responsible for scaling L2 reads and verifying that the sequencer is acting honestly. They scale L2 reads by independently deriving the L2 chain state from the **DA** layer and serving read traffic. In the future, they will be able to challenge a sequencer on-chain if their derived state root does not match the state root posted on-chain by the sequencer.
3. **Sequencer Components:** The sequencer is a node that has two additional jobs: to post batches of transactions to **DA**, and to post state roots of executed transactions to Ethereum.

In our fork we made changes to the two parts of OP Stack that interact with DA.

**EigenDA Writes**

![](https://www.blog.eigenlayer.xyz/content/images/2023/12/SVG-for-Teddy.svg)

The OP Stack batcher is the part of the sequencer responsible for posting batches of L2 transaction data to DA. We updated this component to write batches to EigenDA instead of Ethereum calldata. When a batch is successfully written to EigenDA, the EigenDA disperser returns a unique blob key, which can be used to later retrieve the data that was written. The batcher then posts this blob key to Ethereum calldata, so that Ethereum remains the source of truth for the L2 ordering of EigenDA blobs.

Importantly, the OP Stack x EigenDA fork supports writing each batch to multiple EigenDA quorums, enabling redundant security and mitigating data withholding attacks.

**Node EigenDA Reads**

OP Stack full nodes derive the L2 state from transaction data posted to DA. In a loop, they download L2 transaction batches from DA and execute them. To add EigenDA support, we modified the OP Stack node to seamlessly handle retrieving EigenDA blobs using blob keys indexed in the Ethereum calldata. This involves a set of scatter-gather requests to EigenDA Operators storing chunks of the blob requested. If the retrieval of the blob from one quorum fails, the next will be tried, until the blob is retrieved or there are no more quorums to try.

**Ethereum DA Fallback**

EigenDA is new software, and we don't expect protocols to trust it blindly. If, for whatever reason, EigenDA goes down, the sequencer is designed to gracefully fall back to using Ethereum calldata for DA again. On the other side, our OP Stack nodes are smart enough to differentiate between L2 batches and EigenDA blob keys when reading inbox calldata from Ethereum.

**Limitations**

At the time of writing, OP Stack does not support fraud proofs, and neither does the EigenDA x OP Stack fork. When fraud proofs are enabled for OP Stack, we plan to update the contracts in OP Stack to support verifying the correctness and availability of L2 inbox data using KZG verification and calls to EigenDA contracts.

**Conclusion**

The integration of EigenDA with OP Stack is critical for scaling existing rollups and onboarding the next generation of users. With so many L2s building on the OP Stack today, Eigen Labs is excited to free them from the DA scaling constraints of Ethereum and build towards the vision of consistently cheap blockspace and fast confirms. Eigen Labs is committed to permissionless innovation, building alongside best-in-class projects, and supporting the most principled communities. Stay tuned for updates on the EigenDA x OP Stack Fork and other EigenDA rollup integrations.

**Next Steps**

- Review the [EigenDA OP Stack Integration documentation](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-rollup-user-guides/op-stack-+-eigenda-user-guide) for further technical explanation of the integration and instructions on how to setup the demo locally.
- Read through the code, available at: [https://github.com/Layr-Labs/optimism](https://github.com/Layr-Labs/optimism)
- To discuss how we can support and accelerate onboarding your OP Stack rollup to EigenDA, please reach out to our team via [https://contact.eigenda.xyz/](https://contact.eigenda.xyz/)
- On X/Twitter, follow [@eigen\_da](https://twitter.com/eigen_da) and [@teddyknox](https://twitter.com/teddyknox) for updates on the EigenDA x OP Stack Fork and other EigenDA rollup integrations.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/#/portal)

Subscribe[![Principles and Best Practices to Design Solidity Events in Ethereum and EVM](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2024/07/mainheader.png)](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[Solidity events are a crucial feature of Ethereum and EVM blockchains, with a vast number of use cases within the ecosystem. Primary use cases, for example, include but not limited to\\
\\
\\* Logging: where events provide a mechanism to log critical actions and state changes inside smart contract, for track contract](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)![Eigen Foundation](https://www.blog.eigenlayer.xyz/content/images/2024/07/egf.jpg)

[![Introducing Programmatic Incentives v1](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Programmatic-Incentives-v1-1.png)](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

[The Eigen Foundation is excited to announce the upcoming Programmatic Incentives v1 release, a new EigenLayer protocol feature, providing programmatic EIGEN rewards to stakers and operators for their active participation in supporting AVSs.\\
\\
What\\
\\
Programmatic Incentives v1 will enable weekly programmatic rewards of newly-minted EIGEN tokens to qualifying stakers and](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

Subscribe![EigenDA](https://www.blog.eigenlayer.xyz/content/images/2024/08/Screenshot-2024-08-14-at-10.16.11.png)

[![Introducing Updated EigenDA Pricing: Unlocking Greater Value and Accessibility](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Pricing-Update.png)](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[EigenDA’s mission is to scale the decentralized world by making reliable, scalable, and secure data availability (‘DA’) abundant. In pursuit of this goal, and enabled by the scalability of its design, EigenDA strives to be the most price-performant DA solution.\\
\\
Today, we are pleased to announce a 10x reduction](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[![Introducing EigenDA Base Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Base-Rewards-5.png)](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[Last week EigenLayer experienced a major upgrade. The Rewards protocol, the onchain system that enables AVSs to distribute rewards to their stakers and operators, went live on mainnet on August 6th. The Rewards protocol upgrade grants AVSs the ability to begin rewarding stakers and operators for their past, present, and](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[![EigenDA Dual Quorum and Production Traffic Announcement](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/Screenshot-2024-05-20-at-12.53.32-PM.png)](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[We are supporting rollup production traffic on EigenDA mainnet available immediately. Anyone can now deploy their rollup and be whitelisted on our free tier. Interested rollup customers should fill out the EigenDA Contact Form in order to be approved for the initial free-tier usage phase. Please provide the ETH address](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[![Onboarding Rollups to EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/eigenda-Blog-post---1.png)](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[Following the EigenDA mainnet launch announcement on April 9, we are pleased to announce that rollups can now onboard onto EigenDA mainnet for test traffic. We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[![With New Arbitrum Orbit Integration, EigenDA and AltLayer Bring Horizontal Scalability to the Ethereum Ecosystem](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/Untitled.png)](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[Feb 2, 2024 – EigenLabs, along with Offchain Labs and AltLayer, today announced EigenDA support for Arbitrum Orbit chains, bringing scalability to the Ethereum ecosystem without sacrificing on security. The integration offers developers the ability to build EigenDA-based Orbit rollups that bridge from Arbitrum One, Arbitrum Nova, and Ethereum, and boast](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[![Polymer x EigenDA: Open, Neutral Interoperability for Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/BlogPost-Partnership-polymer.jpg)](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[EigenDA is a data availability store made by EigenLabs and built on top of EigenLayer. Currently live on the Goerli testnet, EigenDA is on a path to launch on mainnet later this year, with support for up to 10 MB/s in write throughput. As a rollup developer, if you&](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[![Announcing EigenDA x OP Stack Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/v2--1-.png)](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[We built EigenDA to help rollups scale, and thus help Ethereum scale. Rollups need somewhere decentralized, scalable, and secure to post transaction data so they can scale their systems to the next billion users. To achieve this mission we intend to make it as easy as possible for the next](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[![Versatus x EigenDA: The First Stateless Rollup](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/BlogPost-Partnership-versatus.png)](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[EigenDA launched on testnet a few weeks ago, alongside the EigenDA Launch Partner Program featuring eight rollup infrastructure providers: AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.\\
\\
We kicked off a series of case studies on these launch partners starting with Layer N x EigenDA: A Case](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[![Layer N x EigenDA: A Case Study in Hyperscale DA for Finance](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/BlogPost-Partnership-02--2-.png)](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[Now that EigenDA is live on testnet, we're proud to announce the first participants in the EigenDA Launch Partner Program. The first eight rollup infrastructure providers who are actively working to deploy EigenDA as a data availability option for their end users include: AltLayer, Caldera, Celo, Layer N,](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/LinkedIn-cover---1-1.png)

![EigenLabs](https://www.blog.eigenlayer.xyz/content/images/2023/04/Pasted-Graphic-1.png)

[![EIGEN: The Universal Intersubjective Work Token](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Exploring-EIGEN-Banner.png)](https://www.blog.eigenlayer.xyz/eigen/)

[Over the past few years, we at Eigen Labs have been developing a platform for advancing the concept of open, verifiable digital commons. This blog post summarizes the intersubjective forking protocol enabled by the EIGEN token. We will break down the significance of EIGEN, its core ideas, its high-level implementation,](https://www.blog.eigenlayer.xyz/eigen/)

[![Unpausing Restaking Deposits on April 16th!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Ghost_05.png)](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[We're excited to announce the culmination of our phased rollout with the removal of all Liquid Staking Token (LST) caps and unpausing deposits, effective April 16th, 2024, at 9:00 AM PST. This milestone marks the next chapter in EigenLayer's journey, fostering a wide-open and dynamic](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[![EigenLayer Holesky Testnet Launch + Dual Quorum Support for EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/Twitter---22.png)](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[The EigenLayer and EigenDA Holesky Testnet is now up and running. This marks an important milestone for the EigenLayer ecosystem, and we're keen for operators, stakers and rollup developers to continue testing on Holesky as we gear up for the upcoming mainnet launch. As a reminder: points are](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[![EigenLayer Mainnet: Preparation for launch!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/x--2-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[We're thrilled to announce the next chapter in our journey towards a secure and successful EigenLayer mainnet launch! Our primary focus remains a smooth launch that prioritizes both security and performance. To achieve this, we're introducing a multi-phased approach starting now and ongoing for the next](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[![Security Bounty Competition on EigenLayer by Cantina](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/IMAGE-2024-03-07-12-54-55.jpg)](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[Cantina is a marketplace for web3 security that gives protocols the flexibility to easily book a security review with their desired team, price, and timeline. They have recently launched a competition focused on EigenLayer. This competition presents an exciting opportunity for security researchers and professionals to contribute to the safety](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Twitter-header---1-2.png)

![EigenLayer](https://www.blog.eigenlayer.xyz/content/images/2023/04/Set-A---Wordmark.png)

[![Balancing Neutrality and Decentralization in EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/EigenLayer_Ghost_balancing2_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[Today, EigenLayer has unpaused token restaking and removed TVL caps for every token. While the unpause is temporary this time, in the coming months the pause and caps will be lifted permanently. In this post, we reflect on the challenges of balancing neutrality and decentralization in the protocol, and suggest](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![Intro to EigenDA: Hyperscale Data Availability for Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/09/EigenDA_Ghost_Header_IntroToEigenDA_1200x675_01.png)](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[If you're interested in integrating your rollup with EigenDA, please fill out the EigenDA questionnaire!\\
\\
EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA will be the first actively validated service](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

Subscribe![EigenLayer Research](https://www.blog.eigenlayer.xyz/content/images/2023/10/Set-A---Wordmark.png)

[![Fragmentation to Fusion: Intention is All You Need (Part 2)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents hold the potential to transform how we interact with Ethereum, yet today, they risk creating fragmented “walled gardens” where each intent-based application exists in](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[![Intention is All You Need](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents are transforming how we think about interactions on Ethereum.\\
\\
Instead of rigid transaction paths, intents open up a world where users declare their desired](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[![Celebrating Commit-Boost’s Journey Towards Mainnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Commitboost.png)](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[Eigen Labs is excited to announce that Commit-Boost, an open-source public good that we have proudly supported since its inception, is moving towards audit and production on the Ethereum mainnet. This milestone, and the community now behind it, marks a significant step forward in ensuring the healthy growth of the](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[![Intelligent DeFi](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Intelligent-DeFi-B.png)](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[Thanks to Karthik, Ismael from Lagrange, Nikhil from Aethos, Soubhik, Gautham, and Brandon for reviewing drafts of this piece.\\
\\
Ethereum gave birth to DeFi with the launch of Maker in December 2017. Uniswap and Compound launched soon after, forming an economy around ETH and ERC20s. Since then, we’ve witnessed](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[![Request for AVS: Uniswap v4 Hooks](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/1200x630.png)](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[Occasionally, you hear an idea so exciting that you see the future with utter clarity. At Eigen Labs, we’re excited to work with exceptional builders and hear their special insights about the world on a daily basis. We want to share some of ours with you. We’re starting](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[![EigenLayer AVS Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/medium-banner_option4.png)](https://www.blog.eigenlayer.xyz/avs-launch/)

[EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs\\
\\
The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s MACH restaked rollups with Xterio being](https://www.blog.eigenlayer.xyz/avs-launch/)

[![Fhenix: FHE Coprocessor on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/image1.png)](https://www.blog.eigenlayer.xyz/fhenix/)

[Fhenix and EigenLayer Join Forces to Pioneer FHE Coprocessors, Revolutionizing Onchain Confidentiality on Ethereum\\
\\
We are excited that FHE Coprocessor will be building on EigenLayer and to announce the development of FHE-based coprocessors in collaboration with Fhenix.\\
\\
FHE coprocessors are secured by Fhenix’s optimistic FHE rollup infrastructure and EigenLayer’](https://www.blog.eigenlayer.xyz/fhenix/)

[![Ethos: Powering the Convergence Era of Blockchains](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/ethosxeigen.png)](https://www.blog.eigenlayer.xyz/ethos/)

[Blockchains are entering an era of convergence, characterized by the dissolution of boundaries between different network architectures. Cosmos started as a network of appchains, with each chain independently establishing its validator set and trust protocol using native tokens. Ethereum, on the other hand, chose a shared security approach where every](https://www.blog.eigenlayer.xyz/ethos/)

[![Ritual ♾️ EigenLayer: AI × Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[Summary\\
\\
The worlds of artificial intelligence and onchain protocols are increasingly intersecting as permissionless protocols look to unlock new customer behavior around ownership and markets by utilizing AI models. We’ve witnessed how in offchain settings, AI models could dramatically improve on the status quo of problem solving across various](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[![Inco: Building an Universal Confidential Computing L1 on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/----5-.png)](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[One of the open challenges in the blockchain industry is achieving trustless confidentiality. The inherent transparent nature of public blockchains prevents the development of applications requiring on-chain confidentiality across gaming, decentralized finance (DeFi), governance, and identity without relying on a trusted third party.\\
\\
Current approaches to providing privacy on the](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[![Cosmos. Ethereum. EigenLayer.](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Screenshot-2024-01-09-at-7.49.43-AM.png)](https://www.blog.eigenlayer.xyz/cosmos/)

[By connecting Ethereum and Cosmos, EigenLayer will bring in a new wave of innovations.](https://www.blog.eigenlayer.xyz/cosmos/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Dual Staking: secure a PoS network with two tokens](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/dualstaking.png)](https://www.blog.eigenlayer.xyz/dual-staking/)

[One of the most powerful features of EigenLayer is the notion of dual staking. In this article, we will discuss what dual staking is, how it increases the robustness and decentralization of any PoS network, how it mitigates the death spiral problem with network token volatility, and how these networks](https://www.blog.eigenlayer.xyz/dual-staking/)

[![You Could've Invented EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/i-made-eigenlayer-copy-2.jpg)](https://www.blog.eigenlayer.xyz/ycie/)

[In this blog post, we will take you through the evolution of the protocol, by covering how EigenLayer's architecture emerged from the initial concept.](https://www.blog.eigenlayer.xyz/ycie/)

[![Announcing EigenLayer Research Fellowship](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/ERFwhite.png)](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[Update: The EigenLayer Research Fellowship (ERF) structure has been updated on October 25th, 2023, particularly in selection criteria and program dates.\\
\\
We believe EigenLayer is about to kickstart a new era of secure innovative infrastructure applications that will transform how blockchain applications operate. We’ve been diving deep into these](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[![The EigenLayer Universe: Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/Eigen-Universe-3.png)](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[EigenLayer empowers builders to develop innovative distributed systems without worrying about how to build the underlying trust networks for these systems. We call these distributed systems AVSs - actively validated services. We have categorized AVSs into 5 types:\\
\\
1\. Rollup Services: augmenting the Ethereum rollup ecosystem with services that inherit](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[![The Three Pillars of Programmable Trust: The EigenLayer End Game](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/image--1--1.png)](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[Thanks to Alex Obadia, Austin Griffith, Dan Elitzer, David Phelps, Jon Charbonneau, Soham Zemse, and Waylon Jepsen from the community for reviewing and giving feedback. \\
\\
Today, if any developer wants to build a smart contract protocol such as a DEX, a lending protocol, etc., on Ethereum, they can inherit security](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[![Censorship Resistance with Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_Liveness-firstRelay_1200x675_01.png)](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[MEV-Boost protects Ethereum’s validator set from Maximum Extractable Value’s (MEV) centralization pressures. It is an open-source project designed by the Flashbots team. MEV-Boost’s relays simplify the interaction between builders and proposers in this system, streamlining the system and eliminating the need for complex cryptography.\\
\\
There are however](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Twitter-header---1-2.png)

![EigenLayer](https://www.blog.eigenlayer.xyz/content/images/2023/04/Set-A---Wordmark.png)

[![Slashing on Mainnet is Coming Soon - What You Need to Know](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog-Header_slashing_mainnet.jpg)](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[Slashing goes live on EigenLayer mainnet April 17th. Get essential details on what slashing is, the implementation timeline, key reminders for all users, and specific guidance for AVSs, Operators, and Stakers in this comprehensive update.](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[![Intro to Slashing on EigenLayer: AVS Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_AVS-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[EigenLayer's slashing on mainnet arrives April 17th, enabling AVSs to set custom conditions, manage operators through Operator Sets, and implement Unique Stake Allocation—creating stronger security guarantees with targeted accountability.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[![Intro to Slashing on EigenLayer: Stakers' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_stakers-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[Slashing arrives on EigenLayer mainnet April 17th. Learn how it impacts Stakers through opt-in requirements, allocation timelines, and monitoring responsibilities—plus why this feature completion enhances the risk/reward ecosystem for delegators.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[![Intro to Slashing on EigenLayer: Operators' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_operators-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[EigenLayer's slashing launches April 17th, giving Operators control through opt-in participation, Operator Sets, and Unique Stake Allocation—isolating risks while creating new reward opportunities aligned with your specific risk profile.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[![The Future of EigenLayer Testing: New & Improved Testnets & Tooling Coming Soon](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[Eigen Labs expands EigenLayer protocol testing capabilities with multiple new testnets and developer tools. Explore Hoodi deployment, improved CLI, Self Slasher AVS, and local devnet toolchains—empowering AVS builders with robust, versatile environments for seamless integration.](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[![AI Beyond the Black Box: Inference Labs is Making Verifiable, Decentralized AI a Reality with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/AVS_Spotlight_Inference_BlogPost-Header.jpg)](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[Inference Labs is making AI verifiable on-chain with their Zero-Knowledge Verified Inference Network. By leveraging EigenLayer's security, they've created a system that makes dishonest behavior unprofitable while reducing proving times by 76%.](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[![EigenLayer Update: Holesky Network Instability and Upcoming Sepolia Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--3-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[Due to Holesky testnet instability following Ethereum's Pectra Upgrade, EigenLayer will launch on Sepolia Network (March 10) while maintaining Holesky support. EigenDA targeting Sepolia availability by late April.](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[![The Trust Layer for AI Agents: How Ungate Wukong Leverages EigenLayer for Trust-Minimized Autonomous Agents](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/AVS_Spotlight_Ungate_BlogPost-Header.png)](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[What if you could know with certainty that an AI is acting independently? Ungate and EigenLayer build the trust layer for AI, where autonomous agents don't just claim independence—they prove it cryptographically through verifiable execution.](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[![Redefining AVS: From Actively Validated to Autonomous Verifiable Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Header.png)](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[EigenLayer redefines AVS as Autonomous Verifiable Services, reflecting a powerful shift from how these services are validated to what they truly represent: systems that are self-sustaining, verifiable, and built for decentralized ecosystems.](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[![Scaling to $1B in Delegated Assets: How Pier Two Unlocked Growth with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[From early launch partner to securing over $1B in delegated ETH and EIGEN, Pier Two's success demonstrates how institutional operators can scale efficiently with EigenLayer's restaking framework to build market-leading positions.](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[![EigenLayer at ETHDenver 2025: Complete Event Guide](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/EigenLayer-at-ETHDenver_Blog-Header.png)](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[Your comprehensive guide to all things Eigen at ETHDenver 2025](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[![Introducing Verifiable Agents on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient.png)](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[The “Level 1 Agent” is a standardized way for agents to integrate with verifiable tools by using AVSs.](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[![EigenLayer 2024 Year in Review: Building the Future of Open Innovation](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/01/EigenLayer-2024-Year-in-Review.png)](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[2024 was a monumental year for EigenLayer.\\
\\
Since launching our first staking contracts in June 2023, and following the full mainnet deployment of the EigenLayer protocol in April 2024, we’ve made great strides toward our vision of building coordination engines for driving open innovation. \\
\\
We launched the EigenLayer protocol,](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[![AVS Spotlight: Hyperbolic](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/AVS_Spotlight_Twitter-Thread-Header.jpg)](https://www.blog.eigenlayer.xyz/avs-spotlight-hyperbolic/)

[Proof of Sampling by Hyperbolic: How EigenLayer Leverages PoSP, the Gold Standard for Verification\\
\\
Overview\\
\\
One of the biggest challenges in decentralized AI is ensuring reliable, efficient, and scalable validation. Hyperbolic, a decentralized AI infrastructure platform, has developed Proof of Sampling (PoSP) to address this issue. PoSP, created in collaboration](https://www.blog.eigenlayer.xyz/avs-spotlight-hyperbolic/)

[![Introducing: Slashing](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Slashing---Testnet.png)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[We are excited to introduce Slashing, a proposed protocol upgrade detailed in ELIP-002: Slashing, and the next significant step in the evolution of the EigenLayer protocol. This powerful upgrade introduces slashing, a critical tool for AVSs to enforce cryptoeconomic commitments.\\
\\
This proposed upgrade is the second EigenLayer Improvement Proposal (ELIP)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[![Introducing: Rewards v2](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Rewards-v2-2.png)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[We are excited to announce the arrival of the Rewards v2 protocol upgrade on Mainnet. It is designed to bring greater flexibility, efficiency, and customization to rewards within the EigenLayer ecosystem. This upgrade is also the first EigenLayer Improvement Proposal (ELIP), ELIP-001: Rewards v2, using the EigenLayer Governance process (EigenGov)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[![EigenLayer x LayerZero: The CryptoEconomic DVN Framework](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/LAYER-ZERO.png)](https://www.blog.eigenlayer.xyz/dvn/)

[Eigen Labs, in partnership with LayerZero Labs, is introducing a framework for CryptoEconomic Decentralized Verifier Networks (DVNs). Eigen Labs chose LayerZero because it is one of the most battle-tested protocols in crypto, handling millions of messages and securing billions for apps.\\
\\
As the first step of this integration, we are](https://www.blog.eigenlayer.xyz/dvn/)

[![Introducing the EigenLayer Security Model: A Novel Approach to Operating and Securing Decentralized Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Security-Model-1.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[Eigen Labs is excited to introduce the EigenLayer Security Model, an evolution of our previous model, in preparation for the upcoming ‘Slashing’ protocol feature release.\\
\\
This blog post is a simple description of how EigenLayer empowers AVSs to operate more efficiently and create economically aligned incentives using three foundational concepts:](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[![Introducing the EigenPod Upgrade](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Introducing-EigenPod-Upgrade.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[Eigen Labs is pleased to announce the EigenPod upgrade, a major update to the EigenLayer protocol, making native ETH restaking on EigenLayer easier and more rewarding.\\
\\
What\\
\\
The EigenPod upgrade introduces a novel balance checkpointing system for managing Ethereum validator and EigenPod balances.\\
\\
When\\
\\
All EigenPods on mainnet have been](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[![Community Update: Airdrops and the EigenLayer Ecosystem Network](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/communityupdate.png)](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[CoinDesk published an article on airdrops in our ecosystem and Eigen Labs employee participation in those airdrops. We’ve written a community update on what happened, why it happened, and steps we’ve taken. \\
\\
We want to make clear that we have no knowledge or evidence of any employee at](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[![Coming Soon: Permissionless Token Support on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/comingsoonpermissionless.png)](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[Eigen Labs is proud to introduce Permissionless Token Support, an upcoming update, to the EigenLayer protocol. This feature will enable any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[![Coming Soon: AVS Rewards and EIGEN Programmatic Incentives](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/b-1.png)](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[Summary\\
\\
\\* Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.\\
\\* At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.\\
\\* These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[![Holesky Launch: AVSs Can Now Test Restaker and Operator Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/rewards-claim5-630bdf4627a2efba655108a1f5648581.png)](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[We're proud to announce the launch of the MVP experience for Rewards on the Holesky testnet. This MVP allows AVSs to begin integrating with the rewards system, and test setting up incentives for restakers and node operators. It also allows restakers to test claiming functionality for (worthless) testnet](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[![Mainnet Launch Announcement: EigenLayer ∞ EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/lightbox_launch_0409-3.jpeg)](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[The Start of Infinite Sum Games\\
\\
Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[![On Liquid Restaking: Risks & Considerations](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/2024-03-21-13.07.06.jpg)](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[In recent months, liquid restaking protocols and liquid restaking tokens (LRTs) have seen tremendous growth, and are beginning to become systemically important in the EigenLayer ecosystem. There are many different flavors of LRT, each with different values, features, and offerings. All of these LRT projects are independent of the underlying](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[![Balancing Neutrality and Decentralization in EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/EigenLayer_Ghost_balancing2_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[Today, EigenLayer has unpaused token restaking and removed TVL caps for every token. While the unpause is temporary this time, in the coming months the pause and caps will be lifted permanently. In this post, we reflect on the challenges of balancing neutrality and decentralization in the protocol, and suggest](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![Intro to EigenDA: Hyperscale Data Availability for Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/09/EigenDA_Ghost_Header_IntroToEigenDA_1200x675_01.png)](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[If you're interested in integrating your rollup with EigenDA, please fill out the EigenDA questionnaire!\\
\\
EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA will be the first actively validated service](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

Subscribe![Nader Dabit](https://www.blog.eigenlayer.xyz/content/images/2024/04/headshotzoomedout.png)

[![Accelerate Rollup Deployment with EigenDA's RaaS Marketplace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/b2--1-.png)](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[Teams are launching high performance, low cost rollups and rollup services on EigenDA. In this post we’ll take a look at some of them and learn how you can get started building your own rollup.\\
\\
Rollups as a Service (RaaS) provide everything you need to build, customize, and deploy](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

Subscribe### EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs

The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s [MACH](https://docs.altlayer.io/altlayer-documentation/altlayer-facilitated-actively-validated-services/overview) restaked rollups with Xterio being the first restaked rollup showcase, Brevis’ [Coprocessor](https://docs.brevis.network/), Eoracle's [Ethereum native oracle](https://eoracle.gitbook.io/eoracle), Lagrange’s [State Committee](https://lagrange-labs.gitbook.io/lagrange-v2-1), Witness Chain’s [DePIN Coordination Layer](https://docs.witnesschain.com/) – now live and ready to be delegated and run by EigenLayer stakers and operators.

**What are AVSs:**

AVSs are actively validated services - middleware, services, chains, networks, and PoS systems - secured by EigenLayer restakers and run by EigenLayer operators.

To build an AVS: [https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-developer-guide](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-developer-guide)

**Meet the new category-expanding AVSs:**

Stage 3 empowers developers to unlock exciting new functionalities through a diverse range of AVSs:

**Rollup Acceleration:** Lagrange, MACH AVS, and Witness Chain’s Watchtower specifically cater to rollup needs. Lagrange’s State Committee offers secure light client functionality which facilitates faster and more secure communication between optimistic rollups and other blockchains. MACH AVS revolutionizes rollup finality by achieving blazing-fast block confirmation speeds (under 10 seconds), significantly improving the user experience on Ethereum. Witness Chain’s Watchtower enables incentive-compatible and crypto-economically-secure Proof-of-Diligence (PoD) to make sure watchtowers are working on the happy path for optimistic rollups. This empowers bridges and exchanges operating on rollups to function with enhanced smoothness and security.

**Real-World Data Integration:** Eoracle, the first Ethereum-native oracle network built on EigenLayer, similarly serves a broad range of blockchain applications. It provides a modular and programmable solution for integrating real-world data directly into applications. This empowers developers to create innovative use cases that leverage the power of decentralized applications with the critical insights gleaned from the real world.

**Coprocessors**: Where rollups offer cheaper and faster execution than Ethereum L1 while retaining state, coprocessors shine by acting as the stateless counterpart that offers cheap/fast compute. This class of compute is quickly gaining adoption across the space where the compute input is fully specified onchain (imagine needing to compute over some history of the chain) and the state change (the compute) also occurs onchain. EigenLayer enables cryptoeconimcally-bonded coprocessors to exist such that with a large enough economic bond on execution, others are permissionlessly allowed to secure compensation for erroneous computation. This of course requires fraud proofs for slashing similar to optimistic systems. Brevis is at the forefront of launching this category on EigenLayer.

Below we highlight some of the specifics of how each of these AVSs function.

### AltLayer’s MACH AVS

AltLayer MACH is a fast finality AVS for OP rollups. It leverages EigenLayer’s restaking mechanism to build a decentralized network that validates OP Mainnet rollups. Block validation is done by verifying that a new rollup state is indeed the result of applying the state transition function on an ordered set of transactions proposed by the sequencer and a previously valid rollup state. When an invalid block is detected, MACH operators will raise an alert, and if enough operators agree on the alert, then it can be pushed upstream to interested clients such as dApps. Upon receiving the alert, a client (that subscribes to MACH's services) can act upon it to ensure that the invalid state update is rejected as early as possible.

Unlike Ethereum, which takes about 12 minutes to finalize transactions, or the OP Mainnet sequencer that gives soft-confirmations that lack economic backing, MACH which is built as an overlay network on top of Ethereum can give soft-confirmations with economic backing via restaking.

As a fast finality AVS, MACH offers the following core services to end users on OP rollups:

1. Faster confirmation for rollup transactions,
2. Crypto-economic security to detect any malicious network participants,
3. Decentralized validation of rollup states.

Users, dApps and other services can avail MACH’s services through an RPC endpoint that reports if a certain rollup block is considered final. This RPC endpoint can be integrated into any dApp running on OP rollups and would allow for faster and more reliable transaction confirmations. By tapping into the economic guarantees provided by MACH, dApps can safely update the contract state in the UIs and frontends allowing them to offer a better and more reliable UX.

MACH will also be helpful for exchanges, bridges, light-weight explorers and other ancillary services that need streamed access to rollup states and blocks to seamlessly offer their products and services. By tapping into MACH, these providers can offer a faster service to the end users. For example, an exchange or a liquidity-based bridge may not need to wait for 7 days to allow a customer to withdraw their assets from OP rollups to Ethereum.

**Become an AltLayer (MACH) Operator:** Fill out the [**Intake Form**](https://forms.gle/Wh1AwGJQVBcAPzAm7).

### Brevis’ ZK Coprocessor

Brevis is a smart Zero-Knowledge (ZK) Coprocessor that empowers smart contracts to read from and utilize the full historical on-chain data from any chain, and run customizable computations in a completely trust-free way. Brevis enables exciting new use cases such as [data-driven DeFi](https://twitter.com/UniswapFND/status/1719760883965149501), trust-free Active Liquidity Management solutions, autonomous intent framework, ZK Reputation, dApp [UX personalization](https://twitter.com/TrustaLabs/status/1768114687089295770), ZK Bridge and more.

Initially, Brevis operated only a "pure-ZK" model, wherein ZK proofs are always generated upfront and verified on-chain before coprocessing results can be used. While the pure-ZK model provides simplicity and a trustless framework, many dApps seek a more flexible solution that can be cost-effective across different scenarios based on the value and time sensitivity of a request, while still ensuring security through robust ZK guarantees.

To meet this demand, Brevis introduced the coChain AVS powered by EigenLayer’s restaking quorums, marking the beginning of a novel ZK Coprocessor architecture that **combines the strengths of crypto-economic security and ZK fraud proof**. Upon receiving a request, the Brevis coChain first produces crypto-economically secured coprocessing "proposals", which are open to "challenges" through ZK fraud proofs. In the majority of cases where challenges are not initiated, the results can be used directly in smart contracts without incurring steep costs associated with ZK proof generation and verification. Beyond significant cost savings, the introduction of the coChain AVS also supports essential features like proof of non-existence and proof of completeness, which were difficult to attain in a pure-ZK framework.

**Become a Brevis Operator:** Fill out the [**Intake Form**](https://form.typeform.com/to/M4LT46SY).

### Eoracle

Addressing one of the largest markets in the blockchain industry, eoracle stands as the Actively Validated Oracle network. Designed as a modular and programmable data layer, eoracle is backed by restaked ETH and the decentralized network of Ethereum validators.

This approach revolutionizes the oracle space by extending Ethereum's core principles: permissionless participation, decentralized ownership and cryptoeconomic security to establish the next generation oracle network via a credibly neutral marketplace for data and computation across all Ethereum rollups.

**Become an Eoracle Operator:** Fill out the [**Intake Form**](https://t.me/+3ytUXL3cz9ZjNmVk).

### Lagrange State Committees

Lagrange Labs has been continually testing the Lagrange State Committee light client with EigenLayer and its ecosystem, and it’s exciting to see them be among the first to launch as a Zero Knowledge (ZK) AVS on EigenLayer mainnet. Lagrange State Committees are launching with a decentralized network of 15+ operators that are each independently secured by natively restaked ETH delegations from major liquid restaking (LRT) protocols in the space.

The Lagrange’s State Committee network is a ZK light client for optimistic rollups that settles on Ethereum, powered by Lagrange’s ZK Coprocessor. By combining Lagrange State Committees with EigenLayer, we are creating a zone of shared security that any interoperability protocol or dApp can leverage, without incurring high operational overhead. Compared to other cross-chain interoperability solutions, such as permissioned validation or bonded staking/slashing, which come with issues related to risk stacking, low latency and price volatility, State Committees and restaking through EigenLayer offer an optimal balance of **robust shared security, trust minimization and decentralization along with cost efficiency**.

**How Lagrange State Committees Work**

Each Lagrange State Committee consists of a group of client nodes that have restaked on Ethereum via EigenLayer. Each Lagrange State Committee node independently attests to the finality of the state of an optimistic rollup once the transactions are finalized on a Data Availability (DA) layer. Lagrange’s ZK Coprocessor can then be used to generate a ZK state proof, which applications can treat as the source of truth for the state of the given optimistic rollup. While inspired by Ethereum’s Sync Committee, a Lagrange State Committee is not limited by the number of nodes, so it offers **super-linear security** by dynamically scaling the number of nodes verifying the state of an optimistic rollup as capital grows. They are also chain-agnostic, so can be added onto existing interoperability protocols as an additional layer of security.

The Lagrange and EigenLayer integration is particularly impactful, as operators are faced with reduced opportunity costs due to the restaked collateral, interoperability protocols have access to greater economic security, and dApp users experience lower fees and stronger security guarantees compared to if a cross-chain interoperability protocol were to rely on other security mechanisms or build their own.

**State Committee + Other ZK Building Blocks**

Lagrange State Committees are fueled by various building blocks within Lagrange’s tech stack. For example, Lagrange’s [Reckle Trees](https://lagrange.dev/reckle-trees) and [ZK Coprocessor](https://medium.com/@lagrangelabs/a-big-data-primer-introducing-zk-mapreduce-12cf404eab75) are used for efficient aggregation of public key (APK) proofs, which encourages AVS participation by keeping computational costs of verifying attestations from committee nodes low. This is also how Lagrange is able to support super-linear economic security, whereby an economic attack on the protocol requires collusion of the entire large set of nodes, as opposed to a smaller subset.

State Committees can also be further combined with Lagrange’s ZK Coprocessor and Verifiable Database to achieve efficient, scalable and inexpensive computations of on-chain data via an off-chain verifiable database. As an example, a multi-chain lending application developer could leverage Lagrange’s ZK Coprocessor plus the State Committee light client to efficiently and cheaply calculate the total amount of collateral deposited by a user across different chains. Other ways cross-chain interoperability protocols can leverage Lagrange’s ZK building blocks can be found [here](https://medium.com/@lagrangelabs/scaling-programmable-trust-on-eigenlayer-with-lagranges-state-committees-and-big-data-coprocessor-ec56a7562652).

To learn more about Lagrange’s State Committees and other ZK solutions, check out this [Lagrange blog](https://medium.com/@lagrangelabs/lagrange-launches-as-first-zero-knowledge-actively-validated-service-avs-on-eigenlayer-mainnet-7e4dfe1ecb13).

**Become a Lagrange Operator:** Fill out the [**Intake Form**](https://forms.gle/ihK4vbFaEXeBsH9e6) **.**

### Witness Chain’s DEPIN Coordination Layer AVS

Witness Chain is the coordination layer to unify DePIN economies. EigenLayer infrastructure can now be utilized to achieve consensus on physical state - evolving DePINs to a new era.

Witness Chain’s DePIN Coordination Layer unlocks a shared economy of integrated physical assets - enabling new economic instruments to be built on top of DePINs.

Foundational proof systems, including Proof-of-Location and Proof-of-Bandwidth, introduce decentralized verification to essential attributes of physical networks.

The DePIN Coordination Layer enables:

- additional security with access to dual-staking
- platform for DApp builders to build DeFi for physical assets
- discoverability and composability with other DePINs

The Rollup Watchtower Network is now live as Witness Chain’s in-house DePIN. This network is the first line of defense for rollups, ensuring that operators actively validate transactions on a Rollup and detect any fraudulent execution. This network implements the Proof-of-Diligence (PoD) protocol, which requires watchtowers to continuously provide proof that they have verified L2 assertions and get rewarded for the same. Proof of Diligence protocol includes a carefully-designed incentive mechanism that is provably secure when watchtowers are rational actors, under a mild rational independence assumption. Learn more about the specifics of this protocol in [official docs](https://docs.witnesschain.com/diligence-watchtowers/watchtower-protocol-version-2/how-it-works) and in the [PoD research paper](https://arxiv.org/abs/2402.07241).

Witness Chain has a vibrant ecosystem of 20+ DePINs and rollups that will benefit from validation of their state. This validation is provided by robust crypto-economic security by a strong ecosystem of EL Operators.

**Become a Witness Chain Operator:** Fill out the [**Intake Form**](https://discord.gg/ZwdnTBAtqV) **.**

### Xterio’s MACH AVS

Similar to AltLayer MACH, Xterio MACH is a fast finality AVS that serves the [Xterio](https://xter.io/) rollup – a gaming tailored OP Stack rollup. Xterio MACH is therefore an instance of an AVS set up for an app-specific rollup. Xterio’s MACH AVS will validate rollup blocks produced by the Xterio chain and serve as its fast finality layer.

For context, [Xterio](https://xter.io/) is a leading Web3 game publisher and platform with five AAA games and over 45 gaming partners. Xterio has amassed a gaming community of over two million users worldwide and has developed the most successful gaming NFT launchpad.

**Become a Xterio Operator:** Fill out the [**Intake Form**](https://forms.gle/Wh1AwGJQVBcAPzAm7).

### Going Forward

**Phased rollout of AVSs**

While these projects and the community are excited to kick off this open innovation journey and unlock new possibilities, they are doing so progressively and with caution. As such, they have chosen to initially work more closely with a select group of operators they are onboarding to retain high confidence in their services charting new territory. As these services stabilize, each team will look to expand their operator set with the aim of the final stage being completely open for anyone to participate. In the interest of safety we'll get there progressively - but the end goal is always _permissionless innovation_.

**A Growing Ecosystem**

The introduction of these AVSs signifies a crucial step forward in fostering a vibrant and innovative EigenLayer ecosystem. By offering a diverse range of solutions, Stage 3 empowers developers to build a wider array of blockchain applications. As the ecosystem matures, we encourage developers to ideate on the use cases for restaking alongside operator partners in a proactive and iterative onboarding process to push the boundaries of permissionless open innovation.

**The Future of EigenLayer**

We have exciting plans for the future of EigenLayer, and we're actively working on introducing even more groundbreaking features and AVSs.

**Build Infinite Sum Games**

As we embark on this exciting new chapter with the EigenLayer Mainnet, we look forward to your continued involvement and feedback. Together, we will maximize open innovation. Join us in this infinite sum game!

Visit the links below to learn more about and follow each project.

AltLayer/Xterio: [Altlayer site](https://altlayer.io/), [Altlayer X](https://twitter.com/alt_layer), [Xterio site](https://xter.io/), [Xterio X](https://twitter.com/XterioGames), [overview](https://docs.altlayer.io/altlayer-documentation/altlayer-facilitated-actively-validated-services/overview), [operator onboarding doc](https://alt-research.notion.site/AltLayer-MACH-Operator-Onboarding-Guide-ed00bc7f43c647b3afd085d95fd67792), [repo](https://github.com/alt-research/mach-avs/), [operator intake form](https://forms.gle/Wh1AwGJQVBcAPzAm7)

Brevis: [site](https://brevis.network/), [X](https://twitter.com/brevis_zk), [overview](https://docs.brevis.network/), [operator onboarding doc](https://github.com/brevis-network/brevis-avs), [repo](https://github.com/brevis-network/brevis-contracts-avs), [operator intake form](https://form.typeform.com/to/M4LT46SY)

Eoracle: [site](https://www.eoracle.io/), [X](https://twitter.com/eoracle_network), [overview](https://eoracle.gitbook.io/eoracle), [operator onboarding doc](https://eoracle.gitbook.io/eoracle/eoracle-operator-guide), [contracts](https://github.com/Eoracle/eoracle-middleware), [cli](https://github.com/Eoracle/Eoracle-operator-cli), and join [Telegram](https://t.me/+3ytUXL3cz9ZjNmVk) to be an Eoracle operator

Lagrange: [site](https://www.lagrange.dev/), [X](https://twitter.com/lagrangedev), [overview](https://lagrange-labs.gitbook.io/lagrange-v2-1), [operator onboarding doc](https://github.com/Lagrange-Labs/client-cli), [contracts](https://github.com/Lagrange-Labs/lagrange-contracts), [operator intake form](https://forms.gle/ihK4vbFaEXeBsH9e6)

Witness Chain: [site](https://www.witnesschain.com/), [X](https://twitter.com/witnesschain), [overview](https://docs.witnesschain.com/diligence-watchtowers/introduction), [operator onboarding doc](https://docs.witnesschain.com/diligence-watchtowers/for-the-node-operators/watchtower-setup), [repo](https://github.com/witnesschain-com/diligencewatchtower-contracts), [architecture](https://docs.witnesschain.com/diligence-watchtowers/watchtower-protocol-version-2/watchtower-architecture), and join [Discord](https://discord.gg/ZwdnTBAtqV) to be a Witness Chain operator

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/avs-launch/#/portal)**Proof of Sampling by Hyperbolic: How EigenLayer Leverages PoSP, the Gold Standard for Verification**

## **Overview**

One of the biggest challenges in decentralized AI is ensuring reliable, efficient, and scalable validation. [Hyperbolic](https://www.hyperbolic.xyz/), a decentralized AI infrastructure platform, has developed **Proof of Sampling (PoSP)** to address this issue. PoSP, created in collaboration with UC Berkeley and Columbia University, offers a mechanism that is 100–1000 times more cost-efficient than zkML and more secure than opML. PoSP is versatile and can validate a variety of services in AI but can also be applied to many other areas in decentralized networks which may value scalability and security.

EigenLayer is integrating PoSP to its restaking protocol to secure Actively Validated Services (AVS). This article explores how EigenLayer leverages PoSP to enhance AVS restaking.

## **The Opportunity**

As decentralized AI services scale, ensuring continuous and trustworthy validation without centralized oversight becomes critical. This post focuses on how Hyperbolic helps decentralized AI services address the challenges of trust and scalability. By leveraging its Proof of Secure Processing (PoSP), Hyperbolic resolves the limitations in traditional methods like optimistic fraud proofs (opML) and zero-knowledge proofs (zkML) offering a scalable, secure solution for consistent service validation.

Hyperbolic identified the need for a scalable, secure, and efficient verification mechanism. The result was Proof of Sampling (PoSP), which achieves continuous service validation through randomized audits and game-theoretic incentives. PoSP operates under [Nash Equilibrium principles](https://www.investopedia.com/terms/n/nash-equilibrium.asp), making dishonesty economically irrational for validators. This innovation aligns seamlessly with EigenLayer’s restaking protocol.

## **What is PoSP?**

**Proof of Sampling (PoSP)** is a verification protocol designed to enhance decentralized systems by combining:

1. **Random Sampling**: Validators are randomly selected to perform audits, reducing computational load.
2. **Incentive Structures**: Honest validators are rewarded, while fraudulent ones are penalized, ensuring rational behavior under Nash Equilibrium.
3. **Minimal Overhead**: PoSP achieves high efficiency, adding only 0.735% computational cost.

This makes PoSP ideal for scaling decentralized services while maintaining trust and security.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcaNONMTeblOW1kV7_fhYJm46lU1DO3dT8zCqpyzdQwX87gzF3yrrbZ89RNL2GkQ5ogztR9m7exjtUo3WeK5vMWhThcxvtKzzvzlJC--5u7oYOiALfgFC2M8V8cZO6XDR4?key=zscmu2Ncxay0uQoCVqGAM9Cp)

## **Integration with EigenLayer**

EigenLayer extends Ethereum’s cryptoeconomic security by allowing stakers to secure additional services, such as AVS, through restaking.

By integrating PoSP with EigenLayer, Hyperbolic is able to achieve:

- **Randomized Validation**: Validators are selected randomly to verify outputs, ensuring unbiased results.
- **Scalable AVS Support**: PoSP reduces computational demands, allowing EigenLayer to secure large-scale services efficiently.
- **Fraud Deterrence**: Strict penalties make dishonesty unprofitable, while honest behavior remains the optimal strategy.

This integration enables Hyperbolic to provide scalable and trustworthy AI services through EigenLayer's infrastructure while laying the groundwork for other AVS use cases.

## **The Results**

The integration of EigenLayer and PoSP has delivered significant benefits to decentralized AI validation:

- **Enhanced Security**: Randomized audits drastically reduced the risk of fraud and ensured validator honesty.
- **Operational Efficiency**: PoSP added less than 1% computational overhead, maintaining high performance.
- **Improved Scalability**: Hyperbolic efficiently scaled its decentralized AI services to meet growing demand.
- **Cost Savings**: The solution achieved a 75% cost reduction compared to centralized alternatives.
- **Competitive Advantage**: Hyperbolic’s platform now rivals centralized solutions in performance while offering the security of decentralization.

> _"The EigenLayer protocol combined with our Proof of Sampling protocol fundamentally transforms how we secure decentralized services. We now offer scalable, reliable, and fraud-resistant infrastructure at a fraction of the cost."_

_Jasper Zhang, CEO of Hyperbolic_

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcp3uAMZao_VPV4k_zXtvUXjm1ANd7KKDqAY415-0twVtrh3XsA38n6YVeI-M6pDRB5kKyqrczCaquOYARLsBIJIWe4oleUTP1DQXQ2zfI9FYaf268AgixOF6mcRr9Ulc0?key=zscmu2Ncxay0uQoCVqGAM9Cp)

## **Applications of PoSP in AVS Restaking**

Looking ahead, Hyperbolic plans to introduce a generalized PoSP AVS framework to support other AVS systems built on EigenLayer, further expanding its impact on decentralized services. PoSP has broad applications across decentralized services secured by EigenLayer, including:

- **AI Inference Validation**: Ensures high-quality, trustworthy results in decentralized AI computations.
- **AI Training and Data Validation**: Guarantees integrity in training datasets and inference outputs.
- **Data Availability and Rollups**: Secures Layer 2 transactions by validating data availability with minimal overhead.
- **Cross-Chain Operations**: Enhances the security of bridges by ensuring reliable transaction verification.

## **Conclusion**

Hyperbolic and EigenLayer’s collaboration addresses the critical challenges of securing decentralized services. By integrating Proof of Sampling with EigenLayer’s restaking protocol, Hyperbolic has  achieved a scalable, secure, and efficient solution for Actively Validated Services. PoSP has set a new standard for decentralized validation, delivering enhanced security and trust with minimal computational cost.

[Learn more](https://www.hyperbolic.xyz/about) about Hyperbolic and PoSP, and follow along on [X](https://x.com/hyperbolic_labs).

Schedule a [demo](https://calendly.com/jeremhazan/hyperbolic-sales-demo?month=2024-12) or explore the [documentation](https://docs.hyperbolic.xyz/docs/getting-started) to see how PoSP is a gamechanger for decentralized AI services.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/avs-spotlight-hyperbolic/#/portal)

SubscribeToday, EigenLayer has unpaused token [restaking](https://app.eigenlayer.xyz/) and removed TVL caps for every token. While the unpause is temporary this time, in the coming months the pause and caps will be lifted permanently. In this post, we reflect on the challenges of balancing neutrality and decentralization in the protocol, and suggest a policy for consideration by the protocol community.

The EigenLayer community needs to find a fine balance between two principles of the EigenLayer protocol: Neutrality and Decentralization.

**Neutrality:** Credible neutrality requires that the protocol minimize subjective judgments and maintain impartiality. This principle cultivates trust in the protocol among various ecosystem participants.

**Decentralization:** The protocol should encourage and incentivize the decentralization of participants in the operations and governance of the protocol.

As we have observed in the Ethereum community, these two principles can be in conflict: neutrality in Ethereum staking supports free market behavior, leading to expanding market share by a single liquid staking protocol, which conflicts with the goal of decentralization.

The EigenLayer protocol has similar considerations. In a totally neutral protocol (without any per-LST or per-LRT TVL cap), it is possible that a single token dominates the protocol and undermines decentralization. This could lead to the market for programmable trust being subverted by a single counterparty, such as an LRT or LST DAO, which would have the power to pick AVS winners and losers, or engage in other harmful activities. If instead, the protocol natively caps the proportional representation of any given token in order to promote decentralization, AVSs that require participation from a significant fraction of all stakers may not be able to operate inside EigenLayer, thus undermining neutrality.

We suggest three rules that the EigenLayer community could support, which seek to maintain a reasonable balance between neutrality and decentralization in the long-term:

1. No caps on staked TVL on a per-LST or per-LRT basis
2. No caps on payments from AVSs to stakers on a per-LST or per-LRT basis
3. EigenLayer protocol incentives and governance participation capped at 33% for any particular LST, LRT, or single participant (like an exchange)

Rules (1) and (2) form a basis of credible neutrality because staking is permissionless and uncapped, and thus allows for all possible interactions between stakers and AVS developers.  Rule (3) incentivizes decentralization in the protocol in terms of both LST and LRT participation. We highlight that implementation of (3) requires the governance of the protocol to understand subjectively which EigenPod accounts are shared between a common LRT DAO, for example, or which addresses belong to the same exchange, so that the incentive caps can be applied. EigenLayer protocol incentives and governance participation are inherently subjective, and in our view, this minimal governance will not constitute an undue burden relative to its benefits.

We believe this proposal strikes a reasonable balance between the dual priorities of neutrality and decentralization. It will be up to the protocol community to decide to adopt, refine and implement it.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/#/portal)

SubscribeEigen Labs is excited to announce that [Commit-Boost](https://github.com/Commit-Boost), an open-source public good that we have proudly supported since its inception, is moving towards audit and production on the Ethereum mainnet. This milestone, and the community now behind it, marks a significant step forward in ensuring the healthy growth of the proposer commitment vertical without adding complexity to Ethereum’s core protocol development.

We expect a Cambrian explosion of different proposer commitment AVSs to be built in the next year, increasing Ethereum validator rewards.

If you are any of the following, reach out to [@Commit\_Boost](http://x.com/commit_boost) on Twitter today:

- A staker/operator looking to integrate
- A proposer commitment AVS that hasn’t integrated into Commit Boost
- A open-source developer excited about the future of Ethereum

**Why Eigen Labs Supported Commit-Boost from the Beginning**

We have been excited about proposer commitment since the inception of the EigenLayer protocol. In one of our earliest talks, we discussed EigenLayer’s potential in the proposer commitment space. At the same time, we recognized the risks associated with an expansive proposer commitment vertical, specifically around sidecar fragmentation.

The consequence of this fragmentation is two-fold: it compromises the security integrity of the entire network through fragmented implementations and exponentially inflates the burden on our core developers tasked with executing/testing major network upgrades.

This is why we are excited about Commit-Boost. We recognized the potential of Commit-Boost to enhance the Ethereum ecosystem by reducing fragmentation risks and giving validators greater autonomy over block construction. By standardizing how conditions and constraints are communicated, Commit-Boost acts as a unifying sidecar that not only streamlines operations for validators but also introduces additional transparency and safety checks.

This is a _game changer_.

**Commit-Boost’s Progress**

Over the past few months, Commit-Boost has evolved tremendously thanks to the collaborative efforts of over 70 teams and numerous individual contributors within the Ethereum community. Key achievements include:

- **Successful Testing:** Commit-Boost has been rigorously tested on the Holesky testnet with hundreds of thousands of validators, incorporating feedback and making iterative improvements
- **Community Engagement:** Regular community calls moderated by industry experts have fostered open dialogue, feedback, and collaboration among developers, validators, and researchers
- **Feature Development:** The introduction of novel features that decrease operational risks for validators, such as enhanced safety tools and improved efficiency compared to current market standards

We extend our heartfelt congratulations to all the teams, individuals, and contributors who have dedicated their time and expertise to reach this important milestone.

**Excitement for the Future**

As Commit-Boost moves toward mainnet deployment, we’re excited about the possibilities it brings for validators, stakers, operators, and the broader Ethereum community. The initial focus on enhancing proposer-builder separation (PBS) compatibility promises to improve block construction processes and validator autonomy.

We anticipate the development of additional modules in the coming months, enabling validators to implement more conditions and constraints around how blocks are packed. This advancement not only supports Ethereum-aligned initiatives but also opens the door for higher rewards and increased network security.

**Join Us in Supporting Commit-Boost**

We remain committed to the ongoing development and success of Commit-Boost and encourage others to get involved. Whether you’re a validator, operator, developer, or enthusiast, your participation can make a significant impact. Opportunities to contribute include:

- **Feedback:** Engage with the software on testnets and provide valuable insights
- **Development:** Contribute code, review pull requests, or develop new modules
- **Outreach:** Help spread the word and educate others about the benefits of Commit-Boost

Together, we can support this transformative initiative and look forward to a more robust and decentralized Ethereum network.

Thank you to everyone who has been part of this journey. We are excited for what is ahead and invite you to join us in shaping the future of Ethereum!

Building or thinking of building on EigenLayer? Follow along on Twitter at [@buildoneigen](http://x.com/buildoneigen/), [share with us](https://eigenlabs.typeform.com/buildoneigen) what you are building, and reach out to our Developer Relations team directly at [builders@eigenlabs.org](mailto:builders@eigenlabs.org).

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/#/portal)[MEV-Boost](https://boost.flashbots.net/) protects Ethereum’s validator set from Maximum Extractable Value’s (MEV) centralization pressures. It is an open-source project designed by the Flashbots team. MEV-Boost’s relays simplify the interaction between builders and proposers in this system, streamlining the system and eliminating the need for complex cryptography.

There are however concerns within this system. Data shows that as much as [93% of all proposers](https://mevboost.pics/) exchange their transaction inclusion power for MEV profit, essentially a binary choice that has liveness and censorship concerns.

To alleviate these concerns while allowing the proposers to have MEV rewards, we propose two new partial block relay designs, MEV-Boost+ and MEV-Boost++. These are designed to supplement MEV-Boost in the existing system.

**Full design can be found here at [**MEV-Boost+/++**](https://research.eigenlayer.xyz/t/mev-boost-liveness-first-relay-design/15). A video walkthrough of the designs is also available on [**YouTube**](https://www.youtube.com/watch?v=vRqS9x7Nh40).**

![](https://www.blog.eigenlayer.xyz/content/images/2023/08/image.png)High-level Design of MEV-Boost+

MEV-Boost+ gives proposers the power to conduct partial block auctions, minimizing liveness reliance on relays and builders. A half step further, MEV-Boost++ eliminates the trust assumption on the relay altogether.

A prime feature of MEV-Boost+/++ is its flexibility in block auctioning. **It provides the proposer two lanes for block auction**, one for full and one for partial blocks, expanding the former binary choice model to a more fluid one.

In the partial block relay, to prevent MEV stealing, builders receive a crypto-economic guarantee on partial block atomicity. If a proposer breaches the partial block's atomicity, they compensate the builder 30 ETH through EigenLayer’s restaking mechanism. Builders compare this potential extra revenue against their potential loss if a proposer manipulates a block. If the expected loss exceeds 30 ETH, builders would still use the full block relay.

In practice, we expect full block auction and partial block auction will both be used by different proposers during different times. During high volatility periods, full block relays would still be heavily used because of higher MEV rewards. The partial blocks would be much more competitive during lower volatility periods.

**In conclusion, MEV-Boost+ and MEV-Boost++ present an opportunity to reshape the Ethereum ecosystem with a partial block relay design, geared towards enhancing censorship resistance and empowering proposers while preserving MEV profit exposure.** We hope these designs could not only shed light on the liveness risks of the current MEV supply chain but also demonstrate the powerful applications possible on EigenLayer.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/#/portal)

Subscribe## Summary

- Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.
- At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.
- These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive EIGEN in proportion to the amount of rewards distributed to them by AVSs.
- Stakers and operators that support more AVSs will tend to receive more EIGEN programmatic emissions.
- This rewards-boost program is designed to kickstart the rewards flywheel and enable price discovery even if initial AVS rewards are low.
- Separate from the programmatic incentives program, the Season 2 stakedrop will reward current stakers and operators.
- Details on Season 2 will be announced by the Eigen Foundation soon.

## Introducing AVS Rewards

EigenLayer first launched contracts to mainnet on June 14, 2023. Today, EigenLayer has 4.8 million ETH and 108 million EIGEN restaked; 300+ operators running Actively Validated Services (AVSs); and 16 launched AVSs, with many more incoming.

The ultimate vision of the EigenLayer protocol is to be a permissionless marketplace for any blockchain protocol to elastically rent stake and high quality operator sets using both objective and intersubjective slashing conditions.

In the coming weeks, we take another major step toward this goal: the EigenLayer protocol will launch initial [Rewards functionality](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/rewards-claiming-overview), through which stakers and operators will be rewarded for their participation in securing EigenLayer AVSs. For the first time, token-based rewards will begin to flow through the protocol directly from AVSs to stakers and operators. This is an important step towards enabling price discovery for the value of stake and compute provided via EigenLayer.

### Boosting Rewards with Programmatic Incentives

Rewards enables AVSs to distribute tokens to their stakers and operators. Given the newness of the functionality, we expect AVSs to take time to integrate their systems with rewards. In the beginning, total rewards distributed directly by AVSs will likely be small. Blockchains like Bitcoin and Ethereum have solved this startup challenge in the past with programmatic rewards for participants who operate and use the protocol.

In order to activate the AVS rewards flywheel in EigenLayer, there will be a new programmatic incentives program designed for AVS “reward-boosts”, in which EIGEN will be automatically distributed to stakers and operators of each AVS proportionate to the amount of rewards distributed by each AVS. In effect, stakers and operators will receive more programmatic emissions by supporting AVSs that distribute more native rewards. Thus, the programmatic incentives program can be thought of as "boosting" the rewards paid by AVSs to their stakers and operators with EIGEN emissions.

**For the first year, at least 4% of the total supply of EIGEN will be distributed via this program.**

![](https://www.blog.eigenlayer.xyz/content/images/2024/07/DIAGRAM-1.png)

The goal of this design is to incentivize AVSs to begin distributing rewards to stakers and operators early, in order to benefit from and obtain a larger share of the early EIGEN programmatic incentives.

To support the growth of AVSs who are not ready to distribute rewards on Day 1, a small portion of EIGEN incentives will also be allocated to stakers and operators of AVSs that are not yet distributing rewards. This sets a rewards floor outside of the rewards-boost design, so all legitimate AVSs can distribute some rewards to stakers and operators. In this way, new AVSs are encouraged to join the EigenLayer ecosystem even if they are not ready to distribute rewards on their own.

After Rewards launches in the coming weeks, the programmatic incentives program will follow in the ensuing months. In the interim, the Eigen Foundation will share more details and specifications about the program: how exactly it will work, exactly how much EIGEN will be distributed programmatically, and how AVSs can most effectively participate.

Furthermore, the Season 2 distribution to stakers and operators is separate from these programmatic incentives, and details on that will be announced by the Eigen Foundation soon.

### Interacting with Rewards

**_Stakers/Operators_**

Rewards, when available, will be claimable via both [app.eigenlayer.xyz](https://app.eigenlayer.xyz/) and the [eigenlayer-cli](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/claim-rewards/via-cli). Please see the ["Claim Rewards" documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/rewards-claiming-instructions) for a guide to claiming staker and operator rewards.

Note that because slashing is not live, operators can opt into as many AVSs as possible to earn rewards, with no incremental slashing risk. If operators are worried about future slashing risks, they should consider that EigenLayer will include a mechanism called "unique security" under which operators will be able to specify the amount of stake available to be slashed by each particular AVS that they are running. This is designed to mitigate slashing cascade risks that exist in other models. More details on unique security will be shared before the launch of slashing on EigenLayer.

**_AVS Developers_**

For full details on using Rewards as an AVS, please see the ["AVS Integrations" documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/rewards-claiming-overview#avs-integrations). For technical support, to discuss the Rewards roadmap, or to share feedback , please contact our team at **avs@eigenlabs.org**.

As a preview, AVSs can customize a number of parameters in their rewards:

- Which ERC-20 token to use as a reward
- How much of that token to distribute
- Whether rewards should be distributed to past, current, or future stakers and operators, enabling retroactive rewards as well as credible commitments for future rewards
- Specific distribution logic for operators: While more distribution types will be added in the future – such as equal distribution across all operators, distributions proportional to AVS-specific parameters, and vested rewards – at launch Rewards will allow distribution to operators proportional to stake.
- At launch, operator commissions are fixed at 10% of rewards, and stakers at 90%. Variable operator commissions and other commissions logic are high priority roadmap items to be added in a future release.

### The Future of EigenLayer

We appreciate your support and faith in the EigenLayer ecosystem. As the protocol continues to mature and become more fully-featured, and as the AVS ecosystem continues to grow in leaps and bounds, we expect the economy of EigenLayer to blossom and for stakers, operators, and all ecosystem participants alike to benefit.

We look forward to building the future together with you!

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/#/portal)

Subscribe_Written in collaboration with Sam Hart (_ [_Skip_](https://skip.money/) _,_ [_Timewave_](https://timewave.computer/) _). Special thanks to Myles O’Neil (_ [_Reverie_](https://www.reverie.ooo/) _) and Marko Baricevic (_ [_Binary Builders_](https://binary.builders/) _) for feedback and ideation._

## **Summary**

**Ethereum and Cosmos** started with distinct objectives, but their paths are slowly converging. Both are confronting analogous technical challenges such as **MEV, liquidity fragmentation, and broad decentralization.**

**Cosmos** has flourished as a nexus of experimentation, while **Ethereum** has been validated as the composable **settlement** layer. Though the two ecosystems have learned from one another through informal exchange, **a deeper social and philosophical link has been absent**—until **EigenLayer**.

By connecting Ethereum and Cosmos, **EigenLayer** will bring in a new wave of innovations. The **Cosmos** community can tap into Ethereum's foremost source of decentralized security and liquidity; **Ethereum** can tap into the infinite experimentation happening in Cosmos.

## **Introduction**

Ethereum’s vision is to become the global settlement layer, while Cosmos imagines a world of hundreds of digital city states. Their **differing** aims are reflected in **distinct** design choices; Ethereum adopted Proof-of-Stake, focusing on [solo validators](https://ethereum.org/en/staking/solo/) while Cosmos opted for [delegated-Proof-of-Stake](https://www.ledger.com/academy/what-is-delegated-proof-of-stake-dpos#:~:text=Delegated%20Proof%20of%20Stake%20is,designed%20to%20address%20POS's%20limitations.) optimizing for [light client verification](https://docs.tendermint.com/v0.34/tendermint-core/light-client.html) and governance.

**As these systems grow their designs are beginning to converge.** With Ethereum choosing a [rollup-centric scaling roadmap](https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698) and Cosmos exploring shared security schemes, the two ecosystem’s goals and approaches are slowly coming together. As Ethereum L2s mature, initiatives such as the [Superchain](https://app.optimism.io/superchain), [ZK Stack](https://zkstack.io/), and [Polygon 2.0](https://polygon.technology/blog/introducing-polygon-2-0-the-value-layer-of-the-internet), are beginning to mirror the tightly integrated [economic zones](https://joinatom.zone/) found in Cosmos.

Each of these initiatives has unique design trade-offs, but the main goal remains the same: **scaling crypto infrastructure securely**. Therefore, bridging different design learnings across is not only desirable but **necessary**, especially considering the shared challenges faced by these systems.

## **Cosmos and Ethereum are solving similar problems**

[Maximal Extractable Value (MEV)](https://www.theblock.co/learn/245701/what-is-maximal-extractable-value-mev) has been a significant topic in Ethereum since 2020, **influencing its future** roadmap and protocol design. [Proposer builder separating (PBS)](https://ethereum.org/nl/roadmap/pbs/) aims to counteract the centralizing pressure caused by MEV.

PBS in Ethereum is currently implemented via an out-of-protocol design called [MEV-Boost](https://boost.flashbots.net/) using a **trusted** **commit-reveal scheme**. In the future, [enshrined PBS (ePBS)](https://ethresear.ch/t/why-enshrine-proposer-builder-separation-a-viable-path-to-epbs/15710) designs will be integrated into the base Ethereum layer to remove the trusted third party.

![](https://www.blog.eigenlayer.xyz/content/images/2023/12/image-1.png)Ethereum roadmap _by Vitalik Buterin as of December 2021._

In Cosmos, MEV is also a critical point of discussion. Thanks to the **flexibility** of individual Cosmos chains, more advanced ePBS solutions are being implemented. For example, [Osmosis](https://osmosis.zone/) is experimenting with [top-of-block arbitrage profit sharing](https://gov.osmosis.zone/discussion/7812-osmosis-governance-stance-against-offchain-harmful-mev-collusion-and-extraction-software), while [Skip](https://skip.money/) is testing [BlockSDK](https://skip-protocol-docs.netlify.app/chains/overview/), a decentralized block builder and proposer commitment design. Likewise [Fairblock](https://twitter.com/Fair_Block) is trialing an encrypted mempool and commit-reveal schemes for all chains.

![](https://www.blog.eigenlayer.xyz/content/images/2024/01/image.png)Over 1000 IBC connections facilitate trustless communication among various Cosmos chains. The screenshot is from Map of Zones taken in January 2024.

On the other hand, the Cosmos developer community has made **interoperability a primary focus**, largely because of its modular nature. Conceptualized in 2016, the [Inter-Blockchain Communication (IBC)](https://cosmos.network/ibc/) protocol was engineered to solve interoperability challenges through crosschain light client verification. This protocol was put into action in 2021 and, as it stands today, [secures over 1000 IBC connections](https://mapofzones.com/home?columnKey=ibcVolume&period=24h).

In contrast, Ethereum is experiencing difficulties with interoperability as the numbers of L2s and appchains continually increase. As it currently stands, light client verification across L2s is challenging given their variable sequencer design. However, the implementation of decentralized sequencers such as that of [Espresso Systems](https://www.espressosys.com/) and others, have begun to pave the way towards more **dynamic interoperability designs.**

Applying Cosmos’ technology directly to Ethereum has always been a challenge, but L2s now offer a venue for the proliferation of new experiments.

## **EigenLayer Lowers the Barrier for L2 to Utilize Cosmos Innovations**

Many of Cosmos’ innovations involve leveraging the validator set to perform supplementary work. However, operator sets are not available to L2s today. **Maintaining a quorum of validators with strong economic security is notoriously challenging.**

EigenLayer addresses this issue by providing a platform for economic stake - **allowing any staker to contribute to any PoS network.** By reducing the cost and complexities, EigenLayer effectively paves the way for L2s to **tap** into the **expressive** innovations in Cosmos's stack.

To learn more about how EigenLayer achieves this, check out the first section of [You Could’ve Invented EigenLayer](https://www.blog.eigenlayer.xyz/ycie/).

![](https://lh7-us.googleusercontent.com/UR-j1dVxv_-2MiQ3DhOy3hee3gwEK-pnsuqk-r8Ejk-TmULpkIbG3YzMGK-RZ3snhXDp-2m2M6GGq1UWyiIkOnK82a-1GZEGDpRNogOrYMhPEEFicWyK3B0sMWPmXNRnAWgoIx_XRjpQQzjtezrOFu4)Cover photo of You Could've Invented EigenLayer by @EigenIntern

## **Merging the two tech stack breeds symbiotic relationship with infinite possibilities**

**Bringing Cosmos’s bleeding-edge innovation to Ethereum**

The app-specific innovation coming out of Cosmos is the **perfect complement** to EigenLayer’s sophisticated staking community and capital base. We expect a deeper **collaboration** to be highly generative, expanding the functionality of Ethereum and creating the context for Cosmos builders to apply their talents toward the largest on-chain programmable staking economy in the world.

![](https://www.blog.eigenlayer.xyz/content/images/2024/01/0xkydo_a_man_pulling_Cosmos_and_Ethereum_together_with_immense__49b9b1ed-b87c-44ea-9250-ce9496b9b434-1.png)Midjourney prompt: "a wizard pulling the Cosmos together with Ethereum using an immense amount of force"

We also **imagine sophisticated Ethereum projects** will continue to outsource specific application functionality to specialized committees. These committees will perform advanced but well-defined actions to assist Ethereum applications, such as seal bid auctions, machine learning inference, ordering mechanisms and the like.

**Decentralization From Day One Without a Token**

In the combined Cosmos-Ethereum landscape, **decentralization is prioritized** from the onset. Rather than tying this capacity to a native token, EigenLayer allows tapping into the enormous validator set of the Ethereum network from day one. Exporting Ethereum’s decentralization ensures **enhanced security and promotes permissionless access** to network operation, facilitating a more resilient ecosystem.

**Access to Ethereum User Base and Node Operators**

It is no secret that the Cosmos ecosystem is hungry for users given all its technical innovations. **Ethereum wL2 is the perfect place to house new experiments.** Moreover, since the Beacon chain went live in 2020 Ethereum validators have been trained to operate a protocol worth tens of billions of dollars. **Inheriting Ethereum’s $60 billion security through EigenLayer** allows L2s to replicate their expertise to handle the node-level operation, ensuring a smooth experience for end users.

## **Ethereum’s Economic Security in Cosmos**

The benefit is mutual. **Amazing** teams are actively developing solutions that bring Ethereum's **large economic security to Cosmos**. Here are three projects (listed alphabetically) that are directly involved in this.

**Roll your own AVS with the Cosmos SDK**

Currently, within Cosmos SDK, one can easily spin up a chain with either a PoA or PoS configuration using a native staking token. While [renting security via replicated security](https://github.com/cosmos/interchain-security) or [shared security](https://cosmos.network/interchain-security/) is possible today, deployment involves nuanced governance engagement with other chains. **In the near future, developers will be able to spin up a new PoS chain using Ethereum’s security** or a new PoA chain with a targeted security budget. Chains can tailor their security needs and separate the network launch from the launch of a token.

[**Ethos**](https://www.ethosstake.com/) _contributed by Karthik Raju_

Inter-chain Security (ICS) allowed chains to borrow economic security from the ATOM token, and the Cosmos Hub validator set. Since then, new standards such as Mesh and Hybrid Security have entered the market.

**Ethos is the next step in the shared security narrative of Cosmos.** Ethos brings re-staked ETH from EigenLayer into a new provider chain hub for the Cosmos ecosystem. Cosmos chains often pay with 10%+ inflation schedules because the underlying trust layer is powered by volatile assets. ETH is the highest-quality on-chain asset. **Restaked ETH is the lowest opportunity-cost version of ETH, requiring lower single-digit inflation/cost for consumer chains.**

Ethos will act as a **central source** of restaked ETH, from which Cosmos chains can borrow to bootstrap their trust layer with low cost (inflation). By allowing for greater optionality, Ethos greatly improves on the original mechanics of shared security.

[**Lay3r**](https://twitter.com/Lay3rLabs) _contributed by Jake Hartnell_

Lay3r is building a new IBC-enabled stack supporting both **WASM smart contracts and the EVM**. While drawing much from the Cosmos stack (most importantly Comet BFT and IBC), it features a **modular rust-based SDK** that allows developers to easily launch their own sovereign high performance L1 or EigenLayer powered L2.

Developed by two of the creators of [Mesh Security](https://www.youtube.com/watch?v=BGlWSRlY1PI), the Lay3r L2 (secured via Eigenlayer) will **allow for ETH re-stakers to earn additional rewards** by providing economic security to the Interchain as well as provide a Schelling point for developers building IBC-powered protocols and chains.

## **Conclusion**

**As Cosmos and Ethereum form a deeper relationship, we expect more ideation and innovation.** EigenLayer will **continue** to serve as the conceptual link between the two ecosystems, adding value to both ecosystems through our commitment to **Open Innovation.**

**In line with this vision, we're testing out the idea for a small Cosmos x Ethereum Conference.** We've received positive responses so far. If this interests you, please record your email here to show support: [https://bit.ly/cosmosxethereum](https://bit.ly/cosmosxethereum)

**Lastly, if you are**

- an **L2** interested in tapping into the latest Cosmos ideas
- a **Cosmos chain** interested in working with EigenLayer

Send us a DM on [twitter](https://twitter.com/eigenlayer) or at [@0xkydo](https://twitter.com/0xkydo)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/cosmos/#/portal)One of the **most** powerful features of EigenLayer is the notion of dual staking. In this article, we will discuss what dual staking is, how it increases the robustness and decentralization of any PoS network, how it mitigates the death spiral problem with network token volatility, and how these networks can still curate meaningful value accrual in their ecosystem when opting into EigenLayer.

## Proof of Stake Explained

At a basic level, every Proof of Stake (PoS) network offers some services to its users. **Users submit tasks, and the network responds appropriately.** The user then can **verify** that the network response has satisfied some **quorum** rules.

For example, in a PoS-based oracle network, a user can submit a task for a price feed update. A valid response is a signed price update from a certain percentage (X%) of the network, where X is a variable set by the **network** or the **user** validating the response. ChainLink, for instance, sets this quorum at 66% for a valid response.

Similarly, in a PoS blockchain utilizing Byzantine Fault Tolerance (BFT), a common user task might be to include a **transaction**. Here, a valid response would be a **block** that contains the **transaction**, endorsed by signatures from at least two-thirds of the operators.

**Problem:**

**However, bootstrapping a new Proof-of-Stake (PoS) network is challenging.** It typically involves issuing a new token and persuading people to buy and stake the token.

These native tokens are often **volatile**, incurring high inventory **risk** for holders. These native tokens might also be **hard** to access because of their novelty and **lack** of exchange listings.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/image-1.png)

Moreover, PoS networks in the early days might face the potential “ **death spiral**” because the value of the native token is closely tied to the system’s overall security. A sudden **decrease** in this value could significantly **weaken** the PoS network’s security, leading to a **drop** in Total Value Locked (TVL). This reduction in TVL could further **depress** the native token’s price, creating a death spiral.

**Solution: Dual Staking**

Dual staking uses **two** tokens to secure the **same** PoS network. If one of these tokens is an external network token with **lower volatility, deeper liquidity, and more access, such as ETH**, it could address the aforementioned challenges new PoS networks face.

Dual staking **simplifies the bootstrapping process**. Instead of requiring stakers to hold a volatile new token, they can stake ETH into the network. Alongside staking ETH, stakers can also stake the network’s native token.

Dual staking also **mitigates** the “ **death spiral**” effect. If the native token’s price falls, the PoS network's security is still affected, but the impact is limited. This is because the ETH staked in the PoS network provides a baseline economic security.

## **Dual Staking**

Now that we have two tokens, we also need a way for the users to **validate** the responses from both the native [operators](https://www.blog.eigenlayer.xyz/ycie/#:~:text=Our%20goal%20is%20to%20distinguish%20between%20these%20two%20roles.%20Stakers%20provide%20tokens%20for%20the%20economic%20security%20of%20each%20AVS%2C%20while%20operators%20are%20individuals%20who%20run%20software%20to%20ensure%20the%20operational%20security%20of%20each%20AVS.) **AND** ETH-backed operators. In the following part, we will specify multiple options on how to achieve this and its security trade-offs.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/image.png)

💡

Native operators refers to operators who have native PoS tokens (native token) staked/delegated to them. ETH-backed operators refers to operators who have ETH staked/delegated to them.

1. **Modular Dual Staking:** Here, a valid response requires both the native operators and ETH-backed operators reaching quorum **separately**. If either of the quorum is not met, it won't be considered a valid response.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/modulardualstaking.png)

**2\. Native Dual Staking:** This approach functions similarly to [mesh security](https://github.com/osmosis-labs/mesh-security/blob/main/docs/README.md). Instead of the individual operator sets reaching quorum separately, we will treat them as one set of operator. Each operator stake would be converted into a common denominator based on some external price feed (eg. an oracle). Then the user would validate the response based on the combined quorum. (if this is confusing, check out [mesh security](https://github.com/osmosis-labs/mesh-security/blob/main/docs/README.md) for a more detailed explanation)

3. **Veto Dual Staking**: Firstly, the native operator achieves a quorum on its own, similar to a conventional PoS system. Next, the ETH-backed operator acts as a safeguard, **checking** the actions of the native operators. If the native operators make a mistake, the ETH-backed operators have the power to **veto** the valid response issued by them. This **approach** differs from Modular Dual Staking in terms of **liveness**. If the ETH-backed operators go offline, this model ensures that the PoS continues to function.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/vetodualstaking.png)

## **Comparison of Methods**

Each design has different trade-offs. To illustrate this, the table below describes these trade-offs using dual staking for a [Tendermint](https://tendermint.com/) blockchain. Recall that Tendermint operates under a ⅓-honesty assumption.

|     |     |     |     |
| --- | --- | --- | --- |
| Method | Cost to Violate Safety | Cost to Violate Liveness | Implementation Cost |
| Native Dual Staking | ⅔ $ of (native + ETH) | ⅓ $ of (native + ETH) | High |
| Modular Dual Staking | ⅔ native + ⅔ ETH | Min(⅓ native, ⅓ ETH) | Low |
| Veto Dual Staking | ⅔ native + ⅔ ETH | ⅓ native | Low |

Using Modular Dual Staking as an example, for the chain to experience a safety violation, such as signing an invalid state transition, **both** operator groups must be compromised. This is because acceptance of a block into the chain's history requires agreement from two-thirds of both the ETH-backed and native operators.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/Untitled.png)In Modular Dual Staking, a valid response from the PoS network requires signature/validation from both sets of the validators. If either of them rejects, the response would be considered invalid.

However, the chain can be halted by just one-third of **either** the ETH-backed or native operators. While ETH and the native token may contribute differently to the chain's economic security, an attacker only needs to compromise the group with lesser security to affect the chain's liveness.

## **Recap**

**Dual Staking is a concept designed to solve the difficult issue of bootstrapping a new PoS network.** It reduces risks linked to the volatility of new native tokens and the possible death spiral by integrating a well-established token, such as ETH, into the system.

This system can accommodate various requirements by implementing dual-staking consensus mechanisms like **Native** Dual Staking, **Modular** Dual Staking, and **Veto** Dual Staking. Each mechanism offers unique advantages and cost implications, providing network designers with choices based on their operational and economic needs.

If you are thinking of utilizing dual staking, please submit your proposal via this form, and we can give you feedback on your design and start the conversation: [https://bit.ly/avsquestions](https://bit.ly/avsquestions)

If you’re excited about these primitives, join our research discussion group here: [https://bit.ly/programmabletrust](https://bit.ly/programmabletrust)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/dual-staking/#/portal)

SubscribeEigen Labs, in partnership with LayerZero Labs, is introducing a framework for CryptoEconomic Decentralized Verifier Networks (DVNs).Eigen Labs chose LayerZero because it is one of the most battle-tested protocols in crypto, handling millions of messages and securing billions for apps.

As the first step of this integration, we are proud to announce that Eigen Labs and LayerZero Labs have jointly developed a CryptoEconomic DVN Framework which **allows anyone to easily bootstrap a DVN on EigenLayer using any tokens and operators.**

The first DVN to utilize this novel Framework is the LayerZero Labs DVN.  It **implements a system that accepts EIGEN, ZRO, and ETH as staking assets.** The Framework is open source, allowing other teams to launch their own CryptoEconomic DVNs using their own assets as stake.

This article explains how EigenLayer and LayerZero complement each other and dives into the implementation of the CryptoEconomic DVN Framework.

### **Why EigenLayer x LayerZero?**

Eigen Labs and LayerZero Labs share a vision of building a verifiable internet across blockchains, driven by permissionless security. One where anyone can participate in securing the network and verifying the integrity of messages across chains.

Previously, the security of verifying omnichain messages was solely based on the DVN’s verification mechanism. But now, with Eigenlayer’s re-staking primitive, anyone can stake their assets to provide an extra layer of security (i.e. CryptoEconomic security) to DVNs.

### **EigenLayer: Economic Security**

At its core, EigenLayer allows participants to stake any assets or restake assets that are already securing a protocol like Ethereum to secure additional services, applications, or verification mechanisms. By leveraging Ethereum’s existing trust and capital, EigenLayer’s innovation is to create a shared pool of security that can be extended to multiple use cases.

This coordinated security model enables the CryptoEconomic DVN Framework, where verifiers are incentivized to act honestly, knowing that misbehavior will be punished through slashing.

### **LayerZero: DVN Marketplace**

LayerZero is an omnichain interoperability protocol that enables secure communication across 80+ blockchains, from Ethereum to Solana.

Each application customizes its security by selecting a DVN or set of DVNs to verify its messages, with the choice based on the sensitivity of the data. For example, an NFT project might use a single proof-of-authority DVN for simple transfers, while a DEX may select multiple DVNs with diverse verification methods for critical governance votes. Currently, 35 entities participate as DVNs, including zk-proof-based teams like Polyhedra, multi-bridge attestations from Hashi, and oracles like Google Cloud. This essentially makes LayerZero a marketplace for verifiers.

The CryptoEconomic DVN Framework will supercharge this competition by allowing ANY DVN to transparently demonstrate its reliability through the value of staked assets and the subsequent risk of slashing. This reflects a DVN’s commitment to verifying messages accurately, without malicious behavior or downtime.

### **LayerZero Labs: The First Cryptoeconomically Secured DVN**

The first team to launch a CryptoEconomic DVN secured by EigenLayer restaking is LayerZero Labs.

The LayerZero Labs DVN, which has verified over 100 million messages historically, is transitioning from a PoA model to one backed by restaking. ( _Note: LayerZero Labs is a development team building infra for LayerZero protocol. The protocol itself is DVN-agnostic._)

Specifically, staked tokens— **ZRO, ETH, and EIGEN**—provide economic security for message verification for applications that choose the LayerZero Labs DVN as part of its security stack. If the DVN fails to act correctly or experiences downtime, the staked tokens are at risk of being slashed, giving application developers a clear, measurable way to assess the security of the DVN.

### **Generalizing the Framework for Broader Use**

The CryptoEconomic DVN Framework co-developed by Eigen Labs and LayerZero Labs is open and generalized, meaning any team can design and deploy its own DVN Actively Validated Service (AVS).

This gives applications the flexibility to create CryptoEconomic DVNs, where they can define slashing conditions and choose _any_ _assets_ that can be staked based on their specific requirements.

### **Conclusion: A New Era for Omnichain Verification**

The introduction of EigenLayer – and therefore cryptoeconomic security via restaking –into omnichain messages changes the risk profile of deploying an application across many chains, allowing developers to trust DVNs based on the economic stake at risk of slashing.

This shift moves away from trust-based systems to economically quantifiable security that can be audited onchain and configured in any way (since the architecture is open and permissionless).

With EigenLayer’s restaking model and LayerZero’s messaging framework, developers now have the tools to build highly secure, application-specific DVNs that are economically incentivized to act honestly. As more applications adopt cryptoeconomic security, **we hope to see a proliferation of CryptoEconomic DVNs securing a variety of use-cases**.

This collaboration marks only the beginning of a broader partnership between Eigen Labs and LayerZero Labs. As more innovations unfold, the combined strength of cryptoeconomic security and omnichain messaging will continue to drive the future of a better internet, built across blockchains.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/dvn/#/portal)

SubscribeOver the past few years, we at Eigen Labs have been developing a platform for advancing the concept of open, verifiable digital commons. This blog post summarizes the intersubjective forking protocol enabled by the EIGEN token. We will break down the significance of EIGEN, its core ideas, its high-level implementation, how it extends the utility of Ethereum, and the impact on open innovation via this new token design.

The intended audience for this post is engineers and researchers. You can also read the [full EIGEN whitepaper here](http://eigen.eigenlayer.xyz/).

# Introduction

**_Categorization of faults_.**

Blockchains and other decentralized systems are enabled by protocols that enable disparate honest actors to coordinate towards goals with resilience to adversarial actors. These protocols generally work based on incentivizing protocol-consistent behaviors and disincentivizing malicious behaviors. One observation regarding these protocols is that not all behaviors, or "faults," can be rewarded/punished with the same level of precision. Broadly, we can categorize faults into four types:

1. **Objectively attributable faults** are attributable purely mathematically and cryptographically without the need to ask the opinions of others, e.g., rollup execution validity.
2. **Intersubjectively attributable faults** are a set of faults where there is a broad-based agreement among all reasonable active observers of the system, e.g., data withholding.
3. **Non-attributable faults** are not observable to anyone other than the victim, such as privacy violation in decentralized-secret-sharing.
4. **Subjective “faults”** are those where observers make their own judgments, with no guaranteed concordance across different observers. For example, what is the best restaurant in New York City?

Since non-attributable and subjective faults do not have broad concordance, in this work, we only consider objective and intersubjective faults.

![](https://www.blog.eigenlayer.xyz/content/images/2024/08/a.png)

**_Importance of social consensus._** The mechanisms currently being used in the industry for resolving intersubjectively attributable faults are:

(a) slash the stake of operators whose response to the task diverged from the majority response,

(b) use a committee to slash the operators whose claims are not concordant with the “true” answer, or

(c) slash the stake by forking the chain.

Mechanisms (a) and (b) are vulnerable to tyranny of the majority. In such an attack, many more stakeholders besides the operators actively responding to the tasks or the stakers delegated to the operators will be affected. A nice feature of this broader group of stakeholders, colloquially called _social consensus_, is that it is an unsized and potentially infinite set. Mechanism (c) involves empowering social consensus to slash the adversarial operators in the event of majority corruption of the operator set.

**_Restaking ETH._** Any task with an objectively attributable fault can be resolved onchain by integrating its dispute resolution mechanism within Ethereum’s chain validity through smart contracts. EigenLayer employs this property to restake ETH and expand the scope of staked ETH to secure **A** ctively **V** alidated **S** ervices (AVSs) with objectively attributable faults.

**Staking with EIGEN.** The new EIGEN token introduces a complementary mechanism that is designed to specifically address “intersubjective” faults – faults that are not possible to address via ETH restaking alone.

In essence, ETH restaking in EigenLayer expands ETH to become the **Universal Objective Work Token** and the universality of EIGEN makes EIGEN the **Universal Intersubjective Work Token.**

# Core Ideas

We call staking with EIGEN _intersubjective staking,_ whichbuilds upon the following core ideas.

**_Setup and execution phase._** The social consensus underpinning intersubjective agreement typically proceeds in two phases:

1. the **_setup phase_** at the genesis, where the rules of coordination among the system's stakeholders are codified;
2. the **_execution phase_** in which pre-agreed rules are executed, ideally locally, in a self-evident manner.

It is important to note this separation, because the setup phase is where the coordination conditions are defined, and for any user that opts in, the execution phase is self-evident and self-executable. So we do not mean social consensus in the sense of users getting into a room or a discord channel to arrive at the resultant consensus. Instead, we refer to systems where users can self-execute conditions that they have pre-agreed to.

**_Slashing._** Slashing offers a mechanism to penalize operators that violate the core rules of the AVS, by burning or redistributing their staked digital assets.

_**Token Forking.**_ While slashing can be implemented via smart-contracts for objective faults, for intersubjective faults, smart-contract implementation is insufficient. This is because, from inside the EVM, it is not clear whether the intersubjective fault occurred or not, whereas it is clear to the observers in an external frame-of-reference. We note that the value of a token arises from the external frame-of-reference or the social consensus that one particular token is canonical. Token forking is a mechanism that seeks to utilize this external frame-of-reference in value allocation in order to induce cryptoeconomic penalties. The core idea is that if there is a fault induced by misbehavior of a (super-)majority of EIGEN token stakers, any challenger can create a fork of the token, in which the malicious stakers are slashed. Now this creates two possible versions of the EIGEN token, and users as well as AVSs can freely decide which one to respect and value. If there is general agreement that the slashed stakers misbehaved, then the users and AVSs will value only the forked token rather than the original token. Since in the forked token, the malicious stakers do not have any allocation, they lose the original value inherent in their stake. We call this mechanism _slashing-by-forking_.  This idea was pioneered by Vitalik Buterin documented in the Ethereum blog (circa 2014-2015): [The p + epsilon attack](https://blog.ethereum.org/2015/01/28/p-epsilon-attack), [Schellingcoin](https://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed), [The subjectivity / exploitability tradeoff](https://blog.ethereum.org/2015/02/14/subjectivity-exploitability-tradeoff). A grounding principle in the EIGEN token is to ”slash stakers” only when their fault is beyond a reasonable doubt. Under these conditions, slashing requires large collusion as well as a large potential loss of funds, and this suffices to ensure honest behavior. Thus we expect EIGEN forking to be a last-resort nuclear option, but nevertheless one that is needed to ensure that the stakers behave correctly.

Different from previous mechanisms for token forking, EIGEN presents four new features.

1. _**Universality**._ EIGEN is a universal intersubjective token, which means, it is designed in its Setup phase to fork-and-slash for intersubjective faults committed by EIGEN stakers across any AVS in EigenLayer. In specialized systems like Augur, there is an application dependent mechanism to figure out the total value of cryptoeconomic risk (profit-from-corruption) to ensure the system is safe. In order for EIGEN to be used universally across applications, we have designed an application-independent mechanism for ensuring the system remains cryptoeconomically secure.
2. **_Isolation._** While EIGEN needs to have the ability to fork, this induces an externality on all applications and users as they need to be fork-aware. For example, in a lending market where there is no fixed term for the loan, the forking token cannot be accepted as collateral, as it is possible that the token will fork in which case the smart contract needs to go and redeem that fork. In order to prevent this problem, the EIGEN protocol uses a two-token system where one token can be used in fork-unaware applications and another forkable token is utilized for staking and forking. Of course, there should be a binding relationship between these two tokens. We term the former “fork-unaware” token as a “solid representation” if it allows holders to redeem an equivalent number of tokens from any of its descendant forks at any future time. This property makes it easy for anybody to use the solid representation in applications like a lending protocol. We have designed EIGEN such that it achieves isolation and solid representation properties.

![](https://www.blog.eigenlayer.xyz/content/images/2024/08/b.png)

3. **_Metering._** Resolving any intersubjective fault incurs a cost to social consensus for switching from one token to another, or rejecting a malicious fork. Therefore, the whitepaper proposes that any claim to fork the token (termed an intersubjective challenge) should require depositing a bond in EIGEN to deter malicious challenges. This bond needs to be higher than the cost incurred by the social consensus (users and AVSs) to consider and reject a malicious fork. Additionally, any successful challenge results in a significant cost to the broader ecosystem in the form of contract upgrades for incorporating the new fork in daily operations. A challenge, therefore, should only be raised if a sufficient amount of staked EIGEN can be considered malicious and burnt resulting in a lower token supply and higher utility for the remaining EIGEN. These social costs need to be built into the core protocol to appropriately **meter** the costs and tradeoffs of social consensus.
4. **_Compensation._** The protocol for intersubjective staking ensures that if an AVS is attacked due to a malicious quorum of EIGEN stakers, that AVS is able to slash and redistribute the malicious stake back to the AVS users. If the AVS ensures that this “attributable security” is greater than the harm done to its users, then it achieves “strong cryptoeconomic security” which specifies that no honest user suffers any harm. Previous definitions of cryptoeconomic security required that the adversary have a smaller profit from corruption than the cost induced by slashing, and thus had a major shortcoming in that it is not possible for a protocol to know comprehensively the ways in which an adversary may profit from an attack (for example, using an external short position). Strong cryptoeconomic security, on the other hand, is a user-centric characterization and does not need to make any assumptions on the adversary or even the other users’ incentive structure. For example, when there are multiple AVSs, as long as a given AVS ensures that it has enough attributable security, this particular AVS is protected even if the system is in sum total not secure because other AVSs do not have enough security.

# A World with Intersubjective Staking of EIGEN

**_Complementary roles of EIGEN and ETH._** EIGEN staking and ETH restaking can each fill complementary roles within a staking system such as EigenLayer. Many mature AVS protocols consist of both safety properties that can be secured via objective slashing and liveness or censorship-resistance properties that would previously depend on majority-assumptions which rest on stake decentralization. For a service that uses restaking for safety and intersubjective staking for liveness, fees can be split between the two quorums. Furthermore, for core services provided to the Ethereum ecosystem, we envision many services that will use dual staking between ETH and EIGEN, where the native restaking absorbs the decentralization / collusion resistance and operator alignment that comes with ETH restaking, and the EIGEN staking can actually support cryptoeconomic slashing. In this model, the former serves as a mechanism to obtain majority trust from the Ethereum participants, and the latter serves as a mechanism to obtain economic security.  For example, an oracle built in this model can be thought of as a hybrid between the [enshrined oracle model proposed by Justin Drake](https://ethresear.ch/t/enshrined-eth2-price-feeds/7391) and the cryptoeconomic oracle model proposed in [Augur](https://www.allcryptowhitepapers.com/wp-content/uploads/2018/05/Augur-white-paper.pdf) and in this [post](https://ethresear.ch/t/a-not-quite-cryptoeconomic-decentralized-oracle/6453).

**_EIGEN enables a higher rate of innovation in objective AVSs._** Intersubjective staking can also be used to support digital tasks which could in principle be secured via objective fraud proofs, but where doing so would involve a large amount of technical complexity and associated risk. In particular, in envisioning the lifecycle of a new objective AVS, we can identify a progression that leverages intersubjective staking for security early in the bootstrapping phase of the protocol and transitions to restaking or even native protocol adoption as the protocol matures and ossifies and more faults can be made objective.

**_Quorum composition._** In keeping with the spirit of open innovation, EigenLayer allows AVSs to mix and match these two modalities of objective staking from ETH and intersubjective staking from EIGEN in addition to utilizing the native AVS tokens for providing additional validation from an aligned community of AVS token stakers.

![](https://www.blog.eigenlayer.xyz/content/images/2024/08/c.png)

**_Examples._** Intersubjective staking with EIGEN unlocks a vast range of previously impossible AVSs on Ethereum with strong cryptoeconomic security. This opens the door to innovations for: Oracles, Data Availability layers, Databases, AI systems, Gaming VMs, Intent & Order Matching & MEV engines, Prediction markets, etc.

# Roadmap

The launch of EIGEN introduces intersubjective staking. This has broad ramifications for the crypto ecosystem as a whole. With its design being completely novel, the concept needs to be absorbed and discussed widely by the ecosystem participants. The initial implementation of intersubjective staking at this launch mirrors the full protocol to only a limited extent. However, there are still several parameters that need to be determined for full actuation. To accomplish that, EIGEN is being launched in a meta-setup phase. This meta-setup phase will serve as a call-to-action where researchers, experts, and the broader community engage in public discourse to discuss the necessary parameters to make the protocol and its interaction with the rest of the Ethereum ecosystem most effective.

# Conclusion

The EIGEN system's new functionalities effectively open up new vistas for open innovation. It solves fundamental challenges like universality, isolation, metering, and compensation. By using social consensus and forking, EIGEN empowers the secure execution of diverse digital tasks with cryptoeconomic security, effectively paving the way for the construction of an open, verifiable, digital commons.

As developers and researchers continue to refine and adopt EIGEN, we are excited to help you learn more about EigenLayer and hear from you. Below are some resources:

- Read the [full EIGEN whitepaper here](http://eigen.eigenlayer.xyz/).
- Reach out to us via our builder form: [https://bit.ly/avsquestions](https://bit.ly/avsquestions)
- AVSs that are live: [https://app.eigenlayer.xyz/avs](https://app.eigenlayer.xyz/avs)
- Resources for how to build an AVS: [https://github.com/Layr-Labs/awesome-avs](https://github.com/Layr-Labs/awesome-avs)
- Get inspired on more AVS ideas: [https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigen/#/portal)

SubscribeFeb 2, 2024 – EigenLabs, along with Offchain Labs and AltLayer, today announced EigenDA support for Arbitrum Orbit chains, bringing scalability to the Ethereum ecosystem without sacrificing on security. The integration offers developers the ability to build EigenDA-based Orbit rollups that bridge from Arbitrum One, Arbitrum Nova, and Ethereum, and boast lower transaction fees and higher transaction volume.

[Developer Docs: EigenDA <> Arbitrum Orbit](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-rollup-user-guides/orbit/)

Arbitrum Orbit is the permissionless path for building rollups with the same software that powers Arbitrum's Layer 2 chains _Arbitrum One_ and _Arbitrum Nova_. This integration further enables developers to store L2 transaction data in the EigenDA data availability layer, scaling the maximum transaction throughput from kilobytes per second to megabytes per second, at a fraction of the cost of Ethereum calldata.

EigenDA support for Orbit was made possible through a development partnership with [AltLayer](https://altlayer.io/), which offers rollup deployment and sequencer hosting services as well as a suite of services for adding decentralized sequencing, fast finality and state-root verification support to rollups.

The EigenDA + Arbitrum Orbit integration launches without fraud proofs, a feature planned for release before EigenDA mainnet. Fraud proof support is important for minimizing trust in the rollup sequencer.

EigenDA is a secure, high throughput, horizontally-scaled and decentralized data availability (DA) service built on top of Ethereum using the [EigenLayer](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/eigenlayer.xyz)’s restaking primitive. Developed by EigenLabs, EigenDA will be the [first actively validated service (AVS)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/) to launch on EigenLayer. Ethereum stakers will be able to delegate stake to node operators performing validation tasks for EigenDA in exchange for service payments, and rollups will be able to post data to EigenDA in order to access lower transaction fees for users, reserve bandwidth for lower fee volatility, and higher transaction throughput that scales horizontally with operator count. Furthermore EigenDA offers rollups the ability to pay in their own token as well as utilize their native token in an additional dual quorum setup for securing the DA layer.

“The future of data availability is scaling application development for Ethereum,” said Sreeram Kannan, CEO at EigenLabs. “We are delighted to work with the Arbitrum ecosystem to enable Orbit chains to achieve high throughput while being secured by ETH and their rollup token under the dual quorum model. In addition to data availability, we are also looking forward to enable cross-composable protocols among the Orbits to inherit security via Eigenlayer.”

“We are excited to welcome EigenDA into the Arbitrum ecosystem, enabling secure, cost-effective data availability for Orbit chains. Launching a blockchain has never been more flexible and accessible,” said Austin Marrazza from Offchain Labs. “This addition to their platform represents a significant step forward in the Ethereum ecosystem for rollup developers wishing to participate in the Arbitrum ecosystem. We’re excited to see how this new feature will enhance the growing demand for EigenDA in the Arbitrum ecosystem."

“On the road to mass adoption of Ethereum, we need to provide thousands of rollups for various apps in a fast, cost-efficient, secure and decentralized manner,” said YQ, founder of AltLayer. “With a production-ready integration of Arbitrum's Orbit and EigenDA, we've made a huge leap towards the goal. It's been a pleasure to work with the Arbitrum and EigenLabs team on making this possible. We're excited to leverage this integration to help more rollups scale their businesses via AltLayer.”

Interested in deploying an Orbit chain with EigenDA? Reach out via the [EigenDA contact form](http://contact.eigenda.xyz/) or DM us [@eigen\_da](https://twitter.com/eigen_da) on Twitter. [Docs](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-rollup-user-guides/orbit/) and [code](https://github.com/alt-research/nitro-eigenda/tree/eigenda) are available for eager developers.

**About EigenLabs**

EigenLabs is the research organization leading development of EigenDA and EigenLayer. Founded by Sreeram Kannan, EigenLabs is backed by Blockchain Capital, Polychain Capital, Ethereal Ventures, and more.

**About Offchain Labs**

Offchain Labs is a venture-backed, Princeton-founded company that has dedicated over 5 years to blockchain research and development. As the original developers of the Arbitrum technology, Offchain has been instrumental in revolutionizing the industry through this leading network scaling solutions. The team continues to build upon this foundation by innovating and enhancing products such as Arbitrum Orbit, Stylus, BOLD, and Arbitrum Nitro.  In October 2022, Offchain Labs acquired Prysmatic Labs, the leading consensus client for Ethereum, further cementing Offchain Labs alignment with Ethereum.

**About AltLayer**

Founded in 2021, AltLayer is a decentralized protocol that facilitates the launch of native and restaked rollups with both optimistic and zk rollup stacks. Our solutions strengthen on-chain functionality at every level - sequencing, execution, and verification.

Built atop this protocol, AltLayer also offers a no-code Rollups-as-a-Service (RaaS) launchpad that allows anyone to spin up a customized rollup within 2 minutes with only a few simple clicks. The RaaS launchpad is designed for a multi-chain and a multi-VM world. It supports different rollup SDKs such as OP Stack, Arbitrum Orbit, Polygon zkEVM CDK, and zkSync Hyperchain among others.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/#/portal)

Subscribe_We are supporting rollup production traffic on EigenDA mainnet available immediately. **Anyone can now deploy their rollup and be whitelisted on our free tier.** Interested rollup customers should fill out the_ [**_EigenDA Contact Form_**](https://forms.gle/NFfk1MTEj1NzhAPz5) _in order to be approved for the initial free-tier usage phase. Please provide the ETH address_ _associated with a new ECDSA keypair for authenticating your client with the EigenDA disperser. We strongly recommend that no funds be held by this contract address; see_ [_documentation_](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/overview#authentication) _and_ [_usage tutorial_](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/disperser-golang-grpc-client) _for further usage guidance. If you’d like to learn more about our reserved bandwidth tiers, please_ [_get in touch_](https://forms.gle/NFfk1MTEj1NzhAPz5) _. As always, good places to get started are_ [_EigenDA documentation_](https://docs.eigenlayer.xyz/eigenda/rollup-guides/tutorial) _and the_ [_Holesky_](https://docs.eigenlayer.xyz/eigenda/networks/holesky) _testnet._

**Onboarding Rollups to Dual Quorum**

One of the business models described in the [EigenLayer whitepaper](https://docs.eigenlayer.xyz/assets/files/EigenLayer_WhitePaper-88c47923ca0319870c611decd6e562ad.pdf) is the dual staking utility which can feature two quorums: ETH restakers and $AVS stakers. We are happy to announce that the dual quorum model we [previously announced](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/) on Holesky testnet is now available on EigenDA mainnet. Initially, EigenDA dual quorum launches with ETH as the first quorum and EIGEN as the second quorum. This means that EIGEN holders will be able to stake to EigenDA operators, and those operators can opt in to EigenDA’s dual quorum, adding more security to the network. Following [stakedrop claims](https://blog.eigenfoundation.org/claims-s1-p1/) on May 10, EigenDA currently has over 4 [4M EIGEN](https://app.eigenlayer.xyz/avs/0x870679e138bcdf293b7ff14dd44b70fc97e12fc0) staked and delegated.

We are working hard to extend this multiple quorum model to **enable EigenDA rollups to stake their own custom tokens** to secure blobs. More to come on this soon.

Each quorum backing a given EigenDA blob lends additional security, since the blob will remain available as long as there is a single quorum which has not been corrupted. Individual quorums can also provide additional security properties given the characteristics of the token being staked. EigenDA is currently building on a roadmap which will maximize the security provisioned by both the ETH and EIGEN Quorums:

- As slashing comes to the core EigenLayer protocol, EigenDA will work to enable ETH slashing via [Proof of Custody](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/#security-features).
- The [EIGEN whitepaper](https://github.com/Layr-Labs/whitepaper/blob/master/EIGEN_Token_Whitepaper.pdf) introduces the idea of intersubjective slashing, which becomes a possibility with an EIGEN quorum securing EigenDA. Dual quorum launch prepares the way for entering the [intersubjective setup phase](https://www.blog.eigenlayer.xyz/eigen/) at the right moment.

This is a step towards our ambitious roadmap of providing the best data availability layer deeply integrated with tooling to enable open innovation.

S [take EIGEN](https://app.eigenlayer.xyz/restake/EIGEN) now on EigenDA.

**Mainnet Production Traffic**

We recently announced that [EigenDA is live on mainnet](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/) and we have been onboarding rollups for test traffic. After this final stage of testing and hardening, we are pleased to announce that EigenDA is **now supporting production traffic for any rollup**.

We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable the next generation of blockchain applications. Today, EigenDA has over **230 operators** and over **120,000 stakers** delegating over **3.4M ETH** to secure the network. Not only does this introduce the first alt-DA solution running in production secured by restaked ETH, it also holistically engages many pieces of the EigenLayer cryptoeconomic ecosystem to form a critical piece of infrastructure for the next era of services and applications.

For a limited time, any rollup may use EigenDA with throughput similar to production L2s for free. This is meant to showcase the advantages of the EigenDA product and be competitive with the EIP-4844 Dencun upgrade. For performance beyond these rates, and to reserve bandwidth in advance, please [get in touch with the team](https://forms.gle/NFfk1MTEj1NzhAPz5).

_Interested rollup customers should fill out the_ [**_EigenDA Contact Form_**](https://forms.gle/NFfk1MTEj1NzhAPz5) _in order to be approved for the initial free-tier usage phase. EigenDA currently supports ECDSA authentication to facilitate fast, early-stage integrations. Please provide the ETH address_ _associated with a new ECDSA keypair for authenticating your client with the EigenDA disperser. We strongly recommend that no funds be held by this contract address, and if you are building a rollup, that this is a different from your sequencer address; see_ [_documentation_](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/overview#authentication) _and_ [_usage tutorial_](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/disperser-golang-grpc-client) _for further usage guidance. If you’d like to learn more about our reserved bandwidth tiers, please_ [_get in touch_](https://forms.gle/NFfk1MTEj1NzhAPz5) _. As always, good places to get started are_ [_EigenDA documentation_](https://docs.eigenlayer.xyz/eigenda/rollup-guides/tutorial) _and the_ [_Holesky_](https://docs.eigenlayer.xyz/eigenda/networks/holesky) _testnet._

Onboarding the first rollups onto EigenDA mainnet is the next stage of a multi-stage launch towards the endgame for data availability for the Ethereum ecosystem. Read more at our [mainnet launch announcement](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/) about new brand new use cases enabled as well as EigenDA benefits such as hyperscalability, cost flexibility, ease of integration, and native token staking.

Please find official EigenLayer and EigenDA resources below:

- [EigenDA Documentation](https://docs.eigenlayer.xyz/eigenda/overview)
- [EigenDA Disperser Client Tutorial](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/disperser-golang-grpc-client)
- [EigenDA Authentication Guide](https://docs.eigenlayer.xyz/eigenda/integrations-guides/dispersal/overview#authentication)
- [EigenLayer App](https://app.eigenlayer.xyz/) (app.eigenlayer.xyz)
- [EigenLayer Documentation](https://docs.eigenlayer.xyz/) (docs.eigenlayer.xyz)
- [EigenDA GitHub](https://github.com/Layr-Labs/eigenda/)
- [Discord](https://discord.com/invite/eigenlayer) (discord.gg/eigenlayer)
- X: [@eigenlayer](https://twitter.com/eigenlayer), [@eigen\_da](https://twitter.com/eigen_da)
- [Immunefi Bug Bounties](https://immunefi.com/bounty/eigenlayer/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/#/portal)

SubscribeNow that EigenDA is live on [testnet](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/), we're proud to announce the first participants in the **EigenDA Launch Partner Program**. The first eight rollup infrastructure providers who are actively working to deploy EigenDA as a data availability option for their end users include: **AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.**

![Image](https://pbs.twimg.com/media/F_O3-JnboAA2aou?format=jpg&name=4096x4096)https://www.eigenlayer.xyz/ecosystem?category=rollups

Today we're kicking off a series of case studies on these EigenDA Launch Partners, starting with an examination of [**Layer N**](https://www.layern.com/) **,** which is building a rollup network called Nord.

## Nord - Rollups for High-Performance Finance

Nord, Layer N’s flagship product, introduces a specialized exchange rollup featuring a custom-built, hyper-efficient Rust orderbook execution environment. Nord impressively processes up to 100,000 requests per second with sub-millisecond latency, providing highly scalable computing power. This computing power makes it capable of supporting complex financial instruments—all while maintaining complete composability with EVM smart contract layers.

Specifically focused on addressing the limitations of existing DEX infrastructure, Nord, along with EigenDA, can provide performance equal to or better than centralized exchanges, but with additional benefits such as permissionlessness and customizability.

Nord plus EigenDA showcases the potential for decentralized financial infrastructure on Ethereum to compete and win against existing financial platforms.

### Addressing Existing Infrastructure Limitations

Decentralized trading platforms tend to be plagued by intrinsic limitations that obstruct efficiency and accessibility, such as slow transaction processing times, excessively high operational costs, and lack of composability with other blockchain applications. These shortcomings have been a pain point for both traders and developers and have slowed the advancement of DeFi ecosystems.

> In the words of the Layer N team, "Existing infrastructure for exchanges is either too slow, too expensive, or not composable."

Nord aims to solve these outlined problems by providing the first on-chain solution capable of rivaling centralized exchange performance, offering traditional finance features that users expect, such as cross-margin and cross-collateralization. Additionally, it provides customizable infrastructure randing from the settlement layer to the liquidation engine.

## EigenDA - Hyperscale DA for Rollups

EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum, utilizing the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA is the first actively validated service (AVS) to launch on EigenLayer.

### Why does DA matter?

DA solutions are pivotal in the Ethereum scaling roadmap. EigenDA builds upon core ideas and libraries underlying Danksharding, and sets a new standard for high throughput and low cost, enabling the growth of new on-chain applications with use cases across multiplayer gaming, social networks, and video streaming.

### EigenDA Testnet Launch

With the launch of the EigenLayer testnet Stage 2, EigenDA is now live for rollup developers to experience its high throughput and low-cost features firsthand, marking a significant milestone in its development and integration into the EigenLayer ecosystem.

## Nord & EigenDA - Technical Value Props

The Layer N team is excited to explore several key features offered by EigenDA, such as:

- EigenDA’s integration of multi-quorum and dual staking models to provide robust security and reliability for the network, alongside novel options for value capture.
- Performance metrics such as latency and throughput are critical considerations, especially for DEXs. EigenDA excels in these areas, offering low-latency data availability and high throughput, which are essential for delivering a seamless user experience.
- EigenDA also supports horizontal scaling, allowing the ecosystem to accommodate significant transaction and user growth without compromising its high-performance standards. This contributes to lowering transaction fees, reducing user cost burdens, and making decentralized applications more cost-effective and accessible to a wider audience.

Building upon EigenDA, Nord has the capacity to empower on-chain financial performance. With hyperscale throughput Nord can offer fixed, non-congestion pricing models for DeFi, so users benefit from predictable, stable, and low transaction costs. This ultimately improves the overall accessibility of decentralized financial services for all users worldwide.

As Nord experiences a growing surge of transactions, Layer N can dynamically scale DA requirements using EigenDA. This scalability ensures minimal to zero surge price pressure on Nord, creating an optimally user-friendly environment for traders.

## Conclusion

The collaboration between Layer N x EigenDA showcases the sheer power of EigenDA's hyperscale data availability layer and the types of innovative infrastructure that can be built to scale applications on Ethereum.

If you found this case study interesting and you want to get involved with EigenDA, please visit one of the links below:

- **Read the** [**EigenDA developer documentation and guides**](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-overview)
- **To learn more about EigenDA's design, read this** [**intro blog post**](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)
- **Get in touch with the EigenDA team via this** [**contact form**](https://contact.eigenda.xyz/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenda-layer-n/#/portal)

SubscribeEigenDA’s mission is to scale the decentralized world by making reliable, scalable, and secure data availability (‘DA’) abundant. In pursuit of this goal, and enabled by the scalability of its design, EigenDA strives to be the most price-performant DA solution.

**Today, we are pleased to announce a 10x reduction in EigenDA pricing for both on-demand and reserved bandwidth customers.**

In addition, we are introducing a ‘free tier’ with _up to 1.28KiB/s_ _in throughput_ for developers to get started on mainnet for up to 12 months.

## What does this mean for customers now?

_Note that both previous and new pricing will start to apply once payments go live later this year. Please_ [_get in touch with us_](https://forms.gle/GUTvVYNWCKBMU3BS9) _if you are launching soon._

As always, testnet remains free to use, but by default is rate limited to 3.1 KiB/s and testnet network performance can be highly variable. If you'd like to run tests at higher throughputs please [contact us](https://forms.gle/GUTvVYNWCKBMU3BS9) and we can work with you to design a performance and scalability test.

The new [EigenDA pricing](https://www.eigenda.xyz/#pricing) can be found below:

![](https://www.blog.eigenlayer.xyz/content/images/2025/04/image-1.png)

## Where do the bandwidth payments go?

EigenDA distributes most of the fees as rewards to the stakers and operators that power the network. Eigen Labs will use a portion of the fees to fund development and operating costs, which include cloud costs and gas fees paid to Ethereum L1 for bridging DA certificates.

## Next steps

Customers interested in testing or launching on mainnet, please fill out our [EigenDA Contact Form](https://forms.gle/Fn3Y2zGjuBrM3kfCA).

Please find official EigenLayer and EigenDA resources below:

- [EigenLayer App](https://app.eigenlayer.xyz/)
- [EigenDA Documentation](https://docs.eigenda.xyz/)
- [EigenLayer Documentation](https://docs.eigenlayer.xyz/)
- [EigenDA Github](https://github.com/Layr-Labs/eigenda/)
- [Discord](https://discord.com/invite/eigenlayer)
- X: [@eigenlayer](https://twitter.com/eigenlayer), [@eigen\_da](https://twitter.com/eigen_da)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/#/portal)

SubscribeEigenDA launched on testnet a few weeks ago, alongside the **EigenDA Launch Partner Program** featuring eight rollup infrastructure providers: **AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.**

We kicked off a series of case studies on these launch partners starting with [Layer N x EigenDA: A Case Study in Hyperscale DA for Finance.](https://www.blog.eigenlayer.xyz/eigenda-layer-n/) Today, we continue the series by examining [Versatus](https://versatus.io/), which is partnering with EigenDA to bring the world's first stateless rollup to Ethereum.

_To learn more about EigenDA,_ [_read documentation here_](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-overview) _, follow_ [_@eigen\_da_](https://twitter.com/eigen_da) _on X, and contact the EigenDA team at **vyas@eigenlabs.org** and **wes@eigenlabs.org**._

## Introduction to Versatus

Versatus is a P2P services provider building [LASR](https://versatus.medium.com/introducing-versatus-lasr-the-worlds-first-stateless-rollup-9327779b92fa), a decentralized execution environment that enables language-agnostic smart contracts, cross-chain native asset transfers, and cross-chain contract integrations. Versatus plans to deploy LASR to Ethereum within the next few months.

### Understanding LASR

The LASR rollup consists of three layers:

1. P2P Compute Network
2. Decentralized Compute Stack
3. Executable Oracle Contract

The Versatus **P2P Compute Network** is a distributed system for executing compute payloads on machines owned and operated by independent node operators. These operators can freely participate in the network without needing permission or whitelisting. This network supports various web services, including serverless backends, decentralized service hosting, and on-demand computing.

The Versatus **Decentralized Compute Stack** is a layered application with nano-runtime enabling creation, execution, and destruction. It consists of a Compute Agent, a Container Runtime, a Unikernel Virtual Machine, and a Language Runtime. These layers enable secure, lightweight, architecture and platform-agnostic applications. The stack can also integrate with runtimes such as ZK (Zero Knowledge) and FHE (Homomorphic Encryption) runtimes for enhanced security.

The **Executable Oracle Contract** plays a pivotal role in freezing state data at a specific point in time. It sends signals to the compute network to execute operations using programs built on the compute stack. The contract incorporates six Versatus modules, each serving essential roles. Let’s break them down:

1. The Event Emitter is responsible for transmitting data to the Versatus Network.
2. The Escrow feature ensures that relevant data is frozen at a specific point in time. This guarantees the availability of funds, preventing race conditions on the base chain.
3. The MetaFactory aggregates Factory Contracts generating base chain contracts as needed. It primarily focuses on creating tokens and assets. For example: If a token is created on Versatus, it will only exist on the base chain once initiated by the Versatus Executable Oracle.
4. The instruction ingestor, which operates under ‘ **OwnerOnly**’ permissions, receives data from the Oracle account and processes it within the contract. It converts this information into data, determining how and where escrowed assets should be dispersed.
5. The Disperser also operates exclusively under ‘ **OwnerOnly**’ permissions and serves as the mechanism for releasing assets from escrow.
6. Finally, we have the Oracle Account owned by the Executable Oracle, which functions as an MPC Wallet that verifies the threshold signatures of Versatus network compute quorums. This account passes data to the Instruction Ingestor for activation of operations when required.

### SWOT Analysis

Below is an assessment of strengths, weaknesses, opportunities, and threats from the project's perspective:

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/SWOT2.png)

This analysis reveals two key trade-offs in the project:

1. Versatus enables cheaper complex and longer running compute, in exchange for slightly more expensive same chain asset transfers and swaps.
2. Versatus enables a vastly better developer experience and seamless onboarding due to the language-agnostic smart contracts, in exchange for ex-post settlement network-wide proofs.

### The Versatus Mission

Versatus is building scalable infrastructure that ensures censorship resistance, and protects user data and privacy, while enabling seamless interoperability between current web-based systems to meet growing user demand.

By aiming to revolutionize decentralized computing, Versatus will improve accessibility and versatility with language-agnostic smart contracts, and pioneer bridgeless interoperability, to simplify the process of asset and data transfers between blockchains. Integrating EigenDA will reinforce this mission, by supporting enhanced network reliability and security.

## EigenDA - Hyperscale DA for Rollups

EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum, utilizing the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA is the first actively validated service (AVS) to launch on EigenLayer.

### Why does DA matter?

DA solutions are pivotal in the Ethereum scaling roadmap. EigenDA builds upon core ideas and libraries underlying Danksharding, and sets a new standard for high throughput and low cost, enabling the growth of new on-chain applications with use cases across multiplayer gaming, social networks, and video streaming.

### EigenDA Testnet Launch

With the launch of the EigenLayer testnet Stage 2, EigenDA is now live for rollup developers to experience its high throughput and low-cost features firsthand, marking a significant milestone in its development and integration into the EigenLayer ecosystem.

## Versatus & EigenDA - Technical Value Props

LASR’s statelessness requires validators to reconstruct transactions with relevant data at each transaction’s timestamp. EigenDA enables this process, which is impractical with alternative base chain data availability solutions. Using EigenDA, Versatus stores “snapshots” of relevant data to each transaction, enabling a network of stateless watcher nodes to sample from EigenDA, fully recreate transactions, and prove the validity of the transactions.

EigenDA benefits from its collaboration with Versatus given LASR's focus on bringing in new developers to Web3. Versatus expects that its developer community will create innovative new on-chain use cases, especially ones that require high throughput and demand for data availability. EigenDA, with its cost efficiency, scalability, and flexibility, is ideally positioned to meet this increased demand effectively.

### Conclusion

With the combination of the LASR rollup and data availability via EigenDA, Versatus and EigenDA hope to contribute to solving Web3 challenges such as censorship-resistance, data privacy, and interoperability.

The collaboration between our teams will require deep research and design around trade-offs such as the costs associated with various transaction types on LASR, and the need for ex-post settlement proofs. Despite these problems to be solved, the teamwork between Versatus and EigenDA holds enormous potential for advancing decentralized technology and contributing to significant growth within the Ethereum ecosystem.

_To learn more about EigenDA,_ [_read documentation here_](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-overview) _, follow_ [_@eigen\_da_](https://twitter.com/eigen_da) _on X, and contact the EigenDA team at **vyas@eigenlabs.org** and **wes@eigenlabs.org**._

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenda-versatus/#/portal)2024 was a monumental year for EigenLayer.

Since launching our first staking contracts in June 2023, and following the full mainnet deployment of the EigenLayer protocol in April 2024, we’ve made great strides toward our vision of building coordination engines for driving open innovation.

We launched the **EigenLayer protocol**, which introduced restaking, a powerful new primitive enhancing and scaling crypto-economic security.

We built the **Eigen Ecosystem**, the fastest growing (fastest dev ecosystem according to [Electric report](https://www.developerreport.com/developer-report?s=thank-you-to-the-339-people-who-contributed-via-github-1)) ecosystem of builders, stakers, and operators.

We bootstrapped the **Eigen Economy**, an open marketplace for innovation built on top of EigenLayer, creating billions of dollars of value.

We introduced the **EIGEN Token** and scaled the [**Eigen Foundation**](https://eigenfoundation.org/).

We introduced [**EigenGov**](https://blog.eigenfoundation.org/introducing-eigengov-part-1/) and the [**Protocol Council**](https://blog.eigenfoundation.org/the-protocol-council/), essential steps forward in establishing the ecosystem's governance framework.

Here's a look at what we've accomplished together this year and how we’re shaping the industry's future.

* * *

## **What We’ve Done**

### Protocol Accomplishments

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdeytUsiiqYa3F3DjLlR7jwIL9mn_tUFwNVlIO-n3grDvuZNJI_X5bwUYykmZGlvr9-bKT-c3UVzYzJxMK5jYsMWjuxRQgIa6qv8fpW10zhZ-RRrsnrcQl01cBZJLWNYwoeFJMiyw?key=5mycsYKpHEV-aXeOk5Vce89r)

### **Ecosystem Accomplishments**

- **Fastest-Growing Developer Ecosystem:** EigenLayer was recognized as the fastest-growing developer ecosystem in 2024 (source: [Developer Report](https://www.developerreport.com/developer-report?s=thank-you-to-the-339-people-who-contributed-via-github-1)).

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdexbb_1Vg2uiEuU0BYbaDsQ8x3M5Czx6nuaxZ7kz_xG6PKOhhi0EItmifd0li_w9ynKIO4MZw3Jp3q8nDwXR206vjiHuCXa7wyXC-jqoOov6M8cKefvswiKdUecl3cuAfgSsGq?key=5mycsYKpHEV-aXeOk5Vce89r)

- **Thriving** [**AVS Landscape:**](https://app.eigenlayer.xyz/avs)
  - **130+ Active AVSs** in development or in testnet
  - **29 Mainnet AVS** driving innovation and ecosystem development
- **Integrations:** Collaborations with **14 major partners,** including [Consensys](https://x.com/eigenlayer/status/1857500575715176449) (DIN), [LayerZero](https://www.blog.eigenlayer.xyz/dvn/), and [RiscZero](https://x.com/eigenlayer/status/1859674662969671776).
- **Community Growth:**
  - ~200,000 [Discord](https://discord.com/invite/eigenlayer) members and a vibrant forum with over 8,500 users.
  - ~400,000+ Twitter followers across [EigenLayer](https://x.com/eigenlayer), [EigenDA](https://x.com/eigen_da), and [BuildonEigen](https://x.com/buildoneigen)

### **Ecosystem Accomplishments**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeu4r-LBE7gJi9JOSGv6zTVaqgVzPMVxFC3KIkPQ982TVAh3ha7N5reh8Y-doMixPHoTSunbJiXhNCtaK-twBTzq-ocnxIGRCHV4KmepMR7WWDwvmglWjfLexfWqltrV71-2LWpBg?key=5mycsYKpHEV-aXeOk5Vce89r)

### Token & Eigen Foundation Milestones

The EIGEN token launch marked the beginning of a new era in infrastructure innovation:

**Season 1 Achievements:**

- 113M EIGEN distributed to 280,000+ community members
- 88% of tokens staked and delegated immediately
- Eigen Foundation established itself as the EigenLayer ecosystem steward

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcuKXBMF5c_S3BBkJMfy6Eqtw14KwtGWFwh76VlrpusEfHKxhdkHF1xMjmHhIMc2N2FlfYD0LbO_-1qXwtvW5Yb_AJQpOIuELS_RoyB0UpwSbhJTsQ7mzuyiWjMcBGbsghZK3BgFA?key=5mycsYKpHEV-aXeOk5Vce89r)

**Season 2 Distribution:**

- 70M EIGEN allocated to Stakers and Operators
- 10M EIGEN fueling Ecosystem Partners
- 6M EIGEN empowering Community initiatives

At the conclusion of the Season 2 stakedrop, [we recognized](https://blog.eigenfoundation.org/ecosytem-partners/) the contributions of 110 incredible ecosystem partners building across AVSs, infrastructure tooling, and core protocol integrations.

Since then, we’ve more than doubled (and growing) the number of partners developing in the EigenLayer ecosystem!

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdIcAF5R0QGLtHi0qvl0VC2WFOvQ_WJSmNChnIlmqQulIu0R7Gyk00kbFFPdQyBovUnhc5sLfoB-ncbnuxS6fht51phIHnn-DfXaf80L1XEjtvk4LV37gvhY4CidZo--0u1BHkpPA?key=5mycsYKpHEV-aXeOk5Vce89r)

**Programmatic Incentives**

Following Season 1 and 2 Stakedrop, we released Programmatic Incentives, a significant milestone for EigenLayer and the broader restaking ecosystem as we transitioned from retroactive incentives (points and stakedrops) towards continuous liquid rewards via Programmatic Incentives, ultimately creating a market for price discovery of shared security.

Since Programmatic Incentives launched in October 2024:

- **24,460,000+ EIGEN** was rewarded and made eligible for claims
- **140,000+ Restakers** qualified to earn Programmatic Incentives v1

### **Governance Evolution**

November marked a crucial milestone with the launch of the [EigenGov](https://blog.eigenfoundation.org/introducing-eigengov-part-1/) framework:

- Established council-based coordination engine
- Implemented community-driven oversight
- Developed Core/Vision development tracks
- Activated Protocol Council with 5 expert members and 3-of-5 community multisig
- Launched [ELIP](https://github.com/eigenfoundation/ELIPs) framework

### **Ethereum Ecosystem Impact**

The Eigen Foundation strengthened core Ethereum infrastructure:

- [Committed 1% of EIGEN](https://blog.eigenfoundation.org/eigen-foundation-protocol-guild/) supply (~16.7M tokens) to Protocol Guild
- Supported 180+ Ethereum core developers
- Backed 29 teams building L1's critical infrastructure

* * *

### **How**

**Our Values**

1. **Unreasonable Goals:** From launching mainnet to onboarding tens of thousands of restakers reaching $20B+ in TVL, we’ve set ambitious targets and achieved them.
2. **Relentless Execution:** Our team’s dedication has driven the rapid rollout of major upgrades and ecosystem growth in under 12 months.
3. **Unparalleled Win-Wins:** By aligning incentives across developers, operators, stakers, and partners, we’ve created a thriving ecosystem where everyone succeeds together - infinite sum games.

* * *

### **Why**

**Driving Open Innovation**

EigenLayer exists to build a future of **open innovation**—a world where infrastructure is modular, decentralized, and interoperable, enabling participants to create, collaborate, and scale freely. By empowering developers, operators, and stakers, we’re unlocking new avenues for trustless coordination and economic opportunity on Ethereum and beyond.

* * *

### **Where**

**EigenLayer is a global effort.**

In 2024, the EigenLayer Ecosystem and Economy expanded to over 100+ countries.

EigenLayer's vision resonated across continents through strategic events:

- AVS Day Brussels: Partnered with Consensys to host 25 pioneering AVS teams and 150 ecosystem builders
- Token2049 Singapore & Dubai: Collaborated with Eigen Labs and Eigen Foundation to present restaking revolution keynote and LayerZero DVN framework launch
- MegaZu Thailand: 30-day innovation hub focusing on consumer applications and infrastructure development
- Devcon – AVS Day Bangkok: Featured live AVS demonstrations and ecosystem deep-dives with Consensys and AltLayer

* * *

### **Who**

[**Eigen Labs**](https://www.eigenlabs.org/) **:** Grew from 38 to 95 team members this year, as core developers of the EigenLayer protocol.

[**Eigen Foundation:**](https://blog.eigenfoundation.org/) Announced to the world in April 2024, the Eigen Foundation now boasts a dedicated team of ~30 people driving governance and ecosystem initiatives helping steward the growth and decentralization of the EigenLayer ecosystem.

**EigenLayer Ecosystem:** Our accomplishments wouldn’t be possible without the incredible **developers, stakers, operators, AVSs, and partners** who make up the EigenLayer Ecosystem.

* * *

2024 has been an extraordinary year, and we’re just getting started.

As we look to 2025, we’re excited to continue pushing the boundaries of what’s possible in open innovation. Thank you for being part of this journey.

Together, we’re building the future.

**Be Eigen.**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfVxpWVy4E_u5mxXNcGyrK2XyF1i53FB0ZgczZa_ZYIqOO1oBLh8QKSeZjPq9K9uC3YgniYfcJQo0KjPGY2hSJCCHucU-colrapO5BfnpFrh88WAwKTTLyXw0Tnl6byNpBdgbrDXg?key=5mycsYKpHEV-aXeOk5Vce89r)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-2024/#/portal)

SubscribeOn August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. **After the caps are lifted, users will be able to deposit any one of these tokens into EigenLayer. At the point any LST reaches amilestone of 100k tokens restaked, a global pause will be implemented.**

**_For example: after the caps are lifted, imagine a scenario in which all three tokens have 50k restaked. All deposits will remain open until one of those tokens reaches 100k restaked. If none of them reach 100k restaked, deposits will remain open indefinitely._**

This increase supports the EigenLayer mission to broaden access to restaking opportunities while maintaining the highest standards of asset safety and security.

[Native staking](https://docs.eigenlayer.xyz/guides/mainnet-restaking/native-restaking) for beacon chain validators remains uncapped.

## Details of the Cap Increase

Following the previous successful increase of LST caps, we are delighted to announce another significant expansion. The cap for LSTs, including stETH, rETH, and cbETH, will now be lifted until any one of the LSTs hits 100k tokens restaked. This expands restaking capacity and enables a more extensive network of users to actively participate in restaking endeavors.

The procedure for this cap increase will follow a similar governance process previously undertaken:

**Protocol Parameter Change Approval:** The change will need to be approved through our Multisignatory Governance system.

**10-Day Timelock:** Once approved, the Operations Multisig transaction to raise the caps will be queued in a 10-day timelock, aligning with the governance processes and security protocols.

**Triggering the Action:** After the 10-day timelock period, the Operations Multisig will trigger the action from the timelock contract, initiating the parameter changes and executing the modifications.

**Triggering the Pause:** If any one LST hits approximately 100k ETH restaked, the Pauser Multisig will trigger a global pause.

## A Resilient Ecosystem

This substantial cap increase reflects EigenLayer's continued commitment to network security, decentralization, and the enhancement of the user restaking experience. The measured rise in LST caps, in combination with unrestricted native restaking, ensures a robust and tested ecosystem where users can confidently restake their assets.

**Spotlight on Prominent AVSs: Diving Deeper into the Ecosystem:**

As we continually evolve, we recognize the importance of various Actively Validated Services (AVSs) within our ecosystem. Here's a brief overview of some notable AVSs:

**EigenDA:** Focuses on hyperscale data availability for any execution layer. Notable EigenDA adopters include Celo, Mantle, Rollup-As-a-Service providers and more. This AVS is being developed by EigenLabs.

[**Espresso Systems**](https://www.espressosys.com/) **:** Offers decentralized sequencing on EigenLayer and captures value through cross-rollup MEV. Backed by Yale's Ben Fisch and investors like Greylock and Sequoia.

[**Lagrange**](https://www.lagrange.dev/) **:** Known for ZK-powered cross-chain messaging using state committee, leveraging EigenLayer restaking for dynamic scaling of state proof security.

[**Omni Network**](https://omni.network/) **:** Provides a fast-finality chain for cross-rollup messaging, utilizing the Cosmos SDK. Supported by a Harvard-origin team and investors such as Pantera.

[**Blockless**](https://blockless.network/) **:** Delivers ZK verifiable serverless functions, optimizing decentralized applications. It uses EigenLayer to secure transaction consensus.

[**Hyperlane**](https://www.hyperlane.xyz/) **:** Employs EigenLayer to safeguard interchain application developers’ messages from Ethereum using restaked economic security.

## Looking Ahead: A Deep Dive into the Operator Testnet and M2 Testnet Features

**Operator Testnet: Elevating Our Vision**

As EigenLayer progresses in its journey, we're elated to unveil the upcoming launch of the Operator Testnet. This pivotal testnet serves multiple purposes:

**Validation Services for EigenDA:** Operators get an opportunity to provide validation services for EigenDA.

**Node Operation:** Prospective operators can step up and register as an EigenLayer node operators using the dedicated Operator CLI.

**Performance and Maintenance Tools:** The Operator Testnet is well-equipped with features tailored for operators. They can monitor node performance, effortlessly transition to the latest software versions for both EigenLayer and EigenDA AVS, and gracefully decommission operations.

This testnet essentially acts as a litmus test, ensuring operators can effectively engage with the EigenDA ecosystem, upholding our standards of functionality and security.

## M2 Testnet Features: Enhancing Delegation Experiences

The horizon also brings forth an exhilarating expansion of our Operator Testnet, spotlighting the delegation facet:

**Staker-to-Staker Delegation:** This phase empowers stakers, granting them the autonomy to delegate or retract delegations on the EigenLayer Testnet.

**Operator Delegation via CLI:** A streamlined process, where stakers can effortlessly delegate and un-delegate to operators, leveraging the command-line interface.

As we traverse this delegation journey, our dedication to user-centricity remains unwavering. We have slated user testing sessions, striving for an intuitive and seamless delegation experience for every participant.

The inception of the Operator Testnet underscores EigenLayer's unwavering commitment to fostering innovation, championing decentralized validation, and sculpting a fortified restaking infrastructure. Our aim always remains to resonate with the aspirations and needs of the vibrant EigenLayer community. We invite you to stay engaged, as we unveil more detailed insights about participation and ongoing progress.

Don’t miss out on this opportunity to be part of the restaking collective. Together, we are shaping the future of restaking!

Website: [eigenlayer.xyz](https://eigenlayer.xyz/)

App: [app.eigenlayer.xyz](https://app.eigenlayer.xyz/)

Documentation: [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz/overview/readme)

Discord Community: [discord.gg/eigenlayer](https://discord.com/invite/eigenlayer)

Forum: [forum.eigenlayer.xyz](https://forum.eigenlayer.xyz/)

Twitter: [twitter.com/eigenlayer](https://twitter.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/#/portal)

SubscribeETHDenver 2025, the world's largest Web3 event, brings together builders, researchers, and innovators from across the globe. EigenLayer will be hosting and participating in 30+ events over 10 days, ranging from technical workshops and hackathons to keynote presentations and social gatherings.

This guide organizes EigenLayer's presence at ETHDenver into four main categories:

- Main Conference Programming
- Co-Sponsored Conferences
- Featured Speaking Engagements
- Hackathons & Builder Events

Many events require pre-registration, and locations will be revealed upon approval. Visit [lu.ma/eigenlayer](http://lu.ma/eigenlayer) for a preview of EigenLayer-sponsored events. Below is our comprehensive guide to all EigenLayer activities at ETHDenver 2025:

## 🏟️ Main Conference Programming

### EigenLayer Experience

As a Presenting Sponsor of ETHDenver 2025, EigenLayer will be at the center of the action. Here are our main conference highlights:

Twitter Embed

[Visit this post on X](https://twitter.com/EthereumDenver/status/1882992817594925090?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

[![](https://pbs.twimg.com/profile_images/1821612191856128005/KiqsSUwm_normal.jpg)](https://twitter.com/EthereumDenver?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

[ETHDenver ![🏔](https://abs-0.twimg.com/emoji/v2/svg/1f3d4.svg)![🦬](https://abs-0.twimg.com/emoji/v2/svg/1f9ac.svg)![🦄](https://abs-0.twimg.com/emoji/v2/svg/1f984.svg)](https://twitter.com/EthereumDenver?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

[@EthereumDenver](https://twitter.com/EthereumDenver?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

·

[Follow](https://twitter.com/intent/follow?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F&screen_name=EthereumDenver)

[View on X](https://twitter.com/EthereumDenver/status/1882992817594925090?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

We're thrilled to have [@eigenlayer](https://twitter.com/eigenlayer?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F) on board as a Presenting Sponsor for ETHDenver 2025! ![🙌](https://abs-0.twimg.com/emoji/v2/svg/1f64c.svg)

ETHDenver is about innovation, collaboration, and pushing Web3 forward.

Thank you to [@eigenlayer](https://twitter.com/eigenlayer?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F) and all of our sponsors for making this possible!

[![Image](https://pbs.twimg.com/media/GiG69Y4XQAA21i5?format=png&name=small)](https://x.com/EthereumDenver/status/1882992817594925090/photo/1)

[3:24 AM · Jan 25, 2025](https://twitter.com/EthereumDenver/status/1882992817594925090?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

[X Ads info and privacy](https://help.twitter.com/en/twitter-for-websites-ads-info-and-privacy)

[204](https://twitter.com/intent/like?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F&tweet_id=1882992817594925090) [Reply](https://twitter.com/intent/tweet?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F&in_reply_to=1882992817594925090)

Copy link

[Read 16 replies](https://twitter.com/EthereumDenver/status/1882992817594925090?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1882992817594925090%7Ctwgr%5Edec93d2da1f860d48e3c22c614c54e17f1c443e9%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Feigenlayer-at-ethdenver-2025-complete-event-guide%2F)

👋 **Interactive Eigen Booth**

📍 National Western Complex \| 🕒 Feb 27 - Mar 1, 9:00 AM - 6:00 PM \| 🔗 Open Access

- **For:** Builders, investors, and curious minds
- **Key Takeaway:** Chat with the team, earn top-tier swag (hoodies, shirts, hats, stickers, nails), and learn about EigenLayer.

🎙️ **Eigen** **Podcast Studio**

📍 Near Castle Stage \| 🕒 Feb 27 - Mar 1, 10:00 AM - 6:00 PM \| 🔗 Limited slots, request an interview by emailing podcasts@eigenfoundation.org

- **For:** Ecosystem partners, community ambassadors, and content creators
- **Key Takeaway:** Get featured on EigenLayer's content channels and share your story with our global community.


☕ **Eigen** **Coffee Lounge**

📍 National Western Complex \| 🕒 Daily 10AM-4PM \| 🔗 Open Access

- **For:** Anyone needing a caffeine boost
- **Key Takeaway:** The _only_ place serving coffee at the conference – fuel up with a coffee on us while networking in quiet work zones.


### Main Conference Speaker Sessions

🎤 [**ETHDenver Opening Ceremony Talk**](https://www.ethdenver.com/sessions/recPcL794WU1nVRlq)

📍 Stage: TBD \| 🕒 Feb 27 11:30 AM \| 🔗 General admission

- **Speaker:** Sreeram Kannan
- **For:** All ETHDenver attendees and crypto enthusiasts
- **Key Takeaway:** A powerful 7-minute vision of how crypto will transform human coordination

🎤 [**Keynote - Supercharging AI Possibilities: The Next Frontier in Crypto**](https://www.ethdenver.com/sessions/rec26PYUAAhikaoEg)

📍 REGENERATOOOR Stage \| 🕒 Feb 28 1:00PM \| 🔗 General admission

- **Speaker:** Sreeram Kannan
- **For:** AI and blockchain professionals
- **Key Takeaway:** Exploration of AI-blockchain convergence opportunities


🎤 [**Keynote - EigenLayer: Our Journey to Building Humanity's Coordination Engine**](https://www.ethdenver.com/sessions/recPcL794WU1nVRlq)

📍 BUIDL Squad Stage \| 🕒 Feb 28 4:15PM \| 🔗 General admission

- **Speaker:** Sreeram Kannan
- **For:** All ETHDenver attendees and crypto enthusiasts
- **Key Takeaway:** Comprehensive overview of EigenLayer's vision and roadmap


🎤 [**Keynote - Meta-Settlement: Manifesto for Open Source Builders**](https://www.ethdenver.com/sessions/recG3h3dGHzqjx8HT)

📍 REGENERATOOOR Stage \| 🕒 Mar 1 1:45PM \| 🔗 General admission

- **Speaker:** Robert Drost
- **For:** Open source advocates and Web3 builders
- **Key Takeaway:** A rallying cry for developers to use meta-settlement and blockchain tools in the fight against proprietary software monopolies


🎤 [**Panel - Restaking Revolution: Redefining Infrastructure Efficiency**](https://www.ethdenver.com/sessions/reciKB2uwmymrjqJh)

📍 Captain Ethereum Stage \| 🕒 Mar 1 1:45PM \| 🔗 General admission

- **Speakers:** Sreeram Kannan (EigenLayer), Jake Hartnell (Layer), Sydney Lai (Gaia)
- **For:** Infrastructure builders, protocol developers, and anyone interested in Web3 scalability
- **Key Takeaway:** Panel focusing on the evolution of restaking and how it’s creating new possibilities for protocol development


* * *

## 🎟️ EigenLayer Side Events (Co-Sponsored)

_(in chronological order)_

🔄 [**Restaking Circle**](https://lu.ma/restaking_denver)

📍 Register to See Address \| 🕒 Feb 24, 12:00 PM - 7:00 PM \| 🔗 Waitlist

- **Co-Sponsors:** [Staking Circle](https://x.com/stakingcircle)
- **For:** Liquid staking and restaking professionals
- **Key Takeaway:** Premier conference with 5 panels, 3 keynotes, and 6 workshops on LRTfi and BTCfi
- **Featured Speaker:** Nader Dabit - Topic TBD at TBD


⚡ [**Appchains Power Lunch**](https://lu.ma/cumn73p5)

📍 Register to See Address \| 🕒 Feb 25, 12:00 PM - 3:00 PM \| 🔗 Open registration

- **Co-Sponsors:** Syndicate, Alchemy, EigenLayer, Halliday, Hyperlane, Eco, The Rollup, Refraction, Slow Rodeo, Scene Infrastructure, Rarible, Fraxtal, Boost, Amino Chain, and more
- **For:** App builders and scaling enthusiasts
- **Key Takeaway:** Lightning talks and unconference discussions on appchains' role in Ethereum scaling
- **Featured Speaker:** Sreeram Kannan - TBD at 1:00 PM


🔄 [**Rollup & Prove It**](https://lu.ma/506nul3h)

📍 Blanc \| 🕒 Feb 25, 10:00 AM - 5:00 PM \| 🔗 Open registration

- **Co-Sponsors:** [Polymer Labs](https://x.com/polymer_labs)
- **For:** Rollup developers and infrastructure builders
- **Key Takeaway:** Interactive debates and demos exploring Ethereum interoperability
- **Featured Speaker:** Nima Vaziri - "Rollups and Verifiable AI" at TBD


🧠 [**Open AGI Summit**](https://lu.ma/asjsb6k7)

📍 Register to See Address \| 🕒 Feb 26, 10:00 AM - 6:00 PM \| 🔗 Open registration

- **Co-Sponsors:** [AWS](https://aws.amazon.com/), [Polygon](http://polygon.technology/), [Sentient](http://sentient.foundation/) and [Polygon](https://polygon.technology/)
- **For:** AI and crypto enthusiasts
- **Key Takeaway:** Deep dive into AI agents, DeFAI, and DeAI infrastructure
- **Featured Speakers:**
  - Sreeram Kannan - Topic TBD at 11:30 AM
  - Nader Dabit - Topic TBD at TBD

🔬 [**The Intersubjective Summit**](https://lu.ma/qzfwxaoj)

📍 Register to See Address \| 🕒 Feb 26, 11:00 AM - 5:00 PM \| 🔗 Open registration

- **Co-Sponsors:** [Hetu](https://x.com/hetu_protocol), [EthStorage](https://eth-store.w3eth.io/), [Nethermind](https://www.nethermind.io/)
- **For:** DeAI and DeSci professionals
- **Key Takeaway:** Exploring human-AI collaboration and decentralized science
- **Featured Speaker:** Brandon Curtis - "Consensus Design: Does It Have to Be Purely Objective?" at 11:00 AM


🏗️ [**Builder Nights Denver**](https://lu.ma/builddenver)

📍 Register to See Address \| 🕒 Feb 27, 6:00 PM - 11:00 PM \| 🔗 Open Registration

- **Co-Sponsors:** MetaMask, Linea, Gaia, Wormhole, Brevis, and CrossFi
- **For:** Web3 developers and thought leaders
- **Key Takeaway:** Technical discussions and networking with industry leaders
- **Featured Speaker:** Nader Dabit - Topic TBD at TBD


🤖 [**AI Hype(r)House by Hyperbolic + EigenLayer**](https://lu.ma/hyperhouse)

📍 Register to See Address \| 🕒 Feb 28, 11:00 AM - 11:00 PM \| 🔗 Open registration

- **Co-Sponsors:** [Hyperbolic](https://x.com/hyperbolic_labs)
- **For:** AI researchers and developers
- **Key Takeaway:** Full day of AI agent development workshops and panels
- **Featured Speakers:**
  - Sreeram Kannan - "Verifiable AI Agents" at 3:10 PM
  - Nader Dabit - On the Blockchain Panel at 2 PM
  - Wes Floyd- "Developer Workshop: Building AI agents on top of EigenLayer using Hyperbolic" at 12:20 PM

🔒 [**Proof Point**](https://lu.ma/proofpoint)

📍 Register to See Address \| 🕒 Feb 28, 12:00 PM - 6:00 PM \| 🔗 Open registration

- **Co-Sponsors:** Lagrange, Polygon, Caldera, Hyle, Opacity, The Rollup
- **For:** Founders, builders, and ZK enthusiasts focused on the future of privacy, scalability, and security in Web3
- **Key Takeaway:** Unfiltered discussions on ZK advancements, privacy, and scalability
- **Featured Speaker:** Sreeram Kannanat 4:10 PM


🔗 [**AltLayer AI Rollup Day**](https://lu.ma/AltLayerDoesDenver)

📍 The Curtis Denver, 3rd Floor \| 🕒 Feb 28, 11:30 AM - 5:00 PM \| 🔗 Open Registration

- **Co-Sponsors:** [AltLayer](https://x.com/alt_layer)
- **For:** Web3 builders, AI developers, and infrastructure enthusiasts interested in scaling solutions and agentic computing
- **Key Takeaway:** Deep dive into blockchain interoperability, AI infrastructure, and the integration of Agentic Web into Web3 with 30+ industry leaders
- **Featured Speaker:** Sreeram Kannan - Keynote at 11:30 AM


* * *

## 🎤 Featured Speaking Engagements

_(in chronological order)_

🔒 [**NoirCon 1: Privacy, AI, and App Building**](https://lu.ma/38g79n99)

📍 Catalyst \| 🕒 Feb 24, 11:00 AM - 5:30 PM \| 🔗 Open registration

- **For:** Privacy-focused developers and ZK researchers
- **Key Takeaway:** Practical workshops on building privacy-preserving applications with Noir and ZK tools
- **Featured Speaker:** Wes Floyd at 11:00 AM


🔄 [**Restaking & DeFAI Day**](https://lu.ma/restakingdenver25)

📍 Hilton Garden Inn \| 🕒 Feb 25, 10:00 AM - 5:00 PM \| 🔗 Open registration

- **For:** DeFi developers and AI enthusiasts
- **Key Takeaway:** 5th edition featuring 15+ speakers from leading protocols
- **Featured Speaker:** Nima Vaziri - "AI as DeFi's New Interface: The Abstraction Revolution" at 11:30 AM


🟣 [**Solana Ecosystem Summit**](https://lu.ma/mhpreh2z)

📍 Rise Comedy \| 🕒 Feb 26, 9:00 AM - 6:00 PM MST \| 🔗 Open registration

- **For:** Solana developers and ecosystem participants
- **Key Takeaway:** Comprehensive overview of Solana's ecosystem featuring panels on RWA, DeFi, Infrastructure, and AI
- **Featured Speaker:** Nima Vaziri - Panel on "Staking and Restaking" at 1:30 PM


🏢 [**Innovate Denver**](https://www.innovate.thetie.io/)

📍 Tom's Watch Bar \| 🕒 Feb 26, 9:00 AM - 4:30 PM \| 🔗 Invitation only

- **For:** Institutional investors and builders
- **Key Takeaway:** Connecting institutions with blockchain innovators
- **Featured Speaker:** Robert Drost - "You Get a Chain, You Get a Chain, You Get a Chain" at 12:00 PM


🏗️ [**DePIN Day Denver**](https://lu.ma/depin-denver)

📍 Register to See Address \| 🕒 Feb 26, 10:00 AM - 7:00 PM \| 🔗 Open registration

- **For:** DePIN founders and researchers
- **Key Takeaway:** Latest developments in decentralized physical infrastructure
- **Featured Speaker:** Wes Floyd - "DePin and Web3 Infra" at 3:00 PM


🥩 [**FOMO Factor: EP1 - Re/Staking**](https://lu.ma/3dt9k2t3)

📍 Register to See Address \| 🕒 Feb 26, 12:00 PM - 6:00 PM \| 🔗 Open Registration

- **For:** Web3 professionals and staking enthusiasts
- **Key Takeaway:** Expert panels with exclusive access to "The Whale's Den" project pitches
- **Featured Speaker:** Nader Dabit - "Future of Restaking" at TBD


🤝 [**Schelling Point Keynote**](https://schellingpoint.gitcoin.co/)

📍 Number Thirty Eight \| 🕒 Feb 27, 10:00 AM - 6:30 PM \| 🔗 Open registration

- **For:** Developers, activists, and blockchain enthusiasts
- **Key Takeaway:** Exploring cryptocurrency's impact on social good
- **Featured Speaker:** Sreeram Kannan - "Humanity's coordination engine" at TBD


🌲 [**BASS Denver 2025**](https://lu.ma/xbrjx2nn)

📍 Register to See Address \| 🕒 Feb 27, 9:00 AM - 6:30 PM \| 🔗 Open registration

- **For:** Stanford blockchain community and enthusiasts
- **Key Takeaway:** Latest developments in blockchain technology
- **Featured Speaker:** Robert Drost - "Developments with Eigenlayer on AI agents" at 3:00 PM


🎓 [**Cube Summit**](https://cubesummit.xyz/)

📍 TBD \| 🕒 Feb 28 \| 🔗 University focus

- **For:** University blockchain clubs and emerging talent
- **Key Takeaway:** Connecting student innovators with leading projects and investors
- **Featured Speaker:** Nader Dabit - Topic TBD at TBD


* * *

## 🛠️ Hackathons & Builder Events

🤖 [**The Vault: AI Agent Hacker House w/ ElizaOS**](https://lu.ma/536qhuqb)

📍 Register to See Address \| 🕒 Feb 22 - Mar 3, 10AM - 9PM Daily \| 🔗 Application required

- **For:** Talented developers worldwide building next-gen AI agents
- **Key Takeaway:** 9-day builder residency focused on next-gen AI agent development
- **Daily Highlights:**
  - Morning: Technical workshops with industry experts
  - Afternoon: Collaborative building sessions
  - Evening: Progress demos and networking

⚔️ [**Eigen Games University Hackathon**](https://lu.ma/j0l0705e)

📍 Register to See Address \| 🕒 Feb 22 - Mar 1, 2025 \| 🔗 [Accepting virtual applications](https://collegedao.typeform.com/TheEigenGames)

- **For:** 40+ university blockchain clubs worldwide
- **Key Takeaway:** The largest university competition in Web3 history, where 40+ universities compete head-to-head for a $100,000 prize pool, culminating in a high-stakes pitch day to industry veterans

**Event Schedule:**

- [**Workshop Day 1**](https://lu.ma/j0l0705e): Expert-led talks and hands-on technical sessions focused on AI, AVS, DeFi, and ZK-powered applications
- [**Workshop Day 2**](https://lu.ma/ov8h9jzt): Expert-led talks and hands-on technical sessions focused on AI, AVS, DeFi, and ZK-powered applications
- [**Workshop Day 3**](https://lu.ma/vd54wuor) **:** Expert-led talks and hands-on technical sessions focused on AI, AVS, DeFi, and ZK-powered applications
- [**Investor Pitching Day**](https://lu.ma/jbnhx42b): Top 6 teams present their projects to distinguished investors and Web3 industry veterans for final judging

_All workshop days are open to all students, regardless of competition participation_

🏗️ [**BUIDL Hackathon**](https://www.ethdenver.com/buidl/buidlathon)

📍 National Western Complex \| 🕒 Feb 23 - Mar 1 \| 🔗 Open registration

- **For:** Developers and creators from 125+ countries
- **Key Takeaway:** ETHDenver's flagship hackathon with $1.5M+ historical prize pool

☕ [**BuildOnEigen Breakfast**](https://lu.ma/ytc9jxfx)

📍 Register to See Address \| 🕒 Feb 27, 8AM - 10AM \| 🔗 Approval required

- **For:** Active EigenLayer builders and developers
- **Key Takeaway:** Private networking breakfast with core team members


## See you in Denver!

We can't wait to connect with builders, researchers, and innovators from around the world at ETHDenver 2025. Whether you're joining us at The Vault, dropping by our booth, or catching one of our many talks, the EigenLayer team looks forward to exploring infinite possibilities with you in Denver!

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/#/portal)

Subscribe

Twitter Widget IframeThe EigenLayer and EigenDA Holesky Testnet is now up and running. This marks an important milestone for the EigenLayer ecosystem, and we're keen for operators, stakers and rollup developers to continue testing on Holesky as we gear up for the upcoming mainnet launch. As a reminder: points are not available on the Holesky testnet.

- **Restakers:** Find your way to the EigenDA Holesky testnet [here](https://holesky.eigenlayer.xyz/restake).
- **Operators:** Follow the step-by-step guide for onboarding through the [CLI instructions](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-installation).
- **Rollup Sequencers & Full Nodes:** Post, retrieve and verify blobs. Rollup clients should configure their sequencers and full nodes to disperse and retrieve blobs from the EigenDA Holesky disperser endpoint: `disperser-holesky.eigenda.xyz:443`
  - [Documentation](https://docs.eigenlayer.xyz/eigenda/networks/holesky#specs)

**Dual Quorum Comes to EigenDA Testnet**

We're thrilled to introduce an exciting new feature **“Dual Quorum”** on the EigenDA Holesky testnet. Dual Quorum introduces an extra layer of robustness and decentralization to EigenDA by using two quorums to secure the same proof of stake network.

You can read more about the benefits of Dual Quorum in this [blog post](https://www.blog.eigenlayer.xyz/dual-staking/). At its core, Dual Quorum strengthens EigenDA’s cryptoeconomic security model through redundant storage of blobs across different EigenDA operator sets.

More generally, it allows operators to support any validation services through both LSTs/native ETH and an additional token. AVSs can design staking preferences and validation models using multiple tokens, which opens up new fundamental design possibilities for proof of stake networks.

This feature adds depth to AVS consensus mechanisms and brings further utility to native tokens for AVSs and rollups. While wETH is currently enabled on Holesky for testing of Dual Quorum on EigenDA, we're excited about the potential to welcome a variety of tokens to be used as staking assets in the EigenLayer ecosystem.

**The Unique Benefits of Dual Quorum.** EigenDA's Dual Quorum feature set addresses the bootstraping challenge by easing the initial process of securing a new Proof of Stake (PoS) network. By allowing staking in ETH as well as staking in various protocol tokens, the network can tap into the security of ETH while bootstrapping the new token and network.

**Mitigating volatility risks.** The inclusion of a more stable and non-correlated asset like ETH cushions the network against the inherent price fluctuations of new tokens. This layer of economic security ensures that even in times of native token volatility, the network's integrity remains uncompromised.

**Expanding security and decentralization**. Dual Quorum enables a richer, more resilient security model. By validating through both native token (AVS or Roll up tokens) and ETH-backed operators, it introduces a dual-layered consensus mechanism, enhancing the network's resistance to attacks and faults. AVSs have flexibility to design the Dual Quorum consensus mechanism in various ways (as further described [here](https://www.blog.eigenlayer.xyz/dual-staking/)) that best suit the individual AVS.

Thank you for your commitment and for joining us in this journey. We can't wait to see the Holesky Testnet in action with your support and transition smoothly into the exciting new era of the EigenLayer mainnet.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/#/portal)

SubscribeIn collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!

**EigenLayer's Vision:** Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives in an uncapped and permissionless state. But to reach this future, we need community participation and the right assets to back our goal.

For this contest, all active Ethereum LST tokens backed by more than 15,000 ETH are eligible for submission and inclusion in the voting process, and all their token holders have the right to cast votes. Any LST that receives votes exceeding 15,000 ETH will be integrated into EigenLayer in the upcoming months. The incorporation will start with the LST that attracts the most votes, representing the LST community's commitment to restake, and will continue in order based on the number of votes from highest to lowest. Additional contests will be run in the future, inviting new LSTs to participate.

Below are the LSTs we have identified as satisfying the 15,000 ETH criteria. Did we miss one? Help us list it! You can recommend additions on our [Discord](https://discord.com/invite/eigenlayer) in the #lst-addition-jokerace channel before the contest's official start on **October 20,** when a snapshot will be taken for all eligible LST wallets to participate in the vote.

The initial LST candidate list:

1. [wBETH](https://etherscan.io/token/0xa2e3356610840701bdf5611a53974510ae27e2e1) (Binance)
2. [sfrxETH](https://etherscan.io/token/0xac3e018457b222d93114458476f3e3416abbe38f) (Frax)
3. [SETH2](https://etherscan.io/token/0xFe2e637202056d30016725477c5da089Ab0A043A) (Stakewise) _(if Stakewise gets enough votes, their new LST osETH will be added when live)_
4. [swETH](https://etherscan.io/token/0xf951E335afb289353dc249e82926178EaC7DEd78) (Swell)
5. [AnkrETH](https://etherscan.io/token/0xE95A203B1a91a908F9B9CE46459d101078c2c3cb) (Ankr)
6. [LsETH](https://etherscan.io/token/0x8c1BEd5b9a0928467c9B1341Da1D7BD5e10b6549#balances) (Liquid Collective)
7. [mevETH](https://etherscan.io/token/0x24Ae2dA0f361AA4BE46b48EB19C91e02c5e4f27E) (mev.io)
8. [EthX](https://etherscan.io/token/0xA35b1B31Ce002FBF2058D22F30f95D405200A15b) (Stader)
9. [oETH](https://etherscan.io/token/0x856c4Efb76C1D1AE02e20CEB03A2A6a08b0b8dC3) (Origin ETH)

**Pre-commitments & Ranking Logic**

Tokens are not only welcomed but encouraged to pre-commit restaking volume if they’re vying for being added. The tokens will be ranked based on the ETH volume their communities commit to restake.

**Counting Votes**

During the submission phase, LSTs can present information and rally for votes. All addresses from the snapshot can cast their votes on the Jokerace contest. We will run an X (Twitter) poll to determine the chain that will host the contest. Please help choose where the contest will be held [on this poll](https://twitter.com/eigenlayer/status/1714331527025476017) before October 20.

**Winner Selection**

We’re optimistic about the LST landscape's evolution, aiming to facilitate restaking for every LST in the long haul. However, in the immediate future, the integration will be sequential. Post contest, LSTs that receive over 15,000 ETH in voted interest will be added to EigenLayer. The journey will commence with the leading LST and progress sequentially.

**Submission & Voting Procedures**

- _Snapshot Date_: A snapshot will be taken on Oct 20th to identify eligible Ethereum LST wallet holders.
- _Eligibility_: Wallets holding any of the listed Ethereum LSTs or EigenLayer restakers by Oct 20th are qualified to vote. Each wallet gets 1 vote for every $USD worth of tokens staked.
- _Campaigning_: LST teams, over the subsequent week, will spotlight their tokens, disseminate pertinent information, drive marketing, and pronounce their restaking pledges.

**Voting Insights**

- _Duration_: One week.
- _Focus Areas_:
  - Restaking Capability
  - Total Value Locked (TVL)
  - Reputation
  - Operational Stability

**Key Dates**:

- Interested in adding an LST to the allowlist with more than 15k ETH TVL? Please provide your justification on Discord by October 20th, 9 am Pacific Time.
- _Submissions (for LSTs to campaign):_ Oct 20-25
- _Voting:_ Oct 25-Nov 1

**Feedback or Queries?**

Jump into the EigenLayer Discord and step into the #lst-addition-jokerace channel to converse with team EigenLabs, JokeRace, and other LST team representatives. We're all ears!

We're excited to see the submissions and the community's enthusiasm. Let's continue to shape the future of restaking together.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/#/portal)

Subscribe_Tldr;_

Interested in early restaking opportunities? Please [fill out this Indication of Interest form](https://share.hsforms.com/1D7t0gHzoQn6K3A0EGvdWXQein6l) to get in touch with the EigenLabs team.

**Introduction:**

EigenLayer Mainnet is set to launch soon, following the successful completion of the [Stage 1 Testnet](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/) for restaking functionality. EigenLayer aspires to make a positive impact on the industry by introducing its distinct restaking offering, which enables additional staking opportunities for ETH stakers who opt-in, and helps spur innovation by lowering capital costs for AVSs (actively validated services). In this blog post, we'll share details about the upcoming phased mainnet launch and the benefits of early restaking.

**Phased Mainnet Launch:**

The EigenLabs team has carefully planned a phased mainnet launch to ensure the highest levels of security and stability for the platform. The process will begin with the mainnet launch for restaking functionality, followed by the testnet for operators in the coming months. After the successful completion of the operator testnet, we will move on to the operator mainnet, followed by the testnet for AVSs, and finally, the AVS mainnet launch by the end of the year.

**Restake Interest and Benefits:**

As the EigenLayer Mainnet launch draws near, restaking your ETH earlier could be increasingly advantageous. By restaking early, you can ensure your place within the ecosystem and potentially maximize your staking opportunities over time.

Certain services may cap their restake amounts to ensure meaningful participation for their restakers. By restaking early, you'll be better positioned to benefit from these services as they become available on the EigenLayer platform.

**Why Restake Early?**

- Secure Your Spot: Early restaking establishes your place in the EigenLayer ecosystem, providing you with an advantage over those who wait to restake.

- Skip The Withdrawal Queue: Native [restakers with a 0x00 prefix](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/) can skip the withdrawal queue and point their validators directly to the EigenLayer contracts.

- Maximize Staking Opportunities: Early restaking allows you to enjoy the earliest possible access to restaking on services that launch on EigenLayer, as some services may cap their initial restake amounts. EigenLabs is currently working with a number of partner projects to develop and launch AVSs, across verticals including _hyperscale data availability_, _fast settlement/finality, decentralized sequencing,_ and _proof of decentralization_. While most collaborations are still private, public statements have been made by [EigenDA](https://twitter.com/sreeramkannan/status/1613369534819504130), [Hyperlane](https://twitter.com/Hyperlane_xyz/status/1628509531738198017), [Omni](https://twitter.com/OmniFDN/status/1650867761126539266), [Lagrange](https://forum.eigenlayer.xyz/t/lagrange-cross-chain-state-committees/628), [Espresso Systems](https://twitter.com/EspressoSys/status/1636463910248394752), and [Witness Chain](http://lagrange/).

- Access to Premium Services: Early restakers will be in a prime position to access and benefit from exclusive EigenLayer community experiences, as well as curated working groups with EigenLabs and other restakers, operators, and modules.


**Conclusion:**

The upcoming launch of EigenLayer Mainnet marks a significant milestone for the restaking collective. As the phased mainnet launch is fast approaching, it's an ideal time to prepare for restaking your ETH or LST, positioning yourself to benefit from early participation once the mainnet goes live.

If interested in early staking opportunities, please [fill out this Indication of Interest form](https://share.hsforms.com/1D7t0gHzoQn6K3A0EGvdWXQein6l) to get in touch with the EigenLabs team.

_Stay up to date with the latest on EigenLayer:_

- Visit the website: [eigenlayer.xyz](https://www.eigenlayer.xyz/)
- Check out the testnet: [goerli.eigenlayer.xyz](https://goerli.eigenlayer.xyz/)
- Read through the documentation: [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz/)
- Meet the community in Discord: [discord.gg/eigenlayer](https://docs.eigenlayer.xyz/overview/readmediscord.gg/eigenlayer)
- Contribute research ideas to the forum: [forum.eigenlayer.xyz](https://forum.eigenlayer.xyz/)
- Follow @eigenlayer on Twitter: [twitter.com/eigenlayer](https://twitter.com/eigenlayer)


## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/#/portal)

SubscribeWe're thrilled to announce the next chapter in our journey towards a secure and successful EigenLayer mainnet launch! Our primary focus remains a smooth launch that prioritizes both security and performance. To achieve this, we're introducing a multi-phased approach starting now and ongoing for the next several weeks.

This process is designed to support a seamless transition to a thriving EigenLayer mainnet.

**Why a Gradual Mainnet Rollout?**

Safety and performance are paramount. By meticulously introducing functionalities in measured steps, we can:

**Minimize Risk:** A gradual rollout allows for rigorous testing and monitoring at each step, minimizing potential risks associated with a full-scale launch.

**Optimize Performance:** The phased approach enables us to measure and optimize network performance as more users and operators join the ecosystem. This ensures a stable and scalable network for everyone.

**The Road to Mainnet**

Here’s a breakdown of the exciting phases that will empower key players in the EigenLayer ecosystem. Expect more updates with the timing of each phase.

**1: Final Testnet**

Before deploying to mainnet, we are testing all changes on a testnet first. Since the start of the Stage 2 testnet on Goerli, over 6K operators have participated in successful tests through multiple upgraded versions. As the Goerli chain is no longer actively supported by the Ethereum client teams, we are moving the final EigenLayer testnet version to Holesky, which will be operational soon.

**2\. Webapp & Contract Pause:** Initially, the EigenLayer webapp and all contracts on the mainnet will be paused to facilitate a controlled deployment and onboarding of operators onto the network. This temporary halt in deposits and withdrawls will help ensure a smooth transition.

**3\. Gradual Operator Registration:** Operators will have the opportunity to register gradually on the mainnet. This measured approach allows for performance testing and controlled capacity growth, ensuring a stable network.

**4\. EigenDA Launch:** Witness the power of open innovation with the launch of EigenDA, the first AVS to launch on top of EigenLayer's infrastructure. EigenDA enables hyperscale data availability for rollups, boasting a target write throughput of 10 MB/s at launch. Operators will start to opt in to EigenDA at this stage.

**5\. Mainnet Launch Completed:** The EigenLayer webapp will be updated to include a new staker dashboard and discovery page for operators. With the completion of this stage, restaker delegations, deposit, and withdrawal functionality will be re-enabled.

**6\. The Emergence of the AVS Ecosystem:** Following the launch of EigenDA, a vibrant ecosystem of AVSs is poised to flourish on the EigenLayer mainnet. Stay tuned for exciting announcements as more AVS teams bring their innovative solutions to mainnet.

**What This Means for You:**

This is a significant milestone for the entire EigenLayer ecosystem! We're building a robust and dynamic network for open innovation together.

While the initial focus of Stage 2 is on operators and EigenDA, these phases lay the foundation for expanded future participation. Stay tuned for exciting developments in upcoming stages, including comprehensive developer tools and resources that will unlock the potential for a diverse array of AVSs. Get ready to build and deploy your AVS seamlessly!

**Join us in Building Infinite Sum Games**

Stay tuned for further details about the launch process and a dedicated channel for questions and feedback.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/#/portal)

Subscribe_Update: The EigenLayer Research Fellowship (ERF) structure has been updated on October 25th, 2023, particularly in selection criteria and program dates._

We believe EigenLayer is about to kickstart a new era of secure innovative infrastructure applications that will transform how blockchain applications operate. We’ve been diving deep into [these ideas](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/) for months, but our creativity can only take us so far.

We want to support you in your fresh perspectives and ideas for [Programmable Trust](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/).

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/ERFwhite-1.png)

## What is this program?

The EigenLayer Research Fellowship (ERF) is a playground for building groundbreaking infrastructure concepts. A platform that allows you to explore any ideas as broadly or deeply as you wish. It’s an opportunity to dream big, execute your designs, and have a blast collaborating with the EigenLayer team.

**For season 1, our research team has the bandwidth to work with a maximum of 5 fellows.** The program is **~2.5 months from December 11th, 2023 - Feb 16th, 2024**.

* * *

The ERF isn’t your typical fellowship; it’s:

**Comprehensive**: We’ll challenge you on every front, from crafting the biggest architectural designs to nailing the implementation and finalizing a presentation.

**Hands-on**: Expect the EigenLayer team to be supporting you throughout the journey. We’re here to be your personal sounding board and help you brainstorm.

**FAFO (freestyle around and find out)**: We’re all about embracing curiosity and encouraging exploration. So, go ahead, play around, and find out where your ideas take you.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/2023-10-18-20.35.32.png)And the best part? Upon completion, you get to showcase your project and become an official part of the EigenLayer protocol (more details soonTM).

Fellows can look forward to:

- Weekly group calls, individual meetings, and consistent research and engineering support from the core EigenLayer Research Team.
- Monthly demos.
- Amazing Swags: Eigen Nike Air Force 1s (or equivalent), sweater, tshirt, and hats
- Round trip flight and stay covered in Istanbul, Devconnect
- In-person meetups & coworking opportunities in NYC and Seattle

> 👀💙🤍 [@eigenlayer](https://twitter.com/eigenlayer?ref_src=twsrc%5Etfw) [@eigen\_intern](https://twitter.com/eigen_intern?ref_src=twsrc%5Etfw) [pic.twitter.com/JXR9iEytPq](https://t.co/JXR9iEytPq)
>
> — tina (@html\_tina) [July 20, 2023](https://twitter.com/html_tina/status/1682019049520209920?ref_src=twsrc%5Etfw)

## Who is the ideal candidate?

We’re on the lookout for candidates who have the outlined skills below. Don’t worry if you’re not an expert in every category; we’re all about growth, as long as you're willing to put in the work:

**Self-motivation and Self-direction**: While we’ve got your back, the responsibility for developing and implementing your ideas lies with you. Navigate this complex idea landscape, stay motivated, and conquer whatever challenges come your way.

**Strong Tech Foundation**: Designing on EigenLayer is a highly technical task. You don’t need a Ph.D., but you should be ready to learn new concepts across a broad tech stack continuously. Foundational knowledge that enables you to dive deep into a specific domain is a must.

**Comfortable with Public-facing Work**: Your work will be visible in the public domain. So embrace it, share your ideas, remember to accept criticism with humility, and maintain a humble attitude while learning from others.

**Coding Ability**: No need to be a coding expert, but you should be able to use code as a tool to communicate with computers and achieve your objectives.

## Selection Criteria

We place great importance on selecting our fellows. Although we don’t have a fixed selection criteria, we consider the following areas:

- Your application and passions.
- Any previous crypto app or systems design experience you bring.
- Interest in EigenLayer and love of open innovation.
- Your authentic self. Be you!

This is a rolling application, however, we will prioritize candidates who fill in the application via [bit.ly/eigenlayerrf](https://bit.ly/eigenlayerrf) before Wednesday, November 1st, 2023.

Two rounds of interviews will follow this before the final selection is announced.

## Structure of the Fellowship

**Phase 0: Selection \| October - November 2023**

We will conduct interviews to identify areas of interest and potential project scope. Candidates that pass the interviews will be invited to our Devconnect Hacker House, and get the chance to meet each other as well as the EigenLayer Research Team. **_This is optional, however, their stay will be covered alongside a travel stipend if they decide to come._**

**Phase 1: Ideation \| December 2023**

Fellows will brainstorm **; finally** exciting project ideas with the EigenLayer core team after Devconnectand begin the design process.

**Phase 2: Build \| December - January 2024**

Design, build, redesign, and build again. The core team will have your back with hands-on engineering support.

**Phase 3: Present \| February 2024**

Prepare for a project presentation; finally, the project is revealed amongst a wider group of peers. We may offer further support direct grant funding to teams or individuals as appropriate going forward.

**Fellows’ Must-Dos**:

- Create an exceptional one or two-pager about your project.
- Commit about 10 hours per week for three months.
- Join us remotely via Telegram and Google Meet or meet in person during Devconnect in Istanbul, and cowork with us in NYC and Seattle if you are around.

**Ready for the adventure?** We will prioritize candidates who fill in the application via [bit.ly/eigenlayerrf](https://bit.ly/eigenlayerrf) before Friday, October 27th, 2023.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/image_2023-10-18_01-59-44.png)

**About EigenLayer:**

More information about EigenLayer is available at [https://www.blog.eigenlayer.xyz/tag/avs-research/](https://www.blog.eigenlayer.xyz/tag/avs-research/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/#/portal)

Subscribe

Twitter Widget Iframe## Overview of the Guarded Launch, Protocol Security, and Governance

EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:

- **StrategyManager:** 0x858646372CC42E1A627fcE94aa7A7033e7CF075A
- **EigenPodManager:** 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338

To restake on mainnet, visit the EigenLayer app at: [app.eigenlayer.xyz](https://app.eigenlayer.xyz/)

For this Stage 1 launch, the functionality aligns with what has been accessible on the Goerli testnet since April 6, 2023. Ethereum stakers can participate in EigenLayer by securely managing their liquid staking tokens and self-directing their funds. This can be done by utilizing the EigenLayer contracts or by creating an EigenPod and configuring the Beacon chain withdrawal credentials to their EigenPod addresses.

As outlined in the [Stage 1 Testnet announcement](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/), the mainnet launch of EigenLayer marks an important milestone in enabling restaking. EigenLayer's next phase will introduce Operators, who will be responsible for performing validation tasks for AVSs (Actively Validated Services) built on the EigenLayer protocol.

In the third phase, the protocol will facilitate the launch of multiple AVSs, leveraging the robust foundation provided by EigenLayer.

For a preview of some potential AVSs building on EigenLayer, see these public discussions: [EigenDA](https://www.youtube.com/watch?v=B2qBnLmPupQ&themeRefresh=1), [Espresso](https://www.espressosys.com/), [Witness Chain](https://witnesschain.com/), [Omni](https://omni.network/), and [Lagrange](https://www.lagrange.dev/). There are several other AVSs currently in different stages of research and development. More details about these projects will be shared in the near future.

Once Restakers, Operators, and AVSs are launched on EigenLayer, the ecosystem will function as follows: Restakers have the flexibility to delegate their staked ETH to Operators or run validation services themselves, effectively becoming an Operator. The delegation process involves a double opt-in between both parties, ensuring mutual agreement. Restakers retain agency over their stake and choose which AVSs they opt-in to validate for.

Operators and AVSs operate in a symbiotic relationship, with both parties needing to opt-in to serve each other. Operators perform validation tasks for AVSs, contributing to the security and integrity of the network. AVSs, in turn, deliver innovative use cases to users within the Ethereum and wider crypto ecosystem, leveraging the power of Ethereum's shared security.

To learn more about EigenLayer, read the [user guides and documentation](https://docs.eigenlayer.xyz/guides/mainnet-restaking).

**Guarded Launch**

In the interest of caution with regards to protocol security, EigenLayer is launching with restaking limits. These limits will be raised progressively, with the goal of reaching an open and uncapped state where any user can restake any amount of staked Ether.

At this time, the protocol supports liquid staking of Lido stETH ( [stETH](https://etherscan.io/token/0xae7ab96520de3a18e5e111b5eaab095312d7fe84)), Rocket Pool ETH ( [rETH](https://etherscan.io/token/0xae78736cd615f374d3085123a210448e74fc6393)), and Coinbase Wrapped Staked ETH ( [cbETH](https://etherscan.io/token/0xBe9895146f7AF43049ca1c1AE358B0541Ea49704)). The protocol will support _many_ more liquid staking tokens in the future, and is already working with several staking protocols on launch plans. If you would like to introduce your LST to the EigenLayer community, please do so on the [forum](https://forum.eigenlayer.xyz/t/about-the-new-lst-token-on-eigenlayer-category/6641).

In addition, the protocol facilitates native restaking, allowing users to create EigenPods and assign the withdrawal credentials of multiple validators to the addresses of their EigenPods.

The following restaking limits and conditions are in place:

- Within Liquid restaking, there is a limit of 3,200 tokens that can be deposited for each supported asset. That is, only a maximum of 3,200 stETH; 3,200 rETH; and 3,200 cbETH may be restaked.
- Within Liquid restaking, there is a deposit limit of 32 tokens per user transaction.
- The creation of additional EigenPods will be paused once native restaking hits approximately 9,600 ETH. No restrictions have been placed on the number of validators that a user may link to a givenEigenPod.
- There will be a 7-day withdrawal delay that will serve as a security measure during the early stages of the EigenLayer mainnet, to optimize for the safety of assets. This withdrawal lag, which is common in staking protocols, is required when AVSs go live, as there is a lag to verify that activity associated with any AVS was completed successfully.

Read the [user guide to liquid restaking here;](https://docs.eigenlayer.xyz/guides/liquid-restaking) and the [user guide to native restaking here](https://docs.eigenlayer.xyz/guides/native-restaking).

## Protocol Security

EigenLayer has undergone a series of third-party security analyses, each of which is listed below. Please click through the links to see each security report. All security reports are also available in the [Security section](https://docs.eigenlayer.xyz/overview/audits) of the EigenLayer documentation:

- Security Audits: [ConsenSys Diligence](https://consensys.net/diligence/audits/2023/03/eigenlabs-eigenlayer/), [Sigma Prime](https://github.com/Layr-Labs/eigenlayer-contracts/blob/master/audits/Sigma_Prime_Layr_Labs_Eigen_Layer_2_Security_Assessment_v1.pdf)
- Audit Contest: [Code4rena](https://code4rena.com/contests/2023-04-eigenlayer-contest)

In addition, there are $2,000,000 of bug bounties available via [Immunefi](https://immunefi.com/bounty/eigenlayer/).

The protocol’s top priority is the safety of restaked assets, and it will take measures to reinforce security on an ongoing basis.

**Decentralization**

EigenLayer is committed to decentralizing the protocol and engaging its community members to empower participation within a fully trustless restaking protocol. As the protocol matures, the goal is to increasingly establish immutable parameters and rules that foster a decentralized network of participants that sustainably contribute to the protocol.

EigenLayer is establishing a pathway towards this goal in collaboration with all the stakeholders in the EigenLayer ecosystem. The pathway involves an iterative approach designed to support permissionless growth and development of restakers, operators, developers, and the Ethereum ecosystem as a whole.

This effort will require active participation from members of the community, like you. EigenLayer eagerly looks forward to your engagement in an inclusive and transparent decision-making culture that strengthens the protocol, governance model, and future projects.

Together, we are paving the way for a truly decentralized and thriving ecosystem.

## Initial State

EigenLayer is designed with upgradeable smart contracts, the ability to pause functionality, and various adjustable parameters. The ability and responsibility to make decisions regarding contract upgrades, pausing functionality, and adjusting parameters initially have been delegated to three main governance multisigs.

**The Operations Multisig**

The Operations Multisig is a 3-of-5 and can execute routine upgrades and maintenance through a timelock that enforces a minimum 10-day delay on all safety-critical actions. It can also pause EigenLayer functionality in emergency situations.

**The Pauser Multisig**

The Pauser Multisig is a 1-of-9 multisig that can also pause EigenLayer functionality in emergency situations, but holds no other powers.

**The Community Multisig**

The Community Multisig is a 9-of-13 multisig composed of members of the Ethereum community. In normal circumstances, the signers of the Community Multisig will simply act as observers, receiving regular updates on the Operations Multisig’s transactions, including notifications of the Operations Multisig queuing new time-locked actions. In extraordinary circumstances, the Community Multisig can perform emergency actions, including immediately executing time-critical upgrades or replacing the Operations Multisig in the event of private key compromise.

The contract address for the Community Multisig is 0xFEA47018D632A77bA579846c840d5706705Dc598 and its members include:

- Tim Beiko - Ethereum Foundation
- Viktor Bunin - Credibly Neutral
- Uma Roy - Succinct
- Brian Retford - RISC Zero
- Pramod Viswanath - Witness Chain
- Swapnil Raj - Nethermind
- Dimitry Ukhanov - P2P
- Tarun Chitra - Robot Ventures
- Anna Rose - ZK Validator
- Curtis Spencer - Electric Capital
- Yuan Han Li - Blockchain Capital
- Ben Rodriguez - Coinbase Cloud
- Rob Pellecchia - Figment Capital

Finally, there is an _Executor Multisig_ which only has the (automated) role of executing functionality passed to it by either the Operations or Community Multisig.

On a technical level, the governance architecture looks like the following:

![](https://lh5.googleusercontent.com/tdLYguBq5wyfQbJRkyVo7pqT1tBasCLxXP-aA60GZGXlqKDkLtQIN9guogHXdSRObApuLHT3LpPfIxZWJruxaAJBH5skfRY3EQAPya0sxxUnj1EoDgkUCxItwETv-dpaVVAV86JCzpYduZcpLQlH9-0)

These multisigs represent a rudimentary but working system of transparent initial governance, with appropriate checks and balances. As the protocol continues to evolve, so will its governance, and we are continuously evaluating our options for potential future governance mechanics.

Decentralized governance practices are rapidly evolving, continuously pushing boundaries and exploring innovative approaches. In addition to the above, EigenLayer anticipates the emergence of novel mechanisms that have yet to be designed and is excited to research them and facilitate implementation.

## Summary

The launch of EigenLayer on Ethereum mainnet marks a significant milestone in its mission to create a robust environment for open innovation. With the support of our community, we will progressively decentralize the protocol and establish a robust network of participants. By fostering inclusion, transparency, and continuous improvement, EigenLayer aims to empower the ecosystem stakeholders and innovate on both the future of shared security, as well as the future of decentralization.

Join this exciting journey to unlock the full potential of EigenLayer, and push forward the mission to maximize open innovation in crypto.

## Join the EigenLayer Community

- Check out the restaking app: [app.eigenlayer.xyz](https://app.eigenlayer.xyz/)
- Visit the website: [eigenlayer.xyz](https://www.eigenlayer.xyz/)
- Read through the documentation: [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz/)
- Meet the community in Discord: [discord.gg/eigenlayer](https://discord.com/invite/eigenlayer)
- Contribute research ideas to the forum: [forum.eigenlayer.xyz](https://forum.eigenlayer.xyz/)
- Follow @eigenlayer on Twitter: [twitter.com/eigenlayer](https://twitter.com/eigenlayer)
- Join the EigenLabs team: [https://www.eigenlayer.xyz/about#careers](https://www.eigenlayer.xyz/about#careers)


## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/#/portal)

SubscribeEigenLayer will expand [restaking opportunities](http://app.eigenlayer.xyz/) for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.

To modify the restaking limits, a protocol parameter change must undergo approval from the [Multisignature Governance system](https://docs.eigenlayer.xyz/overview/readme/multisig-governance). As of today, June 30, the Operations [multisig transaction](https://app.safe.global/transactions/tx?id=multisig_0xBE1685C81aA44FF9FB319dD389addd9374383e90_0x8b224fff9df230380e04b2794286c5030a3d526621375675b0694dc6d7567aa9&safe=eth:0xBE1685C81aA44FF9FB319dD389addd9374383e90) to raise the caps has been queued in the 10-day timelock, ensuring adherence to governance processes and security measures.

After the 10-day timelock expires, the Operations Multisig, which is responsible for overseeing the protocol’s routine operations and procedures, needs to trigger the action from the timelock contract. This will initiate the parameter changes in the Executor Multisig and execute the modifications. As a result, after a minimum of ten days, the Operations multisig may lift the caps at any time. Closer to the exact time, we will provide updates on Twitter, this blog post, and Discord. We expect the caps to be raised sometime during the week of July 10.

At that time, the caps for LSTs, including rETH, stETH, and cbETH, will be increased to 15,000 tokens per LST with no individual deposit limit. Once the sum total of all LST deposits reaches 30,000 tokens, a global pause on LST restaking will be initiated.

This significant increase in restaking capacity empowers more users to actively participate in future restaking opportunities. Additionally, native restaking remains uncapped, allowing users to create EigenPods and link them to an unlimited number of validators.

Our commitment to network security and decentralization drives the measured increase of LST caps and the unrestricted [native restaking](https://docs.eigenlayer.xyz/guides/mainnet-restaking/native-restaking) capability. We strive to establish a resilient ecosystem for users to confidently restake their assets.

**What's on the Horizon?**

With the upcoming LST cap increase, let's explore what's next on the EigenLayer roadmap. Our focus remains on enhancing the restaking experience, while ensuring top-tier security and decentralization.

Q3 Operator Testnet: This phase will allow operators to familiarize themselves with EigenLayer infrastructure and prepare for seamless service of AVS validation tasks. It serves as a crucial step towards the operator mainnet launch, scheduled for the end of Q4.

Q4 AVS Testnet: EigenLabs is currently in active development of [EigenDA](https://www.youtube.com/watch?v=B2qBnLmPupQ), the first AVS scheduled for launch on the EigenLayer platform. We are also collaborating closely with a variety of rollup teams to leverage this offering. Furthermore, our team is engaged in partnerships with multiple projects to develop and introduce a wide range of actively validated services (AVSs) on EigenLayer. During the AVS testnet phase, our partners have the opportunity to refine their offerings, ensuring the highest standards of security and performance. The anticipated AVS mainnet launch is currently scheduled for Q1'24, providing users with a diverse selection of AVS options to enhance their restaking experience.

Continuous Security Measures: EigenLayer prioritizes the security of users' assets. Rigorous third-party [security audits](https://docs.eigenlayer.xyz/overview/audits), including collaborations with ConsenSys Diligence and Sigma Prime, and participation in the Code4rena Audit Contest, reinforce our commitment to a secure ecosystem. Our bug bounty program with [Immunefi](https://immunefi.com/bounty/eigenlayer/) encourages community involvement in identifying and resolving potential vulnerabilities.

As EigenLayer progresses on its roadmap, we continue to provide increased restaking opportunities, foster decentralization, and safeguard assets within our ecosystem. Stay updated by visiting our website, exploring the app, reviewing documentation, joining our Discord community, contributing to the forum, and following us on Twitter. Together, we shape the future of restaking.

Website: [eigenlayer.xyz](http://eigenlayer.xyz/)

App: [app.eigenlayer.xyz](http://app.eigenlayer.xyz/)

Documentation: [docs.eigenlayer.xyz](http://docs.eigenlayer.xyz/)

Discord Community: [discord.gg/eigenlayer](http://discord.gg/eigenlayer)

Forum: [forum.eigenlayer.xyz](http://forum.eigenlayer.xyz/)

Twitter: [twitter.com/eigenlayer](http://twitter.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/#/portal)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/2023/10/Eigen-Universe-4.png)_If you’re excited about these primitives, join our private research discussion group here:_ [_https://bit.ly/programmabletrust_](https://bit.ly/programmabletrust)

EigenLayer empowers builders to develop innovative distributed systems without worrying about how to build the underlying trust networks for these systems. We call these distributed systems AVSs - actively validated services. We have categorized AVSs into 5 types:

1. **Rollup Services:** augmenting the Ethereum rollup ecosystem with services that inherit aspects of security from Ethereum’s trust network
2. **Applied Cryptography:** creating robust threshold cryptographic systems and TEE committees via decentralized nodes
3. **General Decentralized Networks:** bootstrapping networks easily from prover markets, and relayer markets to security monitoring counsels
4. **MEV Management:** allowing proposers to make additional credible commitments on block inclusion and ordering
5. **AI Inference:** ensuring program integrity and session privacy in a cost-effective manner

Our [previous blog post](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/) highlighted the three types of trust that any AVS can inherit from Ethereum via EigenLayer, namely economic trust, decentralization trust, and Ethereum inclusion trust, or some combination of the three. As a recap, with EigenLayer, you can **program these three types of trust**:

- **Economic Trust:** trust from validators making commitments and backing their promises with financial stakes
- **Decentralized Trust:** trust from having a decentralized network operated by independent and geographically isolated operators
- **Ethereum inclusion Trust:** trust that Ethereum validators will construct and include your blocks as promised, alongside the consensus software they are running

In this blog, we will walk through a high-level mental model of how to think about AVS system design. We will use this mental model to illustrate innovative AVSs one can build by mixing and matching types of programmable trust.

## Rollup Services

EigenLayer enables the development of foundational services that **scale Ethereum while inheriting security from Ethereum’s trust network**. This modular approach enhances security, and we collectively refer to these services as 'Rollup Services.'

This section will explore the following Rollup Services: decentralized sequencing, data availability, fast finality, keepers, watchers, and reorg-resistance.

#### 1\. Decentralized Sequencing

Currently, rollup sequencers **single-handedly** decide the order of transaction execution, potentially leading to **manipulation and short-term censorship**. While long-term censorship is handled in rollups using a mechanism for writing the transaction directly to Ethereum, these sequencer concerns can be mitigated by implementing a decentralized transaction ordering service.

In this service, users send their transactions to a network of decentralized nodes. There can be a variety of different decentralized sequencing services with different transaction ordering policies. We give some examples here:

- Approximate first-in-first-out ordering services (so-called fair ordering protocols)
- Multilateral ordering services with increased censorship-resistance
- Guaranteed MEV return to the rollup
- Shared / individual sequencing across rollups
- Threshold encrypted transaction ordering
- Automated event-driven activation.

For any of these decentralized sequencing services, one can inherit decentralized trust from EigenLayer, as well as reordering protection via economic trust inherent in EigenLayer.

#### 2\. Data Availability (DA)

To ensure the correctness of state execution in optimistic rollups and guarantee the liveness of zero-knowledge rollups and optimistic rollups, a critical requirement is the short-term Data Availability (DA) of underlying transaction blobs processed by the rollup.

The core concept involves rollups storing these blobs with a designated set of nodes committed to storing and serving them for a specified time frame, during which anyone can access the blob.

Consider situations where data-heavy consumer applications like gaming and social networking function within rollups. These apps usually have **low value per data bit but need a lot of bandwidth for state execution**. As a result, they demand **substantial** throughput, often tens of megabytes per second or more. Meeting this demand requires highly scalable data availability architectures that do not sacrifice security.

To satisfy this demand, the DA layer needs economic trust to tackle issues like the **lazy operator problem** via [Proof of Custody](https://dankradfeist.de/ethereum/2021/09/30/proofs-of-custody.html). It also **requires** decentralized trust to guarantee continuous operation.

**EigenLabs has been actively developing a solution called [**EigenDA**](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/) to address these challenges.**

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/eigenda.png)The figure illustrates the architecture of EigenDA and where EigenLayer nodes are used to store the designated data blobs.

#### 3\. Fast Finality

Rollups have faced several significant challenges, including the absence of instant and secure finality, Ethereum finality lag affecting cross-rollup bridging, costly cross-rollup interactions, liquidity fragmentation across rollups, and limitations for ZK verification.

One potential solution to address these issues is implementing a fast finality layer within EigenLayer. In this approach, any rollup can assert a state claim on the fast finality layer, stating that the execution of a specific block of transactions leads to a particular state commitment. In ‘fast mode,’ nodes within the fast finality layer validate the rollup’s claim and provide attestations of its validity.

**If a supermajority of nodes attests to its validity, rollup clients can achieve economic finality nearly instantaneously.** However, in ‘slow mode,’ the attestations from the fast finality layer are subjected to a challenge period, allowing anyone to raise a challenge if they suspect malicious behavior.

It's important to note that the fast finality layer requires high economic trust to ensure safety.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/FFL.png)The left figure illustrates the benefits of Fast Finality Layer and the right figure shows a variation of how an FFL could be built with EigenLayer nodes.

#### 4\. Keeper Network

Keeper networks prove invaluable for users looking to initiate specific actions based on predefined conditions. These networks deploy nodes to respond to 'if-this-then-that' demands.

There are two types of keeper networks. The first type is suitable for non-time-sensitive actions, such as raising challenges to optimistic rollups within a 7-day window (further discussed in the Watcher Network section below) or managing bridge relays (as expanded in the Relayer Market section below). In such cases, the primary requirement is economic trust, as it enables the penalization of nodes engaging in misconduct.

The second type demands swift and time-sensitive actions, such as preventing collateral liquidation, minting new NFTs, or executing token trades in response to specific on-chain behaviors. These demands can be met through Ethereum inclusion trust via EigenLayer, where validators commit to prioritize and fulfill such requests. This approach can potentially change the status quo and relieve users from the burden of paying high gas fees.

![](https://lh6.googleusercontent.com/vR37nPQswokvVQcr-kcL5d-lRKrDIyMtkbEXQ69XjseXF7q8zwf3hU9SzVeznC6-AlWCqVjXxeNL8QWXMcCXSwaHMkqvk3yv1gKnmBaYS5we2h4gPeYmNcKbUe6yUL38ThMrM1kaw5SeU7-aggv5hMM)The figure indicates the benefits of EDA with Keeper Networks from a user-centric view.

#### 5\. Watcher Network

For any optimistic rollup (ORU) to be deemed secure, a challenge must be initiated in a pessimistic scenario involving incorrect state execution. Therefore, every ORU client requires assurance that a vigilant group is actively monitoring for any erroneous executions and raising necessary challenges.

This assurance can be established via economic trust by appointing EigenLayer operators as watchers. These operators can be penalized through slashing if they either make unfounded malicious challenges or fail to raise a challenge when required.

#### 6\. Reorg Resistance

One of the paramount characteristics for any blockchain to be considered secure is its resistance to chain reorganizations (reorgs). When a blockchain can leverage the economic trust provided by Ethereum, it significantly bolsters its defenses against potential chain reorgs.

At a high level, a service can be developed to guarantee that nodes with substantial stake in Ethereum attest to the block header of the most recently finalized block of the chain. To achieve this, these nodes run the chain's light client to verify that the finalized block has not been double-signed and is built on the most recently finalized block.

The new confirmation rule for any client on the chain would be to check whether the block header has been finalized by the chain and enough stake from Eigenlayer has attested to the finalized block header.

Obtaining reorg resistance from Ethereum hinges on establishing economic trust through EigenLayer.

#### 7\. Inbound and Outbound Bridge

Cross-chain bridging to and from Ethereum involves a delicate balance between interoperability and security. Centralized bridges offer excellent interoperability but compromise security. Conversely, light client bridges provide high security but incur high gas costs to run the light client smart contract.

One can break this tradeoff between interoperability and security by having a group of opt-in nodes, who have put a significant amount of stake as collateral on Ethereum, attest to messages bridging in and out of Ethereum off-chain. At the same time, they can be slashed on-chain optimistically for wrong attestations. This can be achieved via economic trust.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/bridges.png)The left figure shows the benefits of an outbound bridge with a new State Committee backed by EigenLayer nodes with economic guarantees, in contrast to the Sync Committee. The right figure illustrates how a bridge can be built with EigenLayer nodes attesting to block headers with economic guarantees.

## Applied Cryptography

#### 8\. Threshold Cryptography

Threshold cryptography has been proposed to achieve applications such as commit-reveal for protection against targeted frontrunning, privacy, etc.

The core idea behind threshold cryptography is that, given an encrypted message, at least k out of n signers can efficiently decrypt the message. In contrast, anything less than k is unable to do so.

![](https://lh3.googleusercontent.com/78AKOYqMXI4m3kbYl4WetbNrgNl3rjLy-BddoaRElgqhw8-qvUHAjv-Wdn1UhkM0reCaFk--U6Q3Xuio6iWBg2ZGYMSxiRFqj9YbHxA3Pn-odjNvn0WTORdhlis0ZAr9xA6spOFHHFmwN8NWHuBql2E)The figure highlights one way of building threshold cryptography with EigenLayer nodes in a scenario for Shamir Secret Sharing.

The security of this primitive essentially requires k to be large and the set of these signers to consist of a large decentralized set that inhibits collusion and liveness attacks. This decentralized set of nodes can be inherited from EigenLayer.

#### 9\. Threshold-FHE

Threshold Fully Homomorphic Encryption (FHE) enables distributed computation on encrypted data, delivering a robust privacy guarantee. As threshold-FHE implementation necessitates decentralization, EigenLayer offers a reliable source of decentralized trust.

![](https://lh6.googleusercontent.com/JK5b4y6IqrQGXvTrH0laZRDfnPNmTK8dy9Qn9FpNH0ytb-8AgOijQooG7RnU9PeGDOxlnZR7RQHI3GsGNbEDPIz_t1P4m-oqBEKhCJnFRvgU4jumY9ETvyO3X-646WdWF-cFEE5F5uAAvKNlVNIKGOU)The figure touches on one way of using EigenLayer nodes for threshold FHE where the nodes perform functions on the data without knowing the entirety of the data.

In this method, sensitive data is encrypted using threshold encryption, with the secret key shares distributed among the decentralized network of EigenLayer operators. Subsequently, computations are performed on the encrypted data, preserving privacy and security.

#### 10\. Trusted Execution Environment (TEE) Committees

Protocols can establish robust security guarantees by harnessing not only Trusted Execution Environments (TEEs) but also by constructing a decentralized network of TEEs through EigenLayer, referred to as TEE Committees.

**The combination of TEE and committees strictly improves the reliability of any committee without TEE**: this is because a breach of the system requires both a majority of the committee to be colluding and furthermore that the security model of the TEE has been breached.

We note that it is possible to require multiple distinct TEE models like Intel SGX, ARM TrustZone, and Amazon Nitro in the TEE Committee and require at least one sign-off from each model so that breaking the system requires breaking all these distinct trust models as well as majority collusion.

The decentralization of the TEE committee can be absorbed from decentralized trust on EigenLayer and furthermore, the economic trust can be borrowed from EigenLayer in situations where slashing is possible.

The TEE committee also provides privacy in the normal mode so that the TEE is not attacked. This makes the TEE committee an attractive solution for a variety of problems where both program integrity and privacy are required.

## Decentralized Networks

#### 11\. Relay Networks

Many bridges currently depend on a centralized group of relayers. When a dApp developer selects a bridge to work with, their choices are limited to the options provided by that bridge. This limitation exists in part because setting up a bridge with a set of relays can be challenging. To enhance system resilience, bridges can utilize a decentralized network of relay operators on EigenLayer.

#### 12\. Prover Networks

In the future, we may witness the emergence of prover networks competing to generate zk proofs as quickly and cost-effectively as possible, through parallelization methods. A portion of this network can be initiated through EigenLayer nodes, leveraging the decentralization capabilities of the EigenLayer network to provide broader access to these provers and ensure the overall liveness of the network.

#### 13\. Risk and Transaction Simulation Networks

Banks employ sophisticated transaction risk analysis to safeguard against malicious transactions, a level of security currently lacking in blockchain systems. A Risk Module AVS would tackle this issue by engaging a subset of nodes to simulate transactions and perform comprehensive risk analysis. If a transaction raises a red flag, the module alerts the network.

DApps can subscribe to this risk module, and any transaction seeking interaction with these DApps must undergo analysis through the risk module. The transaction is only executed when a threshold of k out of n nodes within the module signs off on its correctness, as determined by the risk analysis.

## 14\. MEV Management

In the current MEV supply chain, validators can only offer a limited commitment – that they won’t double-sign conflicting block headers (equivocation). Services like MEV-Boost rely on this rigid commitment.

EigenLayer expands this landscape by enabling validators to make a more diverse range of commitments to their counterparts, whether they are builders or direct users. This expansion of possibilities within the MEV supply chain **opens the door to the development of various MEV mechanisms under the “multiple lanes” paradigm**, allowing validators to express their preferences more comprehensively:

- Multi-lane block proposal, incorporating MEV-Boost’s full block auction and partial block auction like [MEV Boost +/++](https://x.com/eigenlayer/status/1696643497753756002?s=20), empowers proposers to contribute to block composition, enhancing censorship resistance.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/MEV.png)The figure shows how MEV-Boost+ and partial block auction can be done when proposers opt into EigenLayer to commit not stealing the MEV from seeing the block content.

- Event-driven activation enables Ethereum validators to function as attributable keepers, agreeing to activate specific event-driven transactions.
- The purchase of blockspace futures facilitates the conversion of statistical arbitrage into atomic arbitrage.
- Threshold encryption offers protection against targeted frontrunning in sandwich attacks.

It’s important to note that the first three MEV mechanisms rely on Ethereum inclusion trust via EigenLayer, while the fourth one depends on decentralization trust from EigenLayer.

## 15\. AI Inference

With the advent of trained open-source large AI models, users inferencing these models on new queries is already becoming a hot topic. However, the current landscape has just a few centralized entities running AI inference engines.

There are several compelling reasons for running AI inferences onchain:

1. **Program Integrity**: While many services opt for centralized servers like AWS, employing zero-knowledge (ZK) techniques for Machine Learning (ML) can be cost-prohibitive. EigenLayer seeks to broaden the market for computational integrity in ML. If you seek **both cost-effectiveness and computation integrity** when running AI engines for inference, especially when relying on decentralized servers is considered too risky, EigenLayer offers a solution. By offering the ability to inherit economic trust, EigenLayer facilitates these operations.
2. **Session Privacy**: EigenLayer operators running AI engines should only be able to decrypt the complete set of consumer queries if a significant portion of operators collude. This underscores the importance of decentralization, which EigenLayer inherits from the Ethereum trust network.
3. **Federated Learning**: Enabling multiple actors to participate in training AI engines ensures the privacy of datasets. For federated training to succeed, decentralization of actors is essential, a feature EigenLayer provides.

## **Wrapping Up**

The above ideas around Rollup Services, Applied Cryptography, General Decentralized Networks, MEV Management, and AI Inference represent only a handful of the use cases that are emerging from the EigenLayer ecosystem. The true potential of the EigenLayer trust network is limited only by your imagination.

We are thrilled to introduce EigenLayer as a versatile toolkit for others to craft their own protocols and services via programming trust for decentralization, economics, and Ethereum inclusion.

While we have envisioned and outlined some of these possibilities, most innovative applications are yet to be conceived. If you’re excited about these primitives, join our private research discussion group here: [https://bit.ly/programmabletrust](https://bit.ly/programmabletrust)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/#/portal)

Subscribe### **What is happening?**

Last week, as the Ethereum Core Developer team rolled out their Pectra Upgrade, the Holesky test network began experiencing instability, which has impacted developers’ ability to test applications, rollups, and services deployed on the EigenLayer protocol. Holesky’s block times have slowed, RPC access and accuracy have been inconsistent, and the Beacon Chain has experienced long periods of non-finality. [The Ethereum Core Developer team identified that this finalization problem](https://blog.ethereum.org/2025/02/14/pectra-testnet-announcement) was attributed to a misconfiguration in major execution clients, including Besu, Nethermind, and Geth, leading to a chain split on Holesky. Developers have addressed the misconfiguration, but the network's performance has remained degraded, and the Ethereum Foundation announced it is not expected to return to finality for up to a month.

EigenLayer has been working closely with the Ethereum Core developers to understand and minimize the impact of Holesky instability and provide developers with reliable Testnet environments while the issues are resolved. EigenLayer’s protocol on Holesky Testnet has experienced degraded performance, and EigenPod functionality has been paused. Because of Holesky’s recent instability, EigenLayer and EigenDA have decided to launch on the Sepolia Network to take advantage of the stronger developer experience and stable environment it provides for dapps and rollups alike. EigenLayer will continue to support Holesky as it stabilizes for end-to-end protocol testing, and moving forward, we plan to maintain both Holesky and Sepolia Testnets to ensure redundancy and developer choice.

### **Why Launch EigenLayer on Sepolia?**

EigenLayer and EigenDA selected Sepolia because it is the go-to dapp development environment on Ethereum. This will provide AVSs a better environment for testing their experiences alongside their customers and integration partners. EigenLayer believes Sepolia will accelerate AVSs development and drive demand.

### **When can AVSs get started using EigenLayer on Sepolia?**

Our Sepolia testnet will be available the week of March 10, 2025. When it is live, we will communicate its readiness in all of our developer channels. Developers will be able to join the Sepolia Testnet at that time using docs.eigenlayer.xyz. Contract addresses will be shared in the [public metadata repository.](https://github.com/Layr-Labs/eigenlayer-contracts-zeus-metadata)

### **When can L2s get started using EigenDA on Sepolia?**

EigenLayer on Sepolia now enables rollups to test EigenDA without deploying on Holesky. We will deploy a public devnet on Sepolia with a managed operator set, which will be operated by our team, and start migrating rollups to this environment. EigenDA is targeting becoming available on Sepolia Testnet by late April.

### **Next Steps**

Look out for updates on when Sepolia is live on EigenLayer and EigenDA. Users can always reach out on [Forum](https://forum.eigenlayer.xyz/) or [Discord](https://discord.com/invite/eigenlayer) to ask any questions. We welcome community input in improving our user experience.

### **FAQs**

#### **How does the Holesky issue affect native ETH restaking testing?**

If you are an LRT, exchange, custodian, or wallet partner relying on Holesky to test native (beacon chain) ETH restaking, **please note that this functionality is currently unavailable on the Holesky Testnet.** The [Ethereum Foundation shared that we can expect Holesky to be back to finality later this month](https://notes.ethereum.org/@ethpandaops/path-forward-holesky). In the meantime, the Ethereum Foundation is mirroring Holesky onto a new “shadow” Testnet, where EigenLayer will be available. The state will mirror Holesky’s, and end-to-end testing will be possible. EigenLayer will continue to communicate to its users around this resolution.

#### **Will EigenLayer continue to run on Holesky?**

Yes, EigenLayer will continue to support Holesky Testnet as it stabilizes for end-to-end protocol testing. Holesky is the best test network for developers building atop native restaking, EigenPods, validator infrastructure, and those moving toward production. EigenLayer will continue to work with the Ethereum Core Developer team to ensure we are taking the proper steps for the continuity of Holesky.

#### **Will EigenDA continue to run Holesky?**

Yes, we will keep the current Holesky Testnet environment and monitor the network closely to maintain its liveness as much as possible. Holesky will continue to serve as the testing environment for permissionless operator sets and EigenDA releases.

#### **Will the marketplace UI support Sepolia deployments?**

The marketplace UI will initially not be available on Sepolia; however, its deployment will be considered and prioritized based on customer demand. If your team requires the marketplace UI on Sepolia, please reach out to us directly to express your needs.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/#/portal)

Subscribe**Blockchains are entering an era of convergence, characterized by the dissolution of boundaries between different network architectures.** _Cosmos started as a network of appchains_, with each chain independently establishing its validator set and trust protocol using native tokens. Ethereum, on the other hand, chose a shared security approach where _every layer built on top leverages the security of staked ETH._

Now, **both networks are converging** on the idea of having a shared security layer support a network of interoperable blockchains. Innovations like mesh security and opt-in shared security allow Cosmos chains to leverage the existing validator sets of other Cosmos L1s. Similarly, through EigenLayer, Ethereum extends the reach of its trust layer to secure protocols beyond its native ecosystem.

Ethos supercharges this convergence by enabling Cosmos blockchains to leverage the economic security of restaked ETH to bootstrap their trust layer.

## Ethos Architecture

The Ethos ecosystem comprises 3 layers:

1. **AVS contract:** An AVS deployed on EigenLayer that enables secure delegation of restaked ETH to Ethos Guardians.
2. **Guardian chain:** A Cosmos L1  that acts as an intermediary layer between Ethereum and Cosmos, by using the restaked ETH to co-validate or delegate to other Cosmos L1s.
3. **Ethos-powered chains** that leverage the Guardians to easily bootstrap a decentralized trust network and/or increase their economic security.

**The 3 components work together as a security coordination layer that powers Cosmos with restaked ETH security.**

![](https://www.blog.eigenlayer.xyz/content/images/2024/03/telegram-cloud-photo-size-4-6041704429582926154-y.jpg)Diagram of Ethos Architecture

## Bootstrapping a validator set from scratch

**For anyone building a PoS chain, setting up a validator network from scratch is a complex and challenging task.** Developers must engage with numerous stakeholders to persuade them to hold and stake the native token. **This coordination overhead stifles innovation** in the appchain ecosystem, requiring developers to possess expertise in building every layer of the stack instead of focusing on their chain's unique value proposition.

Ethos eradicates this complexity and lowers the barriers to innovation. Instead of spending hours with 100+ different validators, developers building a PoS chain can easily leverage the Ethos Guardians to bootstrap a validator network. This eliminates the need for laborious and extensive stakeholder management and empowers developers to concentrate on their core competencies and unique offerings, thereby accelerating open innovation.

## Tapping into an existing pool of security

The other challenging aspect of building a PoS trust layer is the high cost associated with bootstrapping the network. PoS chains pay for the security budget with native token, diluting the token value. For stakers, holding and staking a new token has several costs:

- **Opportunity Cost:** the rewards that stakers could have receive from staking the tokens somewhere else
- **Risk Exposure:** Stakers take on price volatility risk as their staked token is illiquid

Therefore, unless the native token has additional value accrual mechanisms, protocols need to set high token emission rates to convince people to hold and stake the token for an extended period of time.

Ethos solves this problem by allowing any chain to tap into an existing pool of security in the form of restaked ETH. Since the restakers already earn yield from native Ethereum staking, the capital opportunity cost of securing additional chains is almost zero. Further, the risk exposure is significantly lower since the staking token is ETH, the most liquid staking asset.

Because the marginal costs of securing additional chains are drastically reduced for the validators, their rewards can be positive with much lower emission rewards. This allows protocols to bootstrap their networks with 90% lower security costs.

## Seamless coordination between Ethereum and Cosmos

Ethos streamlines the process for any Cosmos L1 to leverage the pooled security of Ethereum by abstracting away the complexity of such an integration on both sides.

Within the Cosmos ecosystem, Ethos is implemented as a security module, enabling rapid deployment of Cosmos L1s without the need for extensive customization. Stake updates and messages are securely transmitted from the Guardian chain to the other Cosmos chains via the IBC protocol.

On the EigenLayer side, **chains DO NOT need to spend hundreds of hours in business development efforts to identify and negotiate with operators.** Ethos abstracts that away.

Moverover, instead of requiring developers to learn Solidity to deploy AVS contracts on EigenLayer or implement custom slashing architectures, Ethos handles all the specifics out-of-the-box, providing an all-in-one solution for Cosmos chains to seamlessly leverage the security of restaked ETH.

## Concluding Thoughts

The convergence of Ethereum and Cosmos ecosystems represents a paradigm shift in the blockchain landscape. Ethos acts as a layer of hybridization, enabling Cosmos chains to effortlessly leverage the economic security of restaked ETH. Ethos not only fosters a more interconnected and resilient blockchain landscape but also **empowers developers to focus on their core competencies, accelerating innovation and fueling the growth of a vibrant appchain ecosystem.**

As the boundaries between network architectures continue to dissolve, the collaboration between Ethos and EigenLayer paves the way for a truly decentralized and interoperable web of blockchains, ushering in a new era of convergence and shared security.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/ethos/#/portal)

SubscribeFhenix and EigenLayer Join Forces to Pioneer FHE Coprocessors, Revolutionizing Onchain Confidentiality on Ethereum

We are excited that FHE Coprocessor will be building on EigenLayer and to announce the development of FHE-based coprocessors in collaboration with [Fhenix](https://www.fhenix.io/).

FHE coprocessors are secured by Fhenix’s optimistic FHE rollup infrastructure and EigenLayer’s restaking mechanism.

**What are Coprocessors?**

A coprocessor serves as a companion processor designed to offload specific computational tasks from a host chain – whether it’s Ethereum, an L2 or an L3, to a designated processor living outside the scope of the host. Unlike [ZK coprocessors](https://www.risczero.com/blog/a-guide-to-zk-coprocessors-for-scalability), which are tailored for scaling computations off-chain, FHE coprocessors specialize in maintaining data confidentiality during computation, which opens up new applications that are not otherwise possible.

**Understanding the FHE Coprocessor Architecture**

FHE coprocessors are effectively stateless, lightweight [FHE Rollups](https://www.fhenix.io/fhe_rollups_whitepaper/) in disguise. By stateless, we mean that they get inputs directly from the host chain (or from a user calling the coprocessor directly), rather than storing a state in by themselves. This makes the coprocessor a more lightweight construction, that can remain idle until it is needed to perform specific tasks. Furthermore, being stateless, we can greatly optimize the performance of these computations by using leveled FHE schemes.

For efficiency reasons, FHE rollups rely on fraud proofs (as opposed to ZK proofs). These proofs are settled on the host chain directly, ensuring the host chain and coprocessor share security. A coprocessor can have robust security mechanisms by building an AVS which uses restaked ETH as security collateral.

![](https://lh7-us.googleusercontent.com/HbYP5YgCaniGxVMELVT9in006YB3GzxJph0kqFLZfyQJiPSp8mxez08G1IMsRbMxtqnUeDTHHb0z9W18InhGCD-Wrl8MQPcZi0ip1fsItztGJpuTnJcuLXjZwzI8ZKPUs-qIZ9k36x0ZDHxAA2MdQvE)

Interacting with an FHE coprocessor is simple: an application on the host chain invokes (through an on-chain relay contract and an off-chain relay node) the coprocessor, asking it to perform some specific computation over a set of encrypted inputs, and gets the result back, which it can continue to use on-chain. The full flow is illustrated above and includes the following steps:

1. An application contract on the host chain invokes an encrypted computation in the FHE Coprocessor.
2. Our relay contract queues the request.
3. A relay node listens to events in the relay contract, and bridges a call to a dedicated Fhenix rollup.
4. A (stateless) FHE rollup executes the computation over the encrypted inputs.
5. The threshold network decrypts the output.
6. EigenLayer operators verify the execution and send partial signatures to a relay node.
7. A relay node calls back the contract with the result and an aggregated signature.
8. The relay contract verifies the aggregated signature and passes the output to the calling contract, relying upon EigenLayer for cryptoeconomic security. Any honest node may still submit a fraud proof within the dispute window (e.g., 7 days), in which case a dispute process would commence should there be any disagreement.
9. The application contract can resume execution, utilizing the result.

**The Significance to the EigenLayer Ecosystem**

EigenLayer allows any [AVS](https://www.eigenlayer.xyz/ecosystem?category=AVS%2CRollup%2COperator) to inherit security from Ethereum restakers. In theory, FHE coprocessors could use EigenLayer to gain a cryptoeconomic guarantee on the correctness of the execution done by the FHE rollup. EigenLayer operators would attest to the validity of executions, but should any operators be malicious, their stake would be slashed.

This is how fraud proofs can be confirmed immediately, removing the need to wait until the end of the fraud proof period (typically 7 days). This means that FHE coprocessors can at last compute at a rate once considered impossible, marking a major milestone in the advancement of on-chain FHE.

It’s also representative of the serious technological impact that an AVS can have, and a profound use case of EigenLayer. This is why we are particularly excited for this collaboration and are investing significant resources.

**New Applications Horizon**

The application spectrum for FHE coprocessors extends across a diverse array of scenarios, particularly in domains where preserving confidentiality is paramount.

Take, for instance, the application of FHE in on-chain auctions, where bid confidentiality ensures fairness and a more economical outcome for all bidders. Similarly, with on-chain AI, FHE coprocessors can run intensive computations away from the host chain, all the while ensuring that the input data is kept private. The full list of use cases is extensive and includes decentralized identity, confidential DeFi, encrypted gaming, MEV protection, and much more.

This is why we consider this partnership to be of substantial significance and is particularly exciting from an AVS use case perspective.

**A Future Shaped by FHE coprocessors**

This collaboration marks a significant milestone for Ethereum.

By harnessing the power of FHE coprocessors, developers can build applications that utilize confidential data securely and efficiently, while maintaining security. This technological advancement opens up new possibilities for confidential blockchain applications and represents a significant AVS use case.

We look forward to our commitment to this endeavor and see confidential computation as a revolutionary area of blockchain.

**About EigenLayer**

EigenLayer is a protocol built on Ethereum that introduces restaking, a new primitive in cryptoeconomic security. Restaking enables staked ETH to be used as cryptoeconomic security for protocols other than Ethereum, in exchange for protocol fees and rewards.

For more information visit [www.eigenlayer.xyz/](https://www.eigenlayer.xyz/)

**About Fhenix**

Fhenix is the first FHE-powered L2 to bring on-chain confidentiality to Ethereum. Fully Homomorphic Encryption, or FHE, is a novel cryptographic scheme that enables direct computation over encrypted data without ever revealing the underlying data. Fhenix extends Ethereum’s capabilities with the goal of advancing application development and bringing true data confidentiality to smart contracts, transactions, and on-chain assets for the first time.

For more information visit [www.fhenix.io](http://fhenix.io/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/fhenix/#/portal)

SubscribeOne of the open challenges in the blockchain industry is achieving **trustless confidentiality**. The inherent transparent nature of public blockchains prevents the development of applications requiring on-chain confidentiality across gaming, decentralized finance (DeFi), governance, and identity without relying on a trusted third party.

Current approaches to providing privacy on the blockchain leverage technologies such as Zero Knowledge Proofs (ZKPs), Multi-Party Computation (MPC), and Trusted Execution Environments (TEEs), each presenting unique [advantages and limitations](https://variant.fund/articles/building-dark-mapping-crypto-privacy-landscape/).

As described in our [Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/) piece, recent development in fully homomorphic encryption (FHE) offers developers a novel paradigm for creating decentralized applications with unique confidentiality benefits.

## **FHE is the Holy Grail of Confidentiality**

Fully homomorphic encryption (FHE) operates using mathematical concepts from algebraic geometry, number theory, and lattice-based cryptography. The basic idea behind FHE is to use mathematical structures that allow **operations to be performed on encrypted data** (ciphertext) in a way that, when decrypted, produces the same result as if the operations were conducted on the original unencrypted data (plaintext).

![](https://lh7-us.googleusercontent.com/HRuInbJAUPxevuWMw0x_1ZUwMQXhJ4W9KrtMJxj0lPK4t_E4Fn6jx5vbUB9et72CTg7cFnfHzygrECOjp_zBwgvi0vVMPsbWB8igNKolehRgQtCbyhwtAMj1lnAxG1mJeCg49YlRfDY66rewRuJtFfM)

Since its theoretical origins in 1978, FHE has undergone substantial evolution. It has encountered scalability obstacles primarily due to the incremental accumulation of "noise" with each operation, which refers to additional randomness needed for security. Craig Gentry introduced a pivotal technique in 2009 called **bootstrapping**, which reduces noise and restores clarity to computations to address this challenge. However, this process was relatively inefficient until a 2021 breakthrough called “ [_programmable bootstrapping_](https://eprint.iacr.org/2021/091.pdf)” in the implementation of the TFHE scheme allowed for much more efficient bootstrapping and computation over ciphertexts with infinite depth and exactness of a wide range of operations (e,g addition, division, remainder) and comparisons (e,g equal, less than, min/max), making it highly suitable for smart contracts.

Despite these advances, it's important to recognize that FHE computations remain slow. However, research and development efforts led by dozens of entities such as [Intel](https://community.intel.com/t5/Blogs/Tech-Innovation/Data-Center/Intel-Labs-Continues-Focused-Research-and-Standards-Efforts-to/post/1488532), [Cornami](https://cornami.com/), [Optalysis](https://optalysys.com/), and [Fabric](https://www.fabriccryptography.com/) in FHE-dedicated hardware solutions (FPGAs) will **enhance computational speeds by 100-1000x** and are estimated to be production-ready by 2025.

## **Applications of FHE in Web3:**

In the domain of smart contracts, FHE empowers developers to store encrypted data securely on the blockchain itself, enabling **a shared, private state that is both on-chain and composable**. This capability is particularly transformative for applications involving multiple parties like prediction markets. FHE allows various participants to submit their encrypted forecasts regarding the future price of Ethereum directly onto the blockchain, preventing any form of imitation and computes the average price prediction while maintaining the confidentiality of individual inputs.

![](https://lh7-us.googleusercontent.com/jltkhsLxALqY718keyfpuS_U8A_mZIhmElzn3Z_bzZ1QIdGRj3X1WYUanWVu3TCEVFCq9obMm7gvjoemblgqXMI6MgZ__MFZV-nTSPENpqfE_FzsQyTSlDCLLLKQOzAk9v6Xzb9zsvFUkHpLpgWaD7U)

The deterministic nature of FHE enables confidential computations to be executed and verified directly on the blockchain and offers the unique advantage of **traceable confidentiality**. This feature facilitates programmable privacy that can be customized to comply with regional regulatory standards. For instance, in confidential on-chain payments, FHE can create an experience akin to Venmo, where transactions between users are visible to their network, maintaining the transparency of the flow of funds while concealing the specific transaction amounts. This contrasts with the approach of Tornado Cash, which obscures both the sender's and recipient's addresses, making them anonymous and prone to regulatory scrutiny.

## **Universal Confidential Computing Layer, powered by EigenLayer**

[Inco](https://www.inco.org/) is architected as a **modular confidential computing Layer-1** that combines FHE, ZK, TEE, and MPC, secured by Ethereum. Inco represents a significant leap forward in open innovation around blockchain confidentiality. Their FHE-enabled Ethereum Virtual Machine (fhEVM), which employs threshold TFHE, simplifies the complexity of FHE by introducing encrypted data types on-chain and enabling computation on these types within the smart contract. This breakthrough enables Solidity developers to **easily** **build confidential dApps with 20 minutes of developer onboarding, utilizing the familiar Solidity smart contract language and toolings** from the Ethereum ecosystem, such as Metamask, Remix, and Hardhat.

Inco enables the creation of dApps that resemble Web 2.0 applications in various domains such as gaming, DeFi, payments, governance, and identity. A notable key feature is its capability to **generate hidden on-chain randomness**, which utilizes public keys to produce a stream of cryptographically secure bits through FHE. This innovation is illustrated by the fully on-chain [Mafia game](https://twitter.com/framed_gg/status/1716849776183148626?s=20), featuring on-chain hidden role generation and bluffing mechanics, developed on Inco for [ETH Global NYC](https://twitter.com/ETHGlobal/status/1706049370351677476?s=20). This game showcases **Inco’s capacity to support game dynamics that are often difficult to implement on traditional blockchains**. These dynamics encompass resource concealment, secret alliances, espionage, sabotage, hidden randomness, fog of war, and elements of guessing and strategy. Collectively, these elements augment the on-chain experience with more entertainment, complexity, and depth.

![](https://lh7-us.googleusercontent.com/kvDKa9fXEj_Ss792_pm6_MVfH-I8s-1TCuiNIwYb_JOCYNoHWpGu0PwO_d-4sijHISLvOSPW6aGgKh8SizGpv7WJ3bAaLg5LDK2uqoOwOfFq6nSFeTegIa5pnVEOXM1_PRiXJFeC8NGxf7T3McV4K2k)

**Akin to how** [**Celestia**](https://celestia.org/) **provides Data Availability (DA) to Ethereum and other blockchains,** [**Inco**](https://www.inco.org/) **extends confidentiality to Ethereum** and other public Layer-1 and Layer-2 blockchains by providing **confidential storage, computing, and access control**. This is accomplished through externally validated bridges, native bridges, and Inter-Blockchain Communication (IBC) protocol. For instance, a trustless on-chain game developed on Arbitrum, with most of its core application logic hosted there, can utilize Inco exclusively for storing concealed information (e.g., cards, player stats, or resources) or performing private computations (e.g., payments, voting, or hidden attacks).

Inco is built on top of **Cosmos SDK and taps into the security of Ethereum.** The Inco team is excited to explore the integration to EigenLayer for several reasons:

1. **Increased Security:** Inco taps into Ethereum's economic security to prevent risks associated with its native token’s volatility as a new protocol. Through its [dual staking](https://www.blog.eigenlayer.xyz/dual-staking/) mechanism, the cold start (security bootstrapping) problem for new zones can be mitigated.
2. **Sustainable Economic Model:** Reduce the emission rate of its native token rate.
3. **Enhancing Ethereum by leveraging its own security framework:** Introduce native confidentiality service to Ethereum, safeguarded by operators underpinned by Ethereum's economic security.

### **Conclusion**

The quest for trustless confidentiality in the blockchain industry has led to significant advancements in privacy technologies, with **FHE re-emerging as a promising new paradigm**.

The collaboration with Inco highlights the **convergence and symbiotic relationship between the** [**Cosmos and Ethereum**](https://www.blog.eigenlayer.xyz/cosmos/) **, unified by EigenLayer**. Inco showcases the **innovative applied cryptography infrastructure that can be introduced to Ethereum**.

_We continue to seek partners to build protocols leveraging novel cryptography on top of Eigenlayer and invite teams to reach out and learn more about us._

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/#/portal)_Thanks to_ [_Karthik_](https://x.com/ksrini_) _,_ [_Ismael_](https://x.com/Ismael_H_R) _from_ [_Lagrange_](https://x.com/lagrangedev) _,_ [_Nikhil_](https://x.com/pumpernikhil) _from_ [_Aethos_](https://x.com/aethosnetwork) _,_ [_Soubhik_](https://x.com/soubhik_deb) _,_ [_Gautham_](https://x.com/bbuddha_xyz) _, and_ [_Brandon_](https://x.com/bcmakes) _for reviewing drafts of this piece._

Ethereum gave birth to DeFi with the launch of Maker in December 2017. Uniswap and Compound launched soon after, forming an economy around ETH and ERC20s. Since then, we’ve witnessed an onchain financial renaissance with concentrated liquidity enabling greater capital efficiency, the evolution of perps, and even the invention of flash loans, a concept not possible in tradfi.

But we seem to have hit a wall. AMM LPs have lost >$700M in MEV since The Merge. Derivative exchanges have centralized their risk engines and orderbooks for greater efficiency. And we’re unable to provide personalized loans, giving better rates to those with lower default risk or easy access to fixed-rate loans for set periods of time.

Many of the problems stem from Ethereum’s constraints as a finite state machine. It’s gas-constrained, has 12-second block times, and can’t natively take in offchain data. Modular architectures provide a way forward by offloading heavy computation and integrating external data without sacrificing Ethereum’s core security.

If the [EVM is the glue](https://vitalik.eth.limo/general/2024/09/02/gluecp.html) that lets developers write arbitrary business logic, then what form should the coprocessors take? While Vitalik describes these coprocessors as precompiles or opcodes, we need a broader solution. We need coprocessors that can handle tasks too computationally expensive or impractical for Ethereum’s limited state machine, and—most importantly—they must be verifiable.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image.png)_modified diagram from_ [_Vitalik’s Glue and coprocessor architectures_](https://vitalik.eth.limo/general/2024/09/02/gluecp.html)

Developers have been building efficient, specialized services for years, but verifiability changes the game entirely. This is where EigenLayer comes in: it provides the infrastructure to create decentralized networks of node operators, economically aligned to run your arbitrary node software.

We call these decentralized networks Actively Validated Services (AVSs), and they drastically decrease the cost of building verifiable, trustless services.

The intersection of DeFi and AVSs unlocks a range of powerful new use cases:

1. **Trustless offchain compute** (coprocessors): Run heavy computations offchain and return the results onchain with minimal gas fees, secured by zero-knowledge proofs or cryptoeconomic guarantees. Think free limit orders or even AI model calls, all verifiable and decentralized.
2. **Trustless offchain data** (verifiable oracles, zkTLS): Pull real-world data—like prices, volatility, real-time liquidity, even sports data—securely into DeFi.
3. **And beyond**: Auction networks, policy layers, decentralized orderbooks—AVSs extend DeFi into previously unreachable areas.

We call this new paradigm Intelligent DeFi because it brings real-time adaptability and personalization to decentralized finance. Leveraging trustless offchain compute and data, Intelligent DeFi enables smarter decisions. In this blog, we dive into 10 use cases that showcase its potential.

# Exchanges

Exchanges are a core primitive of DeFi, yet less than [15% of spot](https://www.theblock.co/data/decentralized-finance/dex-non-custodial) and [6% of derivatives](https://www.theblock.co/data/decentralized-finance/derivatives/dex-to-cex-futures-trade-volume) trading happens onchain. Intelligent DeFi can close this gap, making DEXs more competitive with their offchain counterparts.

## 1\. VIP tier: volume-based fee tiers

Centralized exchanges offer tiered fees based on volume, not just to build loyalty, but to subsidize market makers, enabling them to quote tighter spreads and offer better prices to retail traders. This generates more volume for the exchange.

Implementing volume-based fees on a DEX is challenging. To determine traders’ volumes, a DEX would need to either:

- Dynamically compute the volume
- Store and update the volume for every trader with every trade
  - Tracking rolling 30-day volumes adds further complexity, requiring historical storage and subtraction

Both methods are prohibitively expensive onchain. But by outsourcing computation to a coprocessor like Lagrange or Brevis, we can verifiably compute traders’ volumes for every transaction.

How does this work?

1. Coprocessors index and store a subset of blockchain data in a queryable relational database.
2. The AMM (or [Uniswap hook](https://www.blog.eigenlayer.xyz/univ4-hooks/)) smart contract calls the coprocessor to run a SQL query that calculates the trader’s fees over a set period.
3. The coprocessor returns the verified results to the AMM via a callback, along with a zero-knowledge proof confirming the computation on historical blockchain data.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-1.png)How onchain contracts can interact with zkCoprocessor Lagrange.

## 2\. Dynamic, asymmetric fees for AMMs

[Loss versus rebalancing (LVR)](https://arxiv.org/pdf/2208.06046) is a major problem affecting LP profitability in AMMs. LVR occurs due to the price discrepancies between continuously trading offchain exchanges and onchain AMMs, which only trade every block or 12 seconds for Ethereum Mainnet. [A lot can happen in a block](https://x.com/0x81B/status/1679523642168979456), and at the start of the next block, searchers arbitrage price differences between exchanges.

To improve LP profitability, AMMs can introduce dynamic fees and asymmetric fees:

1. **Dynamic Fees:** Adjust fees based on market volatility. LPs generally fare worse during periods of high volatility. During high volatility, higher fees protect LPs from toxic trades, while lower fees during stable periods encourage volume. This reduces liquidity fragmentation across different fee tiers, making the user experience simpler for LPs. Check out this [very basic proof of concept](https://github.com/ishaan0x/dynametric/).
2. **Asymmetric Fees**: Inspired by [Alex Nezlobin](https://x.com/0x94305/status/1674857993740111872), asymmetric fees would adjust the spread based on external price data. For example, if ETH trades at $1000 on DEXs and $1050 on CEXs, instead of maintaining a symmetrical spread around the DEX price, the AMM could buy at $980 and sell at $1060, reflecting more realistic market conditions.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-2.png)From [Alex Nezlobin’s twitter thread](https://x.com/0x94305/status/1674857999645696000)

In both cases, AMMs need trusted external data—like CEX prices or volatility—to adjust fees. But traditional oracles introduce risks: centralized operators can fail or deliver stale data. Instead, zkTLS (web proofs) offers a better approach. By cryptographically verifying data directly from web servers, zkTLS removes the need to trust third parties. It gives you tamper-proof data in real-time, ensuring AMMs can compute dynamic, asymmetric fees securely, whether onchain or through a coprocessor.

## 3\. Auction to redistribute MEV to AMM LPs

Another way to boost LP profitability involves not just offchain compute, but a decentralized auction network. Currently, searchers compete in auctions to get their transactions at the top of the block. Effectively, the arbitrage goes to a mix of searchers, builders, and proposers, rather than the LPs and swappers.

Instead, AMMs can auction off the right to be the first swap through the pool. If the auction is competitive, most of the LVR will be recaptured. This can then be distributed pro-rata to the underlying LPs in the range of the swap, reducing overall arbitrage and enabling LPs to provide tighter spreads. [Sorella](https://sorellalabs.xyz/) is building this in the form of a Uniswap v4 hook.

The challenge is running a low-latency, censorship-resistant auction. It’d be too complex and expensive to run the auction onchain: each bid would require gas. By the time the winner is chosen, a block would have passed, rendering the auction incomplete. A centralized party could conduct the auction offchain, but this goes against the general ethos of DeFi, while also giving them a last look to potentially extract value for themself.

The solution is a decentralized set of operators running a [leaderless auction](https://www.paradigm.xyz/2024/02/leaderless-auctions), eliminating the need for a single entity and preserving the integrity of the process. The operators select the winning bid and return proceeds to LPs.

0:00

/0:49

1×

From [Paradigm’s Leaderless Auctions](https://www.paradigm.xyz/2024/02/leaderless-auctions)

# Derivatives

While most derivatives are traded on exchanges, Intelligent DeFi unlocks unique use cases specific to this asset class. Let’s dive in!

## 4\. Advanced margin system

Currently, traders can’t express cross-asset viewpoints, like SOLETH or cross, without severely limiting their leverage. Most perp DEXs calculate margin linearly based on the sum of a trader’s open interest across different positions.

For example, if I deposit $10k and take a $50k long on ETH and a $50k short on BTC, that counts as 10x leverage. But this is a different risk profile from someone who simply longs $100k on ETH, and these accounts shouldn’t be treated the same. Ideally, traders should be able to leverage more than 5x ETHBTC without facing such rigid constraints.

The issue lies in onchain compute limits. Specifically, the system needs to account for collaterals (of every spot asset), positions (of every perp asset), unrealized PnL, each perp’s initial and maintenance margin factors, and correlations / delta hedging. This is especially true as DEXs expand to multiple asset types, like perps and options.

By leveraging a coprocessor for more complex calculations to determine accounts’ margin factors, DEXs can create a more customized risk engine tailored to their users’ needs. This enables more flexible, delta-neutral strategies and ensures that liquidations only happen when truly necessary.

To offer even more flexibility, coprocessors can adjust margin requirements dynamically, factoring in real-time liquidity from major CEXs and the open interest in each pool.

[![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-3.png)](https://docs.aevo.xyz/aevo-exchange/options-specifications/portfolio-margin) Aevo relies on a centralized risk engine to determine worst-case scenarios, providing better margin parameters for high-value traders. Coprocessors enable unique ways of calculating margin without compromising on decentralization. From [Aevo’s docs](https://docs.aevo.xyz/aevo-exchange/options-specifications/portfolio-margin).

## 5\. Pricing an options AMM

AMMs for derivatives, especially options, are exciting yet [controversial](https://adjacentresearch.xyz/writing/ramblings_on_amms_and_orderbooks.html). Some believe that they can never be priced accurately; others believe that derivatives only make sense for high-volume assets and that orderbooks are superior for said assets. Nonetheless, Panoptic, [Deri](https://deri-protocol.medium.com/amm-or-orderbook-that-is-the-question-5580b16f1bef), and others believe that AMMs are the ultimate method to provide liquidity, [including for options](https://lambert-guillaume.medium.com/pricing-uniswap-v3-lp-positions-towards-a-new-options-paradigm-dce3e3b50125).

For an options AMM to truly take off, one key factor will be the inclusion of offchain data—such as volatility, historical pricing, and real-time market signals. Additionally, offchain compute will be necessary to model more advanced pricing frameworks like Black-Scholes. Integrating these external data points with onchain trading mechanisms will be crucial to ensure accurate pricing, lower slippage, and better capital efficiency for options traders.

# Lending

Lending protocols face unique challenges, where AI and offchain compute can drive smarter, more dynamic solutions.

## 6\. AI system for parameters

Currently, governance manually updates parameters around lending markets in protocols like Aave and Compound. Generally, a risk service provider like Gauntlet runs model-based simulations and proposes changes to parameters like base rates, collateral factors, liquidation factors, and more. During more serious incidents, they can propose to delist or freeze an asset.

This appr\`oach has two major flaws:

1. The latency is far too long. When I was a delegate for Aave DAO, proposals took at least a week to go through.
2. Governance is not well-informed to vote on lending parameters, nor are all members fully active. The recent Compound governance attack is a prime example of this.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-4.png)_Aave’s governance process takes at least 5 days, from_ [their docs](https://docs.aave.com/governance/master/governance-process) _._

Morpho and Euler v2 move a step in the right direction. They modularize the risk management portion, enabling anyone to curate their own instance of a lending platform. Users choose where to park their assets based on the curator’s track record and reputation. This approach can minimize delays in updating parameters.

But in an ideal system, parameters would update automatically, responding in real-time to both onchain and offchain liquidity. AI-driven models simulate various scenarios, predicting and preventing worst-case outcomes. These models rely on AI-focused coprocessors like Ritual, Sentient, Hyperbolic, Ora, and Valence to process vast amounts of data offchain—accounting for volatility, liquidity shifts, and risk correlations—before verifiably publishing results onchain.

## 7\. Personalized loans via account history and liquidation risk

In traditional finance, borrowers with strong credit histories receive better loan terms, but in DeFi, everyone gets the same lending terms, regardless of their risk profile or track record. While beneficial, I believe DeFi has the ability to combine the best of both worlds: offering fair trustless loans to anyone, and loans with favorable terms to repeat, identified borrowers with low liquidation risk.

Without differentiation, DeFi lending protocols can’t offer customized terms, such as lower collateral requirements or better interest rates, to low-risk borrowers. This lack of personalization not only limits the potential upside for repeat users but also makes lending markets inefficient.

Offering personalized loans first requires an anti-sybil solution to ensure that only verified users receive better terms. Solutions like WorldCoin or Coinbase Verifications prevent malicious actors from repeatedly exploiting the protocol with bad debt.

Once borrowers are verified, the protocol can collect onchain information to build a liquidity profile, including:

- \# of loans, both current and historical
- Repayment history of previous loans
- Onchain net worth and outstanding debt
- NFTs owned (if the lending protocol partners with NFT projects to offer subsidized terms)

The protocol can even look at other addresses tied to the same identity to get a more holistic view!

Finally, a coprocessor can assess liquidation risk and generate custom collateral factors and interest rates, ensuring tailored loan terms for each borrower.

# 8\. Compliant privacy mixers

In August 2022, the U.S. Office of Foreign Assets Control (OFAC) [sanctioned Tornado Cash](https://www.chainalysis.com/blog/tornado-cash-ofac-designation-sanctions/) for enabling money laundering. Privacy, however, is a fundamental right and serves legitimate purposes: people should be able to transfer funds to another account or a friend without exposing their full transaction history.

The problem is that existing privacy mixers don’t differentiate between legitimate users and bad actors. This lack of compliance makes them a target for sanctions and prevents broader adoption.

What if we could create a privacy protocol that only accepts compliant funds? The protocol would manage risks and adhere to regulations, and privacy-conscious users would flock to the system. But determining compliance requires various onchain and offchain data and is a nontrivial task. Ideally, smart contracts can call an API that enforces compliance by approving only valid transactions.

[Aethos](https://aethos.network/) is a policy layer that does exactly this. It enables developers to set rules at the smart contract level, ensuring that transactions follow specific compliance policies. For example, a compliant privacy mixer could set rate limits, time locks, and block transactions in which the depositing or withdrawing address is OFAC-sanctioned or tied to a DeFi hack.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-5.png)From [Aethos’s docs](https://docs.aethos.network/essentials/design)

Incorporating real-time, rule-based policies into smart contracts opens a new wave of institutional-friendly DeFi, where compliance and DeFi values are no longer at odds.

# 9\. Automated rebalancing yield protocols

DeFi offers [an abundance of yield opportunities](https://defillama.com/yields), across various assets and protocols—staking, restaking, lending, AMM liquidity pools, RWAs, and more. Users each have unique risk preferences, tied to the protocol type, chain (Ethereum, Solana, etc.), and asset denomination, along with external market risks. With so many choices, some traders turn to yield protocols to automate their capital allocation.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/image-6.png)So much yield. Not financial advice. From [DeFiLlama](https://defillama.com/yields).

These protocols can leverage AI models to optimize yields across multiple sources. Developers set predefined risk parameters (e.g., capping exposure to 15% per protocol or avoiding protocols with under $100M in TVL), and AI models handle the rest, adjusting the portfolio to meet these criteria while maximizing returns.

Going further, AI models can create fully customized yield strategies for each user, based on their onchain activity and preferences collected through a brief questionnaire. This level of personalization—previously unthinkable—is now within reach, thanks to the scalability of AI.

On the backend, AI-driven coprocessors monitor and rebalance portfolios. They only trigger changes when the benefits justify the gas costs, ensuring efficient, data-driven portfolio management.

# 10\. Hyper-targeted incentives programs

Incentives are core to crypto and DeFi. DeFi summer only truly kicked off with Compound’s introduction of liquidity mining in 2020. By offering rewards for specific user actions, protocols were able to bootstrap growth and activity.

But as the space matures, protocols are seeking more precise targeting and often move to offchain programs. AMMs may focus on incentivizing active liquidity. NFT marketplaces and prediction markets might reward liquidity near the orderbook price. Lending protocols could encourage borrowing from non-loopers for at least 20% of their interest.

With coprocessors, protocols can define complex reward conditions and issue payouts in real time. This moves away from the increasingly unpopular points system and provides users with certainty that rewards will be paid, reducing the cost of capital for protocols. [Gearbox is already shifting](https://www.lagrange.dev/blog/zk-coprocessor-1-0) to Lagrange’s coprocessor to handle multi-asset rewards with different payout mechanics. By making incentives more efficient, DeFi can continue to grow while rewarding the most valuable actions.

# Conclusion

The intersection of DeFi and AVSs will lead to a new financial revolution. From MEV-recapturing AMMs to real-time policies in privacy protocols, these use cases are just a glimpse of what can be built when we push the limits of decentralized finance. If you’re interested in building in Intelligent DeFi or AVSs to support new Intelligent DeFi products, join the [BuildOnEigen telegram chat](https://airtable.com/appnYZo360sWuEYLS/shrz6Pstds7EXjC5N), follow on [Twitter](https://twitter.com/ishaan0x), and reach out directly to the Developer Relations team ( [@dabit3](https://x.com/dabit3))

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intelligent-defi/#/portal)

Subscribe_Thanks to_ [_Kevin_](https://x.com/knwang) _from_ [_Khalani_](https://khalani.network/) _,_ [_Hart_](https://x.com/hal2001) _and_ [_Nick_](https://x.com/mountainwaterpi?lang=en) _from_ [_Across_](https://across.to/) _,_ [_Khushi_](https://x.com/khushii_w) _from_ [_Predicate_](https://predicate.io/) _,_ [_Jon_](https://x.com/nosleepjon) _from_ [_Hyperlane_](https://hyperlane.xyz/) _,_ [_Soubhik_](https://x.com/soubhik_deb) _, and_ [_Brandon_](https://x.com/bcmakes) _for reviewing drafts of this piece._

Intents hold the potential to transform how we interact with Ethereum, yet today, they risk creating fragmented “walled gardens” where each intent-based application exists in isolation. Rather than isolated ecosystems, we need invisible gardens — a landscape where intent-based applications interconnect and interact fluidly, because [composability](https://x.com/ishaan0x/status/1845936557351682381) [matters](https://x.com/jillrgunter/status/1845915395599036650/quotes).

In [Part 1](https://www.blog.eigenlayer.xyz/intention-is-all-you-need), we introduced intents and explored how AVSs (Actively Validated Services) enhance them across single and cross-chain contexts. Now, we’ll dive deeper, examining how intent-based applications can inadvertently lead to fragmentation — and how AVSs, through a Modular Intent Stack, unlock the power of fusion across the broader ecosystem.

# Intent fragmentation

CoW Swap and Across are incredible examples of intent-based apps, as are UniswapX, OpenSea, Blur, dYdX, and DEX aggregators like 1inch and Matcha. Intents are inherently user-centric and platform-agnostic, so, in theory, these applications should be composable.

However, the surrounding solving infrastructure — the systems that determine how intents are shared with solvers and how solvers compete to fulfill them — is currently app-specific. This creates intent fragmentation, where each intent-based app exists in isolation.

To better understand this fragmentation, let’s double click into how CoW Swap collects intents and shares intents with solvers. CoW Swap currently maintains a centralized orderbook to host user orders. While anyone can call the orderbook’s API, this setup creates a single point of potential failure or censorship. Further, CoW Swap runs a centralized engine to conduct auctions and store solvers’ bids.

This isn’t a critique of CoW Swap ( [progressive decentralization](https://a16zcrypto.com/posts/article/progressive-decentralization-crypto-product-management/) is a good thing, actually). Rather, it highlights issues that we can collectively work towards solving (pun not intended):

1. **Low-latency, censorship-resistant environments:** Intents need a fast, resilient space to propagate to solvers without risk of censorship.
2. **Unified integration for solvers:** Solvers need a single, standardized interface for viewing intents. Currently, CoW Swap requires solvers to integrate a custom driver for its interface. While functional, this approach doesn’t scale across multiple applications.

Put more eloquently, “ [intent-based apps don’t scale, intents do.](https://youtu.be/G0nFyq9DDPw?si=ylqJ-ZgviKckjaeU&t=1628)” All that’s missing is the solving infrastructure to unlock their potential.

# Modular Intent Stack

To scale intents beyond isolated applications, we need a shared process to guide intents from a user’s wallet all the way to execution. This requires three essential components:

1. **Representation**: Ways to represent intents across applications, either through standards or a full specification language.
2. **Propagation**: Methods to share intents across solvers.
3. **Selection**: Processes to choose the winning solver to settle an intent.

I call this the Modular Intent Stack.

## ERC-7683: representing cross-chain intents

Intents have far more degrees of freedom than standard, ready-to-execute transactions. By converging on common standards to share intents, intent-based applications could enable solvers to integrate seamlessly across platforms rather than requiring individual integration with each app. This unified approach creates a stronger, more resilient solver network, ultimately benefiting users.

Interestingly, this also shifts competitive focus: instead of competing to maintain exclusive solver networks, applications can focus on enhancing other layers of the stack.

Across and Uniswap Labs took a pragmatic approach, partnering to propose [ERC-7683](https://www.erc7683.org/), a standard for cross chain intents. Just as ERC-20 defines a standard for fungible tokens, ERC-7683 lays out common requirements for all cross-chain intents. ERC-7683 sets a standard flow but leaves implementation details flexible.

[![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-7.png)](https://blog.uniswap.org/uniswap-labs-and-across-propose-standard-for-cross-chain-intents) From [erc7683.org](https://www.erc7683.org/). Authored by [Across](https://medium.com/across-protocol/across-and-uniswap-labs-propose-standard-for-cross-chain-intents-to-accelerate-cross-chain-0423f2242f7c) and [Uniswap Labs](https://blog.uniswap.org/uniswap-labs-and-across-propose-standard-for-cross-chain-intents).

Across and Uniswap’s approach is bottom-up: it starts with the existing Ethereum and rollup ecosystems, extends with Across’s bridge and UniswapX, and proposes a standard that others can build on while maintaining composability.

Alternatively, intents could be represented with a full specification language, providing more flexibility but adding significant complexity. We’ll discuss solutions that adopt this approach in Part 3.

## IntentPool: storing and propagating intents

Aligning on a common standard is not enough. We also need a common decentralized network to transmit intents for solvers to access and settle. This is critical to decentralize the solver selection process and ensure censorship resistance of intents.

So, what does the intentpool look like?

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image.png)From Paradigm’s [Intent-Based Architecture and Their Risks](https://www.paradigm.xyz/2023/06/intents).

Before diving in, let’s outline our goals an intentpool:

- **Censorship resistant** for both users submitting intents and solvers submitting solutions
- **Low latency** to ensure quick access for both users and solvers
- **Denial-of-service resistance** to protect against attacks

### Current approach: centralized database

Currently, intent-based applications like CoW Swap and UniswapX maintain centralized databases to store intents and provide read access to (often whitelisted) solvers. While functional, this approach is neither censorship-resistant nor sustainable.

### New approach 1: repurposing the Ethereum mempool

Transactions and intents share similar requirements. In Ethereum, transactions propagate through a network of validators, who add them to blocks when selected as block proposers (either directly or with help from a builder). Validators maintain local mempools and gossip transactions to the network within 1-2 seconds, creating a censorship-resistant network for Ethereum transactions.

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-1.png)

If the Ethereum mempool works so well for transactions, why not adapt it to also support intent propagation?

This approach works well in certain cases, specifically when an intent is combined with a gas-required transaction and is low MEV. Across, for example, uses the Ethereum mempool to transmit its intents to solvers. Users pay gas to lock up their collateral, and because the intent is simply to bridge assets between chains, it’s low MEV and doesn’t present an attractive target for manipulation.

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-2.png)

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-3.png)

However, repurposing the Ethereum mempool isn’t feasible for intents in the general case, primarily due to DOS risks. For transactions, gas serves as a natural deterrent, preventing malicious attackers from attempting to DOS the network. Supporting gasless intents would require Ethereum nodes to validate that intents are solvable before forwarding them, which is a heavy burden, especially given that most node operators are not involved in solving. Forwarding intents without validation would simply enable malicious attackers to send spam intents, overloading the network.

Intents have far more degrees of freedom than transactions, and handling this complexity would severely strain Ethereum nodes without relying on a [trusted party](https://ethresear.ch/t/a-decentralised-solver-architecture-for-executing-intents-on-evm-blockchain/16608/1) or restrictive limitations on intent functionality. As [Georgios and Quintus](https://www.paradigm.xyz/2023/06/intents) note, “generic support for fully general intents in the Ethereum mempool is out of the question even in the long term.”

### New approach 2: let solvers propagate intents

While the mempool mechanism works well, intents often require more complex validation and thus higher bandwidth than general-purpose Ethereum validators can provide. What if we let solvers propagate intents?

This approach, however, introduces new risks of censorship. The mempool relies on nodes openly sharing transactions across the network, but many intents hold inherent profitability for the solvers who execute them. This incentive can lead solvers to keep high-value intents private, limiting broader access and undermining censorship resistance.

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-4.png)

### New approach 3: store intents on a low-latency data availability network

The previous methods relied on node propagation networks, which work well for transactions but are less suited for intents due to the more complex validation required. The current approach uses a centralized server-client architecture, where a servr acts as a repository for intents, storing and organizing them until solvers (clients) request access. But what if we could retain this server-client model while decentralizing the server itself?

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-5.png)

One solution for an intentpool is to use a low-latency data availability (DA) network to store intents. DA networks are purpose-built to ensure that all posted data is accessible to all participants (read: censorship resistance) through decentralized storage and data redundancy. Further, EigenDA in particular has significant excess capacity (see the “ [All chains fit on EigenDA](https://x.com/sreeramkannan/status/1834265535598788793)” stunt).

EigenDA has two active concerns:

- **Centralized data dispersal:** Currently, EigenDA only supports data posting from a centralized disperser. While the architecture is designed to eventually allow anyone to disperse data (or intents) to the network, this feature is not yet live. Even so, while write access remains permissioned, read access is fully censorship-resistant, offering meaningful improvement over the current centralized approach.
- **Latency requirements:** For optimal execution, intents need sub-second latency, which remains a challenge. Reducing latency is actively being worked on to meet the demands of high-frequency intent execution.

Both concerns will be addressed. In the long run, I envision low-latency DA networks serving as the “public mempool” for intents.

### Use case: CoW Swap and UniswapX with EigenDA

CoW Swap and UniswapX are ideal candidates for using a low-latency DA network like EigenDA to store intents. Both currently store user intents and solver bids (or bid representations via scoring) in a centralized database. Since they follow the client-server architecture for intent dissemination, it’ll be a natural upgrade for solvers with minimal uplift.

Further, once EigenDA’s concerns are solved, CoW Swap and UniswapX will have a strictly better solution:

- **Censorship resistance**: Users wouldn’t have to rely solely on CoW Swap and UniswapX to publish intents. If either platform chooses to censor (whether directly or due to regulatory pressures), users could publish intents to EigenDA independently.
- **Consistency for solvers**: Solvers would receive a unified, consistent view of intents, ensuring a level playing field. This is especially important for CoW Swap’s batched swaps, where uniform access to intents is crucial.
- **Verifiable best execution**: Users could verify that they achieved best execution by comparing all solver bids stored on EigenDA, adding a layer of transparency.

Overall, CoW Swap and UniswapX can pair with EigenDA to build towards a more verifiable, intents-based future.

## Auction AVS: selecting a solver

Building on the benefits of a decentralized intent store with EigenDA, we also need a fair, transparent way to select solvers to execute these intents. There are several ways to determine which solver wins an intent — ranging from RFQs, where solvers compete based on speed alone, to price-based auctions.

Currently, most protocols host solver bids and select the winning solver bid via a centralized auction, which carries risks of censorship or manipulation. However, protocols can leverage an Auction AVS to conduct [censorship-resistant auctions](https://www.blog.eigenlayer.xyz/intelligent-defi/), ensuring the best bid is chosen without centralized oversight.

As protocols move toward decentralized storage and propagation of intents, a censorship-resistant auction layer powered by an Auction AVS provides the final step to a fully decentralized, fair, and verifiable intent-based architecture.

![](https://www.blog.eigenlayer.xyz/content/images/2024/11/image-6.png)From [How can we decentralize intents?](https://ethresear.ch/t/how-can-we-decentralize-intents/16456) on [ethresear.ch](http://ethresear.ch/).

# Conclusion

Intents offer a powerful new paradigm for interacting with decentralized systems, but they risk fragmentation across isolated applications. Actively Validated Services like EigenDA and Auction AVSs created the shared solving infrastructure needed to fuse intents into a unified, interoperable ecosystem.

If you’re building in intents and want to join us in building an invisible garden, [start here](https://linktr.ee/buildoneigen), join the [BuildOnEigen telegram chat](https://airtable.com/appnYZo360sWuEYLS/shrz6Pstds7EXjC5N), follow along at [@buildoneigen](https://x.com/buildoneigen) on X and reach out to our Head of Developer Relations [Nader](https://x.com/dabit3) or the author [Ishaan](https://twitter.com/ishaan0x).

In the final part, we’ll discuss next-generation architectures that supercharge intents via composable solving.

_Thanks to_ [_Kevin_](https://x.com/knwang) _from_ [_Khalani_](https://khalani.network/) _,_ [_Hart_](https://x.com/hal2001) _and_ [_Nick_](https://x.com/mountainwaterpi?lang=en) _from_ [_Across_](https://across.to/) _,_ [_Khushi_](https://x.com/khushii_w) _from_ [_Predicate_](https://predicate.io/) _,_ [_Jon_](https://x.com/nosleepjon) _from_ [_Hyperlane_](https://hyperlane.xyz/) _,_ [_Soubhik_](https://x.com/soubhik_deb) _, and_ [_Brandon_](https://x.com/bcmakes) _for reviewing drafts of this piece._

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/#/portal)

Subscribe_Thanks to_ [_Kevin_](https://x.com/knwang) _from_ [_Khalani_](https://khalani.network/) _,_ [_Hart_](https://x.com/hal2001) _and_ [_Nick_](https://x.com/mountainwaterpi?lang=en) _from_ [_Across_](https://across.to/) _,_ [_Khushi_](https://x.com/khushii_w) _from_ [_Predicate_](https://predicate.io/) _,_ [_Jon_](https://x.com/nosleepjon) _from_ [_Hyperlane_](https://hyperlane.xyz/) _,_ [_Soubhik_](https://x.com/soubhik_deb) _, and_ [_Brandon_](https://x.com/bcmakes) _for reviewing drafts of this piece._

**Intents are transforming how we think about interactions on Ethereum.**

Instead of rigid transaction paths, intents open up a world where users declare their desired outcome — and the network takes care of the rest. From intent-based applications to modular protocols that compose seamlessly with Ethereum, to full-blown intents-first systems, the possibilities are endless.

But none of these visions can scale without verifiable offchain infrastructure — and that’s where Actively Validated Services (AVSs) come in. In Part 1 of this research series, we’ll dive into the intent-based applications of today: how they operate in both single-domain and cross-chain contexts, and how AVSs can unlock new efficiencies for them.

Let’s dive into the future of intents!

## What are intents?

Enough [has](https://www.paradigm.xyz/2023/06/intents) [been](https://members.delphidigital.io/reports/wtf-is-anoma-part-1-wtf-are-intents) [written](https://ideas.skip.build/t/a-formal-ish-definition-for-intents/73/3) [on](https://www.youtube.com/watch?v=G0nFyq9DDPw) [intents](https://blog.khalani.network/intents-solvers-and-everything-aggregators) since Chris Goes coined the term while working on [Wyvern](https://wyvernprotocol.com/), the precursor to OpenSea’s famous Seaport contract, so I’ll keep this brief.

Intent allow users to express desired state transitions — subject to specific constraints and preferences. While a traditional transaction locks in a specific execution path (“trade $1000 USDC for ETH on the Uniswap v3 USDC-ETH 0.3% fee pool with no more than 0.5% slippage”), an intent authorizes the desired end state (“trade $1000 USDC for as much ETH as possible”).

> intent (declarative): I will pay 20 bucks for a Hawaiian pizza at my door by 8pm
>
> tx (imperative): hop on to ur scooter, go to Domino's around the corner, order item#4, and bring it. here is your 20 bucks; 18 for pizza, and 2 for gas.
>
> — Can Gurel (@CannnGurel) [May 29, 2023](https://twitter.com/CannnGurel/status/1663292583550803969?ref_src=twsrc%5Etfw)

A real-life version of an intent vs. a transaction.

From a user’s perspective, intents are typically initiated by signing a message that declares their desired outcome. Once signed, any third party—usually called a solver—can execute the intent. Intents provide several distinct advantages for end users:

1. **Revert-Free Guarantee**: A Uniswap transaction can fail due to high slippage or insufficient gas, yet the user will still pay gas fees for the failed transaction. With intents, transactions only land onchain if successfully executed.
2. **Abstracted Optimization**: Users don’t need to worry about finding the best place to swap or getting front-run. They simply submit an intent, and a solver will execute it optimally.
3. **Chain Abstraction**: Users can sign a single message to transfer assets, without manually bridging across chains. Intents can operate across multiple chains, and users can even allowlist or blocklist certain chains.
4. **No Gas Fees**: Users don’t need to hold a chain’s native asset or worry about paying gas fees. Solvers handle gas and take their fees in an asset the user already holds.
5. **Conditional Execution**: Intents are often [jokingly called](https://x.com/nickwh8te/status/1835580379383935306) a rebrand of limit orders. For example, a user can set an intent to swap USDC for ETH only if ETH hits $2500.

![Differences between an Ethereum transaction and an Anoma intent. From Delphi Digital’s WTF Is Anoma? Part 1: WTF Are Intents?](https://www.blog.eigenlayer.xyz/content/images/2024/10/Screenshot-2024-10-02-at-12.26.03-AM.png)Differences between an Ethereum transaction and an Anoma intent. From Delphi Digital’s [WTF Is Anoma? Part 1: WTF Are Intents?](https://members.delphidigital.io/reports/wtf-is-anoma-part-1-wtf-are-intents)

## Flow of an intent

Let’s walk through the flow of an intent in the real world using [CoW Swap](https://cow.fi/), an exchange that batches user trades for better execution. CoW Swap is one of the most widely used intent-based apps. Here’s how it works:

1. **Users express intents**: Users share their limit orders as signed messages to CoW Swap.
2. **Protocol shares intents**: CoW Swap collects these intents and shares the aggregate with solvers, aiming to find the best overall execution for traders.
3. **Solvers compete on intents**: Solvers run algorithms to determine the best execution. They look for a coincidence of wants—where two opposing trades, like buying ETH with USDC and selling ETH for USDC, cancel each other out—and they route to both onchain and offchain liquidity sources. Solvers then submit their bids to CoW Swap within the allocated time.
4. **Solver settles intents**: CoW Swap selects the winning bid and alerts the solver. The winning solver settles all intents in a single transaction.

![](https://www.blog.eigenlayer.xyz/content/images/2024/10/image--3-.png)A visual representation of the flow of intents in CoW Swap. From [CoW Swap’s docs](https://docs.cow.fi/cow-protocol/concepts/how-it-works/flow-of-an-order).

Overall, CoW Swap delivers many of the key benefits of intents and saves costs by batching orders. However, its atomic settlement only works within a single chain. What happens when we extend intents to cross-chain bridging and swapping?

## Cross-chain intents

Cross-chain intents introduce two major challenges: asynchronicity and non-atomicity. When executing an intent across chains, each chain is unaware of the other’s state. Unlike a single-chain transaction, where actions can be bundled into one atomic step, cross-chain transactions must rely on two separate actions—locking collateral on the origin chain and executing on the destination chain—which cannot be combined into a single transaction.

This lack of atomicity introduces risk for solvers. In theory, solvers should wait until the user’s collateral is finalized on the origin chain. In practice, however, users demand speed, and solvers take on the risk of executing on the destination chain before finalization. This means solvers must rely on their own capital and are vulnerable to origin chain reorgs.

## How Across solves cross-chain intents

We can best understand cross-chain intents by examining [Across](https://across.to/), an intents-based bridging protocol that consistently offers some of the fastest settlement times. Here’s how their system works, with the **new steps** that address the challenges of cross-chain intents:

1. User expresses intents: The user shares their order as a signed message with Across.
2. **Protocol locks user collateral**: Across locks the user’s collateral on the origin chain. Because payment will occur on another chain, the transaction is not atomic. Locking this collateral ensures Across can repay the solver (called a “relayer” by Across).
3. Protocol shares intents: The transaction that locks the user’s collateral also broadcasts the intent to solvers, who monitor the origin chain for opportunities.
4. Solvers compete on intents: Solvers race to settle the intent. Currently, Across has fixed fees, and solvers compete on speed.
5. Solver settles intents: Solver pays user on the destination chain.
6. **Protocol repays solver**: Once the intent is fulfilled, Across repays the solver on the origin chain after a challenge period. If no challenge is made, the locked collateral is released. (Across also enables solvers to be repaid on any chain they specify, via maintained LP pools.)

![](https://www.blog.eigenlayer.xyz/content/images/2024/10/image--4-.png)Full flow of intents in Across. The exact process (and the numbering) differ because I simplified the explanation for this article. From [Across v3’s docs](https://docs.across.to/concepts/intents-architecture-in-across).

While Across’s design is fast, it comes with trade-offs. The added complexity (steps 2+6) comes from the non-atomicity of cross-chain transactions. Users need to lock their capital on the origin chain, which costs gas. Solvers need to wait before getting repaid, which decreases capital efficiency. Even with these constraints, Across solvers typically fulfill orders in under a minute.

## Upgrading cross-chain intents with AVSs

While users earn rewards quickly, it takes time for solvers to be repaid. We can expedite solver repayments by relying on verifiable offchain infra, via EigenLayer AVSs. AVSs allow us to design different cross-chain intent platforms with unique trade-offs.

### 1\. Fast intent verification for solver repayments

Currently, Across pays its solvers after waiting a challenge period. While solvers are waiting to be repaid, they can’t use that capital to fulfill other orders. This decreases the overall capital efficiency of the intents protocol and increases the upfront capital requirements for a solver to be competitive.

We can solve this with a “fast intent verification” AVS. Rather than wait for the challenge period to pass, the EigenLayer operators for the fast intent verification AVS can make a credible commitment that the solver did fulfill the user’s order on the destination chain. Then, Across can pay the solver on the origin chain immediately (in seconds, not an hour).

![](https://www.blog.eigenlayer.xyz/content/images/2024/10/image--7-.png)How the fast intent verification AVS would work, if solvers and AVS operators act honestly.![](https://www.blog.eigenlayer.xyz/content/images/2024/10/image--8-.png)How the fast intent verification would work, if solvers and AVS operators act maliciously.

During the challenge period, anyone can still submit a challenge. If it’s shown that the solver in fact did not pay the user, the operators are slashed for their malicious attestation. These funds are redistributed to the user in a [StakeSure](https://arxiv.org/abs/2401.05797)-like mechanism.

This design is inspired by fast finality AVSs like [AltLayer MACH](https://docs.altlayer.io/altlayer-documentation/restaked-rollups/mach-for-faster-finality) and [Nuffle’s NFFL](https://nuff.gitbook.io/nffl-docs/protocol-design/overview) which cater to optimistic rollups.

At its core, this design shifts capital obligations from active participants to passive ones. Solvers have high cost of capital, whereas AVS operators will have a lower cost of capital. Thus, freeing up solvers’ capital in exchange for locking AVS operators’ capital is net positive.

### 2\. Solver payment before fulfillment

Currently, Across locks user funds on the origin chain, the solver pays the user on the destination chain, and then Across pays the user on the origin chain. These three steps all require gas.

Theoretically, we could simplify this to two steps:

1. User pays solver on the origin chain.
2. Solver pays user on the destination chain.

![](https://www.blog.eigenlayer.xyz/content/images/2024/10/image--9-.png)How the solver payment before fulfillment AVS would work, if solvers act honestly. Solvers are also EigenLayer operators.

However, this requires us to trust the solver to fulfill their end of the bargain. If the solvers register as EigenLayer operators, they can be cryptoeconomically trusted. If they fail to pay the user within predefined criteria, the solvers can be slashed, and slashed funds will be redistributed to the user.

![](https://www.blog.eigenlayer.xyz/content/images/size/w2400/2024/10/image--10-.png)How the solver payment before fulfillment AVS would work, if solvers act maliciously. Solvers are also EigenLayer operators.

Importantly, there’s a limit to how much these solvers can be trusted. Assuming a 1 hour challenge period, solvers’ rolling 60-minute limit must be ≤ their slashable amount for this AVS.

[Relay Protocol](https://relay.link/) is building a gas-efficient intents-based bridge with this design.

## Conclusion

Intent-based applications are already live on Ethereum today. We’re strong believers that we can create more efficient, user-friendly experiences with Actively Validated Services. If you’re building in intents, join the [BuildOnEigen telegram chat](https://airtable.com/appnYZo360sWuEYLS/shrz6Pstds7EXjC5N), and reach out to our Head of Developer Relations [Nader](https://x.com/dabit3) or the author [Ishaan](https://twitter.com/ishaan0x).

Stay tuned for part 2, where we discuss the future of intents.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/#/portal)

[iframe](https://platform.twitter.com/widgets/widget_iframe.2f70fb173b9000da126c79afe2098f02.html?origin=https%3A%2F%2Fwww.blog.eigenlayer.xyz)# 404

Page not found

[Go to the front page →](https://www.blog.eigenlayer.xyz/)

Subscribe_If you're interested in integrating your rollup with EigenDA, please fill out the_ [_EigenDA questionnaire_](https://bit.ly/eigendaquestions) _!_

EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the [EigenLayer](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/eigenlayer.xyz) restaking primitive. Developed by EigenLabs, EigenDA will be the [first actively validated service (AVS)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/) to launch on EigenLayer. Once launched, **restakers** will be able to delegate stake to **node operators** performing validation tasks for EigenDA in exchange for service payments, and **rollups** will be able to post data to EigenDA in order to access lower transaction costs, higher transaction throughput, and secure composability across the EigenLayer ecosystem, with security and throughput designed to horizontally scale with the amount of restake and operators opted into servicing the protocol.

We intend for EigenDA to contribute the following to the Ethereum ecosystem:

- **An innovative DA solution for rollups that builds toward the Ethereum scaling** [**endgame**](https://vitalik.ca/general/2021/12/06/endgame.html) **, with security derived from, and value contributed back to, Ethereum stakers and validators.** EigenDA is built on some of the core ideas as well as libraries underlying [Danksharding](https://ethereum.org/en/roadmap/danksharding/), which is a crucial upgrade to the Ethereum ecosystem, and can play a role in battle-testing these technologies.
- **A standard for high throughput and low cost that enables the growth of new on-chain use cases.** EigenDA will enable applications across multiplayer gaming, social networks, and video streaming, with a flexible cost model for both variable and fixed fees.
- **A key ingredient for protecting decentralization.** In shared security systems like EigenLayer, if every node operator is required to download and store every chain that uses the system, very few node operators will be able to keep up and the system has the risk of eventually centralizing. EigenDA is built to prevent this centralizing tendency; it achieves high-performance while distributing work across many participating nodes, requiring each operator to do only a small amount of work.
- **A proof point for the power of programmable trust.** EigenDA seeks to demonstrate that Ethereum stakers and validators can support critical Ethereum infrastructure in addition to Ethereum consensus, and that AVSs (like EigenDA) and AVS users (like rollups using EigenDA) can succeed with new business and token models based in modularity on top of the Ethereum trust network.

We are excited that several teams have specified plans to integrate EigenDA into their L2 infrastructure, including: [Celo](https://forum.celo.org/t/clabs-proposal-for-celo-to-transition-to-an-ethereum-l2/6109) as they transition from L1 to Ethereum L2; [Mantle](https://docs.mantle.xyz/network/introduction/concepts/data-availability#mantle-da-powered-by-eigenda-technology) and its suite of complementary products out of the BitDAO ecosystem; [Fluent](http://fluent.xyz/) providing a zkWASM execution layer; [Offshore](http://offshore.space/) providing a Move execution layer; [Layer N](http://layern.com/) providing zk-OP hybrid rollups designed for financial applications; and many more.

We continue to look for foundational partners to build on EigenDA, and invite rollups of all kinds to reach out and learn more about the protocol - especially those that are striving to enable high throughput use casesthat require 15 MBps throughput and higher. If you’re interested, please get in touch via this form: [**EigenDA Questionnaire**](https://bit.ly/eigendaquestions).

## Technical Architecture

The diagram below shows the basic flow of data through EigenDA.

![](https://www.blog.eigenlayer.xyz/content/images/2024/09/pasted-image-0--1-.png)

1. The rollup Sequencer creates a block with transactions, and sends a request to disperse the data blob.
2. The Disperser is responsible for erasure encoding data blobs into chunks, generating a KZG commitment and KZG multi-reveal proofs, and sending the commitment, chunks, and proofs to the operator nodes of the EigenDA network.
3. Rollups will be able to run their own disperser, or use a dispersal service that a third party (for example, EigenLabs) operates for convenience and amortization of signature verification costs. It is possible for a rollup to use a dispersal service optimistically, such that in the case the service is non-responsive or censoring, the rollup can use its own disperser as a backstop, thus getting amortization benefits in the optimistic mode without sacrificing censorship resistance.
4. The EigenDA nodes verify the chunks they receive against the KZG commitment using the multireveal proofs, persist the data, then generate and return a signature back to the Disperser for aggregation.

## Technical Considerations

Now that we have a basic understanding of EigenDA’s architecture, let’s discuss the benefits and properties that the system is designed to achieve. Below is a short list of properties we believe are necessary for a good and useful data availability layer for rollups:

- Economics
- Throughput
- Security
- Customizability

We’ll address each property from the EigenDA perspective.

### Economics

Today, many L2s use Ethereum as their data availability layer due to its crypto-economic security guarantees. This leads to extremely high and volatile costs, as rollups compete with all other Ethereum users for limited blockspace based on congestion pricing. For example, both Arbitrum and Optimism have spent tens of millions of dollars on calldata (data availability) costs on Ethereum year-to-date, with no consistency month-to-month. One of the primary value propositions of DA systems is to dramatically lower these costs and give rollups greater predictability in their cost structure.

**Understanding Cost Reduction**

There are three fundamental dimensions of cost incurred in operating a DA system. Let’s analyze how EigenDA aims to minimize the underlying cost structure on each dimension:

- **The capital cost of staking.** To stake capital in order to secure a DA layer, stakers may want to receive a certain percentage yield in order to offset their opportunity cost. EigenDA reduces the capital cost of staking by using EigenLayer, which employs a shared security model that allows the same stake to be utilized across a variety of applications, creating an economy of scale.
- **Operational cost.** Instead of requiring each node to download and store all data, EigenDA uses erasure coding to split data into smaller chunks, and requires operators to download and store only a single chunk, which is a fraction of the full data blob size. This imposes a lower cost on each operator as compared to storing the full blob, making EigenDA “lightweight” to operate by many nodes. As more nodes join the EigenDA network, the resource costs incurred by every node on the network decreases. This enables EigenDA to be secured by a large set of operators at low and _marginally decreasing_ cost, enabling a philosophy of abundance rather than scarcity.
- **Congestion cost.** As bandwidth utilization in any blockchain approaches the system’s capacity, data starts to get more expensive in order to combat congestion. EigenDA reduces congestion in two ways: 1, by having a higher throughput, it seeks to make congestion a rare phenomenon; 2, by allowing for bandwidth reservation, EigenDA can guarantee rollups pre-reserved throughput at a discounted cost. To maintain flexibility, EigenDA also allows rollups to pay for throughput on-demand.

**Rollup Economics**

Rollup economics differ fundamentally from those of an L1 because DA costs are not only high and unpredictable, but they are incurred _in a non-native token_. This makes it difficult for rollups to make price commitments to users and subsidize initial adoption, because they must take “exchange rate risk” between their own rollup token and the token in which DA fees are paid. In comparison, an L1 pays a fixed amount of inflation and can offer a certain amount of transactions-per-second for free to attract users.

In EigenDA we are exploring mechanisms for rollups to pay stakers in native rollup tokens for a predictable long-term reservation rate, on terms that EigenLayer restakers find acceptable. This couples the inherent scale advantages of a shared security system with the inherent advantage of stable native token payments, in order to help bootstrap usage of rollups.

### Throughput

Throughput is another foundational value proposition of DA systems. EigenDA is designed to achieve _horizontal scaling_ such that the more operators there are on the network, the more throughput the network enables. In private testing with a set of 100 nodes with standard performance characteristics, EigenDA has demonstrated a throughput of up to 10 MBps, and has a roadmap to scale to 1 GBps. This opens the door for bandwidth-intensive applications like multiplayer gaming and video streaming on top of Ethereum.

EigenDA delivers high throughput via three pillars in its design:

- **Decoupling DA and consensus.** Existing DA systems couple the attestation of availability of data blobs with the ordering of data blobs into a “monolithic” architecture. Data attestation is parallelizable as nodes can independently attest to the availability of distinct data blobs; however, ordering requires serialization of the data blobs, thus creating significant consensus lags. While this coupling can be beneficial to security in systems which are designed to be the final source of ordering, it is neither required nor useful in a DA system designed to accompany the Ethereum blockchain, which has its own ordering system that rollups rely on for settlement anyway. By shaving off the unnecessary complexity of ordering and designing a pure DA system, EigenDA improves significantly on throughput and latency.
- **Erasure coding.** EigenDA enables rollups to take the data they want to post to EigenDA, decompose it into smaller chunks, and perform erasure coding on those chunks before storing the data as fragments. Using KZG polynomial commitments (a mathematical scheme at the heart of ZK proofs), EigenDA requires nodes to only download small amounts of data \[O(1/n)\] rather than download entire blobs. Unlike systems that use fraud proofs to detect malicious incorrect coding of data, EigenDA employs validity proofs in the form of KZG commitments, which enables nodes to verify correct coding of the data.
- **Direct Unicast instead of P2P.** Existing DA solutions use a peer-to-peer (P2P) network for transmitting blobs, in which operators receive blobs from their peers, then re-broadcast the same blob to others. This significantly constrains the achievable DA rate. In EigenDA, a _Disperser_ sends blobs directly to EigenDA’s operators. By relying on unicast (direct communication) for dispersing data, EigenDA can confirm DA at native network latency rather than via an expensive gossip protocol. This eliminates the significant gossiping penalties that come with P2P, and results in faster data commitment times.

### Security Features

We use security as an umbrella term that covers safety and liveness, as well as decentralization and censorship resistance. The following features attest to EigenDA’s security:

- **EigenLayer.** By using restaking, EigenDA borrows two distinct aspects of security from the EigenLayer system: 1, economic security; 2, decentralization. EigenDA is built to take advantage of these two distinct elements of trust from the EigenLayer and Ethereum ecosystem in a synergistic manner.
- **Proof of Custody.** A key mode of operator failure in EigenDA is that nodes sign off on data items without actually storing them for the required period of time. To solve this problem EigenDA uses a mechanism called proof-of-custody, which was originally proposed by [Justin Drake](https://ethresear.ch/t/1-bit-aggregation-friendly-custody-bonds/2236) and [Dankrad Feist](https://ethresear.ch/t/a-0-001-bit-proof-of-custody/7409) of the Ethereum Foundation. With proof-of-custody each operator must routinely compute and commit to the value of a function which can only be computed if they have stored all the blob chunks allocated to them over a designated storage period. If they attest to blobs without computing this function, the ETH held by the node can be slashed by anyone who has access to their data item.
- **Dual Quorum.** EigenDA also has a feature called _Dual Quorum_, where two separate quorums can be required to attest to the availability of data. As an example, one quorum would be composed of ETH restakers (the ETH quorum), and the second quorum could be composed of stakers of the rollup’s native token. Both quorums are treated as an independent and redundant source of DA, so both quorums have to be compromised before EigenDA can fail.
- **Censorship Resistance.** EigenDA offers higher instantaneous censorship resistance than coupled DA layers. This is because coupled DA architectures usually rely on a single leader or block proposer to linearly order the data blobs, thus creating an instantaneous censorship chokepoint. In contrast, in EigenDA, rollup nodes can directly disperse and receive signatures from a majority of EigenDA nodes, thus improving the censorship resistance to a majority of EigenDA nodes rather than being constricted by a single leader.

### Security Analysis

As discussed, EigenDA is built on ETH staking via EigenLayer and uses erasure codes with a configurable coding ratio that can be set by rollups. There are three distinct perspectives to analyze the security of a blockchain system like EigenDA; we describe each perspective and how it applies to EigenDA as described above:

**Byzantine fault tolerance (BFT):** Assume some fraction of nodes are honest and follow the protocol exactly, and some are malicious and can deviate from the protocol arbitrarily.

- EigenDA is safe, i.e. data can be retrieved, as long as X% of nodes are honest, where X can be 10% to 50% depending on the coding rate.

**Nash Equilibrium model:** Analyzes the economic incentives of each node or small collusion of nodes to follow the protocol, assuming that nodes across distinct collusions act independently.

- As long as the collusion size is smaller than (1-X), storing and serving data to users is the Nash Equilibrium: storing data is ensured as an equilibrium by proof-of-custody, which slashes the ETH of nodes that do not store the data; serving data is ensured as an equilibrium because data is dispersed across many nodes, inducing a competitive market to serve data.

**Pure cryptoeconomic model:** Assumes all stake is held by _the same node_ and models the cost of economic corruption.

- As long as the data is available, or equivalently, as long as X% of nodes are honest, the ETH staked by any node that doesn’t custody data will be slashed. However, EigenDA does not have unconditional cryptoeconomic safety; if _all_ nodes collude and withhold data, then it may not be possible to slash them. In the Dual Quorum model described earlier, where both ETH and a native rollup token are staked, the rollup can slash the native token even in cases where it is not possible to slash ETH.

As we can see, EigenDA is built on a trust model that requires not only economic trust derived from ETH staking, but also decentralization and independence of operators in order to function safely. Fortunately, EigenLayer allows EigenDA to borrow both of these trust mechanisms from Ethereum.

### Customizability

Rollup developers can implement EigenDA with flexibility to adjust parameters as desired. The modular nature of EigenDA enables rollups to customize safety/liveness tradeoffs, staking token modalities, erasure coding, payment tokens accepted, and more.

As discussed in earlier sections some of the most important flexible decisions in EigenDA are economic. For example, a rollup can choose to use dual quorum staking, in which their own token is staked to guarantee data availability; or a rollup can choose a cost structure that is either on-demand or pre-reserved.

## Strategic Considerations

Finally, (and thanks for sticking with us) we believe EigenDA offers strategic value to rollups outside of the technical properties described above.

- Ethereum stakers and validators are the beating heart that powers EigenLayer, and accordingly, that powers EigenDA. By adopting EigenDA, rollups may align with these Ethereum stakeholders, who expressly value decentralization, censorship-resistance, open access software, and composable, permissionless innovation.
- EigenDA is planned to be the first of many AVSs to launch in the EigenLayer ecosystem. We foresee that as the number of AVSs grows, there will be composability benefits between them, which benefit the end-users of those AVSs, which we expect to include rollups of many kinds. For example, after EigenDA, we expect to see AVSs launch with use cases including sequencing, fast confirmation, watcher networks, bridging, fair ordering, and even artificial intelligence.
- EigenDA is in the early stages of a long journey towards achieving a big vision. The EigenLabs team is seeking **foundational partners** who want to work closely with us on EigenDA, but also for the long haul, across many projects and as long-term collaborators. We hope this represents an opportunity to drive the Ethereum ecosystem towards more open innovation together, and we look forward to supporting your project as much as we possibly can.

## The Path Forward

Consistent with our design philosophy we have a phased roadmap for EigenDA, where various features of EigenDA will be built and shipped incrementally. The first version of EigenDA will go to testnet later this year.

If you’re interested in participating, please get in touch with our team via this form: [**EigenDA Questionnaire**](https://bit.ly/eigendaquestions). We look forward to meeting you!

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/#/portal)

SubscribeIn December, Eigen Labs [announced Slashing on Testnet](https://www.blog.eigenlayer.xyz/introducing-slashing/). The launch of slashing on mainnet is a major milestone for the EigenLayer protocol.

As we continue to progress towards slashing on mainnet, coming April 17th, we want users to be prepared for this upgrade. This guidebook is for AVSs using slashing on mainnet.

## **AVSs and slashing on mainnet:**

Here are a few key aspects of slashing that AVSs should understand as we approach the mainnet launch:

### **Managing Operators with Operator Sets**

AVSs will manage their Operators using a new feature called Operator Sets. The AVS will set the conditions for the OperatorSets and any registration requirements. Operators may have to re-register with Operator Sets, but should communicate with the AVSs they are running (or considering running) to learn their specific requirements and registration flows.

### **Setting Custom Slashing Conditions**

AVSs can set the slashing conditions that Operators comply with to support and run their AVSs. Conditions will be tied to specific Operator Sets, but sit outside of the EigenLayer protocol. AVSs can select these conditions based on their business needs, risk profile, and security requirements, but slashing conditions on Operator Sets are optional and can be added over time. Ultimately, the AVS decides and socializes conditions. Slashing benefits  AVSs that have specific requirements or wish to incentivize certain Operator behavior.

### **Gradual, Opt-in Implementation of Slashing**

Slashings will not begin overnight when the upgrade is live on the EigenLayer protocol. Operators will not automatically be opted into slashing with the AVSs they are currently running.  They will choose whether the AVSs’ conditions are acceptable to them, and they can opt in.

### **Targeted Stake Slashing**

AVSs can only slash the specific portion of stake Operators have designated against that specific AVS. This feature is called [Unique Stake Allocation](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#unique-stake-allocation--deallocation). Operators can designate specific amounts to multiple AVSs if they wish to. This isolates slashing risks and strengthens security guarantees for AVSs without exposing Operators to unnecessary risks from unrelated tasks.

### **Enhanced Cryptoeconomic Guarantees**

Unique Stake is needed for AVSs to flexibly define their own slashing conditions with minimal impact to other ecosystem participants. Previously, many types of EigenLayer commitments lacked a mechanism at scale to provide the trustless guarantees we have with things like EVM execution or direct on-chain value transfer. EigenLayer expands the conditions in which underlying stake can be slashed when used to secure services. EigenLayer slashing expands the functional cryptoeconomic guarantees that AVSs can make to users; it can power entirely new classes of trustless work on-chain and novel services with cryptoeconomic commitments.

## **Why should AVSs be excited about slashing on mainnet?**

This launch provides AVSs flexibility to set the conditions to reward Operators for meeting commitments and punish them if obligations aren’t met. This allows for more transparency in the ecosystem because AVSs can set clear expectations regarding the conditions they need Operators to meet. In turn, Operators can understand AVSs’ expectations, opt into the conditions, and be rewarded for running the services. Also, Operators now have transparency around penalties if they do not meet the conditions set by the AVS. Slashing is a significant step forward in the EigenLayer flywheel because this release marks a feature-complete protocol for AVSs to begin rewarding and enforcing commitments based on the needs of their service.

## **Quick Links for AVSs**

- [AVS Mainnet Launch Instructions](https://eigen-labs.notion.site/Slashing-Update-for-AVSs-Mainnet-Launch-1c913c11c3e0803495dbfa94f256cf89)
- [Slashing Documentation](https://docs.eigenlayer.xyz/developers/Concepts/slashing/slashing-concept-developers)
- [Slashing ELIP](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
- [EigenLayer Release Notes](https://github.com/Layr-Labs/eigenlayer-contracts/releases)
- [Watch the Podcast discussing the impacts of Slashing on the EigenLayer ecosystem here.](https://www.youtube.com/watch?v=xFJkKsyCTms)
- Join the [Discord](https://discord.com/invite/eigenlayer) and [Forum](https://forum.eigenlayer.xyz/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/#/portal)

SubscribeIn December, Eigen Labs [announced Slashing on Testnet](https://www.blog.eigenlayer.xyz/introducing-slashing/). The launch of slashing on mainnet is a major milestone for the EigenLayer protocol.

As we continue to progress towards slashing on mainnet, coming April 17th, we want users to be prepared for this switch.  This guidebook is for Operators using slashing on mainnet.

## **Operators and slashing on mainnet:**

Here are a few key things Operators need to know about the upcoming slashing implementation:

### **Opt-in Requirement for Slashing**

Operators will not automatically be opted into slashing with the AVSs they are running once slashing is launched on mainnet. Operators will be able to choose whether the AVSs’ conditions are acceptable to them, and they can opt in

### **Understanding Operator Sets**

Operator Sets are the way in which AVSs will manage which Operators are running their infrastructure. The AVS will set the conditions for the OperatorSets and any registration requirements. Operators may have to re-register with Operator Sets, but should communicate with the AVSs they are running (or considering running) to learn their specific requirements and registration flows.

### **Managing Unique Stake Allocation**

Operators will now be able to designate a Unique Stake Allocation for each AVS. Operators can allocate specific portions of their staked security as “Unique Stake,” ensuring that only one AVS has the ability to slash that stake. Operators can also designate specific staking amounts to multiple AVSs if they wish to.This isolates slashing risks and strengthens security guarantees for AVSs without exposing Operators to unnecessary risks from unrelated tasks.

### **Preparing for Breaking Changes**

Even though Operators will not be automatically opted in to slashing feature, Operators should be prepared on April 17th for some breaking changes to run EigenLayer on mainnet. Some onchain interaction flows have changed in our interfaces and code paths. These changes partially include updates needed to Custom Errors, Operator Registration, State Introspection, Updating the Metadata URI and Operator Details, Withdrawal Delay, and Queuing Withdrawals. You can find detailed instructions around breaking changes [here](https://eigen-labs.notion.site/Slashing-Update-For-Node-Operators-LRTs-Testnet-Launch-14313c11c3e080be842bc91037ebdd89?pvs=4).

### **Why should Operators be excited about slashing on mainnet?**

This launch provides Operators with real insight into the priorities of the AVSs they are running, and they will be able to align their own risk profile with the AVSs that align with them (and new avenues for AVSs to distribute rewards). Slashing is a major step forward in this flywheel because this release marks a feature-complete protocol for AVSs to begin rewarding and enforcing commitments based on the needs of their service.

_Tom Hay from Infura further explains the real benefits and opportunities for Operators in our Slashing Podcast:_

0:00

/0:59

1×

Operators can learn more by joining the [Discord](https://discord.com/invite/eigenlayer) and [Forum](https://forum.eigenlayer.xyz/). You can also watch our full podcast where we discuss slashing and its impacts on the EigenLayer ecosystem, [here](https://www.youtube.com/watch?v=xFJkKsyCTms).

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/#/portal)

SubscribeIn December, [Eigen Labs announced Slashing on Testnet](https://www.blog.eigenlayer.xyz/introducing-slashing/). The launch of slashing on mainnet is a major milestone for the EigenLayer protocol.

As we continue to progress towards slashing on mainnet, coming April 17th, we want users to be prepared for this switch. This guidebook is for Stakers using slashing on mainnet.

## **Stakers and slashing on mainnet:**

Here's what stakers need to know about the upcoming slashing implementation:

### **Opt-in Requirement for Slashing**

Slashing will only affect Operators and their delegated Stakers if the Operator specifically opts into a slashable Operator Set created by an AVS. This means that Stakers will not automatically be subject to slashing when the feature is released.

### **Allocation Timeline and Safety Delay**

On EigenLayer, Operators opt-in to slashing with an initial allocation to an AVS’s Operator Set, which takes ~17.5 days. This is to give Stakers time to adjust or withdraw funds. After this initial allocation, Operators have control over whether allocations are instant or have a Staker safety delay. Stakers should check our app for their Operator's allocation settings or communicate with them directly. More details on slashable stake allocation [here](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#allocating-and-deallocating-to-operator-sets).

### **Gradual Adoption Process**

We do not expect all AVSs to adopt slashing immediately or all at once. This process will likely unfold over time as AVSs gradually implement it into the protocol.

### **Monitoring Operator Allocations**

Stakers should monitor their Operators’ allocations to AVSs and slashable Operator Sets. As an Operator allocates to new AVSs and their slashable stake changes, this will impact all of the Operator’s delegated Stakers

## **Why should Stakers be excited about slashing on mainnet?**

With the release of slashing on mainnet, the EigenLayer protocol will be feature complete, and AVSs will be able to reward or punish Operators and Stakers based on their service’s needs. Stakers will be able to participate by delegating to Operators that opt in to AVSs that have enabled slashing. Stakers may observe the risk/reward decisions made by Operators in their selection of AVSs and Operator Sets and any corresponding exposure to slashing, and may use this insight to determine the Operator to which they should delegate.

_Matt Paik from LayerZero further explains the benefits of staking and slashing and how risk pricing comes into play. You can watch the full podcast on slashing_ [_here_](https://www.youtube.com/watch?v=xFJkKsyCTms) _._

0:00

/0:56

1×

### **Stay Tuned**

Stakers can learn more by joining the [Discord](https://discord.com/invite/eigenlayer) and [Forum](https://forum.eigenlayer.xyz/). Also, watch our full podcast where we discuss slashing and its impacts on the EigenLayer ecosystem [here](https://www.youtube.com/watch?v=xFJkKsyCTms).

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/#/portal)

SubscribeLast week EigenLayer experienced a major upgrade. The Rewards protocol, the onchain system that enables AVSs to distribute rewards to their stakers and operators, went live on mainnet on August 6th. The Rewards protocol upgrade grants AVSs the ability to begin rewarding stakers and operators for their past, present, and future support. See documentation [here](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/rewards).

In addition to this upgrade, EigenLayer [announced](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/) its initial proposal for programmatic incentives for ETH and EIGEN quorums which will boost AVS rewards with 4% of EIGEN’s total supply over the next year.

Today, EigenDA is announcing the EigenDA Base Rewards Program to take advantage of both of these developments. On Tuesday, August 13th, EigenDA will begin rewarding its stakers and operators with a relatively small amount of ETH each month. The monthly rewards will start at 10 ETH and ramp up over time.

## Why are we launching the Base Rewards Program?

We want EigenDA to qualify for the programmatic incentives program so that EigenDA stakers and operators are compensated for the security and work they contribute to running the network. The way the programmatic incentives program will work is it will distribute a certain amount of EIGEN each month to AVSs' stakers and operators in proportion to the amount of rewards that their AVS paid out of pocket.

## When are we launching this program?

Although the start date for qualifying for programmatic incentives has not been announced, EigenDA will start sending rewards to stakers and operators on Tuesday, August 13th, in order to test the EigenLayer rewards protocol and set a good example for the ecosystem. The initial 10 ETH will be distributed over 28 days, starting backdated on August 7th and continuing through September 4th, 2024. A monthly cadence will continue from there.

## Why start base rewards at 10 ETH per month?

Given the proportional distribution of programmatic incentives, we want to start with a smaller reward to set a manageable benchmark for other AVSs to qualify for similar incentive rewards initially.

## How will the rewards ramp up?

While the monthly rewards will begin at 10 ETH, we plan to gradually increase them until they fully cover the monthly operator expenses for EigenDA, which we estimate to be $160,000 across 400 operators, or $400 per operator. We want this to set a precedent for ecosystem participants, as we believe it’s essential for AVSs to adopt rewards models that at least cover operator costs.

Finally, let's quickly cover the endgame of EigenDA rewards. With EigenDA adoption picking up steam, we expect revenue from projects paying for EigenDA usage to greatly exceed $160,000 monthly. Once revenue starts flowing for EigenDA, most of this will be emitted to EigenDA stakers and operators via this same rewards mechanism.

With the upcoming launches of low-latency dispersal, 20MB/s throughput, permissionless rewards and data availability sampling, EigenDA seeks to establish itself as the premier solution for scaling Ethereum's write capacity. We look forward to building the future together with you.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/#/portal)We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.

**Important Dates to Note**

- [Restaking Window](http://app.eigenlayer.xyz/): Available from Jan 29th, 12 PM PT to Feb 2nd, 12 PM PT.
- New LSTs Arrival: Beginning Jan 29th, 12 PM PT.

**Meet the New LSTs**

- [Frax Ether (sfrxETH)](https://etherscan.io/token/0xac3e018457b222d93114458476f3e3416abbe38f): A decentralized staking derivative from the Frax Finance ecosystem. This unique two-token system ensures all validator income benefits sfrxETH holders, with the token's value gradually increasing due to the protocol's earnings.
- [Mantle Staked Ether (mETH)](https://etherscan.io/token/0xd5f7838f5c461feff7fe49ea5ebaf7728bb0adfa): mETH is the receipt token for Mantle LSP, a non-custodial, permissionless ETH staking protocol on Ethereum L1. Governed by Mantle, it offers an attractive APY of 7.2%, supported by Mantle Treasury's own staked ETH.
- [Liquid Staked ETH (LsETH)](https://etherscan.io/token/0x8c1bed5b9a0928467c9b1341da1d7bd5e10b6549): Representing staked ETH plus network and MEV rewards, LsETH is part of Liquid Collective's decentralized staking protocol. It features enterprise-grade security, automatic reward restaking, and built-in slashing coverage, supported by a wide range of platforms and custodians.

**Restaking Caps**

- Reinstating restaking activities at the current cap of 200k ETH for each LST.
- This unpausing allows for continued and balanced participation across all LSTs.
- Native restaking remains uncapped

**Looking Forward**

- These new additions enrich the EigenLayer ecosystem, providing more diversity and strategic options for ETH stakers.
- Our commitment to security, decentralization, and a robust staking environment remains paramount.
- Submit your LST for future inclusion [here](https://forum.eigenlayer.xyz/c/new-lst-token-on-eigenlayer/21).

Get ready to explore these new avenues in ETH restaking starting January 29th, where each choice you make contributes to a stronger, more resilient decentralized ecosystem!

**Official Links**

- [Website](https://eigenlayer.xyz/) (including new [Ecosystem Page](https://www.eigenlayer.xyz/ecosystem-category-avs?category=AVS%2CRollup%2COperator))
- [Documentation](https://docs.eigenlayer.xyz/overview/readme)
- Blog Posts: [EigenLayer](https://www.blog.eigenlayer.xyz/tag/eigenlayer/), [EigenDA](https://www.blog.eigenlayer.xyz/tag/eigenda/), and [AVS Research](https://www.blog.eigenlayer.xyz/tag/avs-research/)
- [X/Twitter](https://twitter.com/eigenlayer)
- [Discord](https://discord.gg/eigenlayer)
- [Discourse](https://forum.eigenlayer.xyz/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/#/portal)

SubscribeThe Eigen Foundation is excited to announce the upcoming Programmatic Incentives v1 release, a new EigenLayer protocol feature, providing programmatic EIGEN rewards to stakers and operators for their active participation in supporting AVSs.

**What**

Programmatic Incentives v1 will enable weekly programmatic rewards of newly-minted EIGEN tokens to qualifying stakers and the operators they delegate to. These incentives will be retroactive based on staking since August 15, 2024, and will become claimable on a weekly basis starting in October 2024. Details and timeline for claiming are provided below.

In the first year, Programmatic Incentives v1 will distribute ~66,945,866 EIGEN, equivalent to 4% of the EIGEN token’s initial supply of ~1,673,646,668 tokens:

- 3% to ETH and LST stakersand operators (ETH and LSTs are weighted equally)
- 1% to EIGEN stakers and operators

**Distribution**

Programmatic Incentives v1 is initially configured to distribute EIGEN as follows:

- Linearly Distributed: ~1,287,420 EIGEN will be distributed weekly to stakers and operators, beginning August 15, 2024
- Proportional to Stake: EIGEN will be distributed to eligible stakers and operators proportional to delegated stake
- Fixed Operator Commission: 10% of the distributions (~128,742 EIGEN per week) will go to operators that have received delegations from stakers, and the remaining 90% (~1,158,678 EIGEN per week) will go to stakers.  These percentages may change with future updates (more below)

**Qualification**

To receive Programmatic Incentives:

- Operators must be registered to at least one AVS
- Stakers must be delegated to an operator that is registered to at least one AVS

The Programmatic Incentives distribution and eligibility configurations will evolve over time, adapting to the changing needs of the ecosystem to incentivize valuable engagement with protocol features and securing the AVSs built on top.  Future updates that are being explored include:

- [Rewards Boost](https://www.blog.eigenlayer.xyz/p/e95f7b9d-48a3-47fb-895b-180d051f546e/): EIGEN will be distributed weekly to stakers and operators of each AVS proportional to the amount of rewards distributed by each AVS
- [Rewards Floor](https://www.blog.eigenlayer.xyz/p/e95f7b9d-48a3-47fb-895b-180d051f546e/): a small portion of EIGEN programmatic rewards will be allocated to stakers and operators of AVSs that are not yet distributing rewards
- Variable Operator Commissions: commission rates will be flexible, replacing the hardcoded 10% operator commission and enabling the development of an operator marketplace

**Claiming**

Programmatic Rewards employs the same [Rewards infrastructure](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/) that is now available for all AVSs to distribute rewards to stakers and operators, and will accrue and be claimable by stakers and operators in the same place and in the same way as [Rewards from EigenDA](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/) and other AVSs.

Weekly programmatic rewards will be available to claim every Tuesday at 19:00 UTC. Below is the anticipated timeline for rollout of Programmatic Incentives v1 and opening of claims for programmatic rewards:

- Thursday, August 15, 2024: Programmatic rewards began accruing to all qualified stakers & operators as detailed above
- Tuesday, October 1, 2024: Rewards accrued will appear as ‘Lifetime Earned Rewards’, an upcoming addition to the [EigenLayer dApp](https://app.eigenlayer.xyz/), and will continue to be updated daily
- Tuesday, October 8, 2024: Claiming becomes available for qualified stakers and operators

Accrued weekly programmatic rewards will not expire and can be claimed at any time after they accrue, beginning on Tuesday, October 8, 2024:

- Direct Stakers and Operators: will be able to claim on the [EigenLayer dApp](http://app.eigenlayer.xyz/), CLI, or directly via smart contract
- LRT Stakers: the LRT protocol will accrue the weekly programmatic rewards, and LRT users should engage with their respective LRT protocols to understand how rewards will be managed

**Why**

This upcoming release marks a significant milestone for EigenLayer and the broader restaking ecosystem as EigenLayer transitions from retroactive incentives (points and stakedrops) towards continuous liquid rewards via Programmatic Incentives, ultimately creating a market for price discovery of shared security.

1. **Providing predictable rewards**



Credible commitment to distribute rewards to stakers and operators actively participating in providing restaked security.
2. **Subsidizing restaking marketplace**



Base level of rewards for stakers and operators effectively lowers the cost of restaked security for AVSs.
3. **Strengthening Incentive Alignment**



Qualification criteria encourages positive actor participation. Stakers must now delegate and operators must run an AVS to qualify for programmatic rewards.

Upon release of Programmatic Incentives v1 on mainnet, a followup blogpost will be published with more details on technical implementation, claiming considerations, and links to supporting documentation and resources.

* * *

_This post was written by Eigen Foundation. Eigen Foundation is a Cayman Islands foundation company dedicated to supporting the EigenLayer protocol and community. More information about Eigen Foundation can be found_ [_here_](https://eigenfoundation.org/) _._

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/#/portal)

SubscribeWe are excited to introduce **Slashing**, a proposed protocol upgrade detailed in [**ELIP-002: Slashing**](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md), and the next significant step in the evolution of the EigenLayer protocol. This powerful upgrade introduces slashing, a critical tool for AVSs to enforce cryptoeconomic commitments.

This proposed upgrade is the second EigenLayer Improvement Proposal ( [ELIP](https://github.com/eigenfoundation/ELIPs/tree/main)) using the EigenLayer Governance process ( [EigenGov](https://blog.eigenfoundation.org/introducing-eigengov-part-1/)) recently announced by the Eigen Foundation.

Join in the discussion of [ELIP-002: Slashing on the EigenLayer forum!](https://forum.eigenlayer.xyz/t/elip-002-slashing-via-unique-stake-operator-sets/)

As of December, 19, 2024, Slashing is live on Holesky Testnet.

### **What**

The proposed slashing upgrade introduces two novel features that bolster security and accountability in the EigenLayer ecosystem:

1. **Unique Stake Allocation**



Operators can allocate specific portions of their staked security as “Unique Stake,” ensuring that only one AVS has the ability to slash that stake. This isolates slashing risks and strengthens security guarantees for AVSs without exposing Operators to unnecessary risks from unrelated tasks.
2. **Operator Sets**



A new core protocol primitive that gives AVSs the tools to organize Operators into distinct groups, where the AVS may define slashing conditions, assign tasks, and manage slashable security with precision. Operators must opt-in to these sets specifically, considering the AVS designs and rewards in alignment with their risk tolerance and competencies.

Together, these features allow for the introduction of slashing mechanisms that penalize Operators for faults, such as incorrect computation or liveness failures, while maintaining flexibility for AVSs to reward performance and incentivize specific activity.

### **When**

- **Testnet Launch:** Slashing will deploy on Holesky testnet on **December 19th**
- **Mainnet Launch:** Mainnet deployment proposed in **late Q1 2025**

_Dates are subject to change._

### **Why**

The proposed slashing upgrade unlocks new possibilities for the EigenLayer ecosystem:

1. **Enhanced Credible Commitments**



AVSs can now create cryptoeconomic commitments to their users with deterrent penalties for non-performance.

2. **Greater Operator Flexibility**



Operators can allocate and manage their staked security in alignment with their risk appetite and technical competencies. Unique Stake ensures that slashing risks are isolated, empowering Operators to confidently participate in multiple AVSs.

3. **Driving AVS Innovation**



Slashing enables AVSs to pioneer new classes of trustless on-chain services, such as decentralized computation and secure data attestation, by offering cryptoeconomic guarantees to their end users.

4. **Transparency for Stakers**



Stakers can now assess the slashing risks associated with their delegated Operators, gaining insight into AVS commitments, Operator performance, and potential rewards. This transparency empowers informed price discovery, decision-making, and fosters alignment across participants as Staker-provided security is used in Operator Sets.

5. **Sustainability Through Accountability**



By tying rewards and slashing risks to performance, the upgrade incentivizes a balanced ecosystem where honest behavior is incentivized, and dishonesty is deterred.

### **How It Works**

#### **Unique Stake Allocation**

Operators allocate specific proportions of their delegated stake to Operator Sets created by AVSs as “Unique Stake.” Unique Stake guarantees that an Operator’s specific slashable stake can be allocated only to one AVS at a particular time, minimizing systemic risk. Operators maintain full control over how much stake to allocate and to which AVS, balancing risk and reward.

#### **Operator Sets**

AVSs define Operator Sets to segment tasks, security, and slashable conditions. Each set allows AVSs to customize the security composition and reward mechanisms for specific tasks. Operators can opt-in and register for these sets, allocate their stake, and take on slashing risks in exchange for targeted rewards.

#### **Slashing Mechanism**

AVSs gain the ability to slash Operators’ Unique Stake. This ensures AVSs can enforce cryptoeconomic commitments by slashing stake without affecting the Unique Stake of other AVSs or Operators. AVSs can design robust systems around slashings, like governance, fraud proofs, or other management mechanisms that work for their protocols and communities. Slashing events are transparent, with detailed records available on-chain for verification.

### **Impact**

- **For AVSs**



Unlocks the ability to design innovative services with cryptoeconomic guarantees enforced by slashing, driving user trust and adoption.

- **For Operators**



Provides control over slashing risks, enabling broader participation in securing multiple AVSs while managing exposure.

- **For Stakers**



Enhances transparency and trust by surfacing slashing risks and histories, helping stakers make informed delegation decisions when choosing Operators.

**Testnet**

While we’re live during this testnet phase, initiating new withdrawals and new checkpoints will be temporarily paused (you can still do so via the CLI). Additionally, there may be temporary inaccuracies in displayed data as we calibrate updates and monitor system performance. Updates and fixes will begin on Jan 2. We appreciate your patience and understanding as we roll out this critical upgrade.

One additional note: The new Operator Set-directed rewards mechanisms mentioned in the ELIP will not be available at testnet launch. These will follow shortly after to testnet in the new year.

For Holesky Testnet Application Users **(** [_holesky.eigenlayer.xyz_](http://holesky.eigenlayer.xyz/) _)_: On December 19, some functionality on the Holesky testnet application will be temporarily paused following Slashing deployment on testnet as we migrate and begin indexing the new contracts. Deposits will continue to process, but you may notice delays in balance updates, delegation changes, and status visibility in the testnet UI. Once the migration is complete, we’ll begin testing the slashing and rewards mechanisms.

### **Looking Ahead**

This upgrade represents a critical milestone in EigenLayer’s journey to build a scalable, secure, and decentralized infrastructure for trustless applications. By introducing slashing, we are beginning a new era of accountability, innovation, and growth in the EigenLayer ecosystem.

This upgrade marks a major step forward in creating an ecosystem where participants can innovate confidently, knowing their commitments are backed by robust, enforceable security. Get started on Holesky testnet today and prepare for the mainnet launch to be part of this transformative shift!

**Learn More**

- [ELIP-002: Slashing](https://forum.eigenlayer.xyz/t/elip-002-slashing-via-unique-stake-operator-sets/14275)
- [Technical Documentation](https://docs.eigenlayer.xyz/)
- [Slashing Technical Documentation - For Operators](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-sets)
- [Slashing Technical Documentation - For AVS](https://docs.eigenlayer.xyz/developers/avs-opset-slashing#slashing-of-unique-stake)
- [Slashing Testnet Release Notes](https://github.com/Layr-Labs/eigenlayer-contracts/releases)
- [EigenLayer Governance (EigenGov)](https://blog.eigenfoundation.org/introducing-eigengov-part-1/)
- [EigenLayer Protocol Council](https://forum.eigenlayer.xyz/t/introducing-the-protocol-council/)
- [EigenLayer ELIPs](https://github.com/eigenfoundation/ELIPs/tree/main)
- [EigenLayer Forum](https://forum.eigenlayer.xyz/c/protocol-council/)
- Follow along:
  - [Discord](https://discord.com/invite/eigenlayer)
  - [X](https://x.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-slashing/#/portal)

Subscribe**Eigen Labs is excited to introduce the EigenLayer Security Model, an evolution of our previous** [**model**](https://docs.eigenlayer.xyz/assets/files/EigenLayer_WhitePaper-88c47923ca0319870c611decd6e562ad.pdf) **, in preparation for the upcoming ‘Slashing’ protocol feature release.**

This blog post is a simple description of how EigenLayer empowers AVSs to operate more efficiently and create economically aligned incentives using three foundational concepts: Operator Sets, Total Stake, and Unique Stake. In upcoming articles, we will dive deeper into the technical details of the planned protocol updates, uncovering the full potential and advantages of EigenLayer’s innovative design.

For additional context, we have included in the appendix a brief description of what properties this new model is trying to achieve in comparison to the original.1

## **Operator Sets**

An Operator Set is a group of operators who have been delegated ETH by stakers and are assigned certain tasks by an AVS. Although EigenLayer accepts many different types of tokens, and multiple tokens can be used in a single Operator Set, for simplicity, we will proceed using only staked ETH, which we will refer to simply as ETH.

Stakers delegate ETH to operators who participate in various Operator Sets created by AVSs.2 To join Operator Sets, operators send requests to the respective AVSs for inclusion. AVSs can reward operators within an Operator Set if they perform their assigned tasks correctly and impose penalties if they do not. An AVS can create as many different Operator Sets as it wants.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfmW-8hI0PTqXuw07AJAKISztW0cwoI7qbKzsrMMKZZSQ6ZD6B-PQD4vnI4Ey--I0w66_xSSeN8zbZI-edJJZcu6ay3hetf9PAIX9YQ71sBQ4yKwYVxQIVvrxb7YUnnmw6cmWBqnRamEZ8hHhVAhyPnZG-8?key=dMmpCx0NfffmzDNWpHCOKA)

## **Total Stake**

The total amount of ETH delegated to an operator is the Total Stake of that operator. An Operator Set’s Total Stake is the sum of all its operators Total Stake. For example, if there are 3 operators in an Operator Set and each has 100 ETH delegated to them, then the Total Stake for each operator is 100 ETH, and the Total Stake for that Operator Set is 300 ETH.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdSfzbEiyHIxfNqGOb4s0rNj0EwBPdThc3ZdsVWfpcLBPYkvudPvUe9NrJamP1xkMxHhbU4oKn-okGoH-uaiIBaJuaOEi8bI-oTOKOVjhq3K5_WI2Zi9Nv_RcPkvGM5kc4xHJDDQPjISyajdIaloxVWXvYZ?key=dMmpCx0NfffmzDNWpHCOKA)

## **Unique Stake**

Operators allocate a percentage of their ETH that an Operator Set is permitted to slash. An operator’s total allocation across Operator Sets must be between 0% and 100%. In other words, a unit of ETH can only be slashed by a single Operator Set at any given time. The allocated amount of ETH is called the operator's Unique Stake for that Operator Set.

If a task is performed incorrectly, the Operator Set may choose to slash an operator for up to that amount as defined by the AVS. The Unique Stake of an Operator Set is the amount of ETH exclusively slashable by that Operator Set.

Building on our previous example, if an AVS has one Operator Set and the three operators allocate 0%, 10%, and 20% of their ETH to it, their Unique Stake would be 0, 10 and 20 ETH, respectively, totaling 30 ETH in Unique Stake for the Operator Set.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcV9AWvp1tfwR36FmcPMi6AWlMQMqaBp3xItP7xtHv-S3t4Nd0Kok-L17koGpzVlj72AjOxXHXyVbrw4q3bWTmbstxZpfTYTjEnaU6KsVkbzI3YC8SckjPVILIOCBuW70JKtsilyJU9IR1PKKozWwjVfNuQ?key=dMmpCx0NfffmzDNWpHCOKA)

## **Attack Mitigation Using Total and Unique Stake**

AVSs can use the Total Stake and Unique Stake of an Operator Set for security. For example, consider an AVS creates a single Operator Set which has operators perform a task and vote on its output. The output that receives more than 50% of the stake-weighted vote is deemed correct. We assume that all pre-existing operators are honest and cannot be bribed to corrupt the task.

Using the numbers from our previous example, if the Operator Set weighs votes solely by Total Stake, a new malicious operator could manipulate the task’s result (attack) by allocating slightly more than 300 ETH in Total Stake (eg. 301 ETH). Without the ability to slash, no penal action can be taken against this operator besides ejection from the Operator Set.

To use slashing as a punitive disincentive for misbehavior, the Operator Set can instead weigh votes by Unique Stake. In this case, the malicious operator would need to allocate just over 30 ETH in Unique Stake to carry out an attack (e.g. 30.1 ETH). Afterward, the Operator Set can slash the malicious operator for 30.1 ETH. While this imposes a tangible economic penalty, it significantly lowers the capital requirement from 301 ETH to 30.1 ETH.

An Operator Set could also require agreement between votes based on both Total Stake and Unique Stake, effectively balancing their weaknesses and leveraging their strengths. This approach forces a new malicious operator to meet the higher threshold of 301 ETH in Total Stake to compromise the task’s result while committing 30.1 ETH in Unique Stake that can be slashed after the attack. Thus, the Total Stake raises the capital required to corrupt the task, while the Unique Stake ensures an economic penalty for the malicious operator.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc0XEX976SvvEEPMTnsXUdci3pFWZmup_4QXK65DGY7WxFry3-d6bDykmAv5B8NPY_M3tkLW9ZL5FIU8BDX-VFCB6L8PARTp6xwli2aCyiFcacOwI5wdfhJuz0aaCkArMEGmG6Ha4tejv_rGl9NSv5M5hd8?key=dMmpCx0NfffmzDNWpHCOKA)

## **Operator Sets are Versatile and Extensible**

Thus far, we have provided a simplified demonstration of how this security model works. In reality, Operator Sets are far more versatile. For example, an Operator Set can include multiple tasks and tokens, none of which need to be ETH. It can specify slashing conditions for different types of tokens, tasks, and infractions, as well as define confirmation rules for the set of tasks. An Operator Set can determine which tokens will be used to reward operators and how these rewards will be distributed. This model is also extensible to accommodate various preferences for handling slashed stake, including burning or redistribution (see [StakeSure](https://arxiv.org/abs/2401.05797)), although redistribution will not be available in the initial slashing release. Finally, because an Operator Set’s Unique Stake is only slashable by that specific Operator Set, slashing is localized, eliminating the need for a common veto committee.

## **Conclusion**

In future articles we will delve into the nuances of the security model, why it is an improvement over other models, and other components that are actively being researched.

## **Appendix**

1 **What is the new model trying to accomplish?**

The original [security model](https://docs.eigenlayer.xyz/assets/files/EigenLayer_WhitePaper-88c47923ca0319870c611decd6e562ad.pdf) had the following structure: each operator can opt into a subset of AVSs, and the entire stake delegated to that operator could be slashed by any of those AVSs, which meant:

1. **Less control over slashing risk**: An AVS sharing slashable stake could be inadvertently exposed to the risks of slashing by other AVSs. Additionally, the entirety of an operator’s delegated stake is at risk of being slashed by any AVS it opted into.
2. **No slashable stake assurances for AVSs**: AVSs had no assurance that they would have access to slashable stake when needed.
3. **Needs a common slashing veto committee:** The system needed a common veto committee to buffer slashing events from becoming systemic.
4. **Permissioned onboarding of AVSs**: Because of the need for a common veto committee, AVS onboarding needed to be permissioned otherwise the veto committee would be taking on uncalibrated risks.

The new EigenLayer security model described in the blog post above has been specifically designed to address these shortcomings by offering greater flexibility and control:

1. **Greater control over slashing risk**: With Unique Stake, the risk of slashing is isolated to the individual AVS, and operators can control how much of their stake any AVS can slash.
2. **Guaranteed slashable stake**: Each AVS knows how much slashable stake they have by adding up the Unique Stake allocated to them across all its operators.
3. **No need for a common veto committee**: Since slashing is localized to individual AVSs, there is no necessity for a common veto committee. Each AVS can specify its own mechanisms for slashing and governance.
4. **Permissionless onboarding of AVSs**: AVS onboarding is permissionless and only requires operators to opt in.

* * *

2 Stakers can delegate to only one operator per address. For example, as shown in Figure 1, stakers collectively have delegated 100 ETH to each of the three operators. However, behind the scenes, each staker uses three separate addresses, with each one delegating to a specific operator.

Consider Figure 5 below in the context of Figure 1:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2GCO1tQ7ppzXYgVqfZM6TQIPyyw9H1RixPWLceZAnMpnk6EVYwj9IAGBqqJo7lCEVZWN1-1CL4seGEoNF63Zg-OWNeEE_KLPNPO-9JXyDXs3quzr1RiyPjIEWnOU13jjIPGehvYPmNq2M4ZXbMmsm9mQ?key=dMmpCx0NfffmzDNWpHCOKA)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/#/portal)

SubscribeEigen Labs is pleased to announce the EigenPod upgrade, a major update to the EigenLayer protocol, making native ETH restaking on EigenLayer easier and more rewarding.

**What**

The EigenPod upgrade introduces a novel balance checkpointing system for managing Ethereum validator and EigenPod balances.

**When**

All EigenPods on mainnet have been automatically upgraded to the new checkpointing system as of 9:00am PST September 4, 2024,

_Although no action is needed, existing EigenPod owners are encouraged to complete their_ [_first checkpoint_](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/EigenPod.md#checkpointing-validators) _by clicking the 'Restake' button on your existing EigenPod to take advantage of the new features, please see supporting documentation_ [_here_](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/native-restaking/#restake-validator-yield-rewards-fees-and-eth) _._

**Why**

With this upgrade, the streamlined checkpointing process enables any EigenPod owner (or assigned proof submitter) to:

1. **Claim Rewards in Bulk**



Instead of proving every beacon chain partial withdrawal as a separate claim, EigenPod users can checkpoint each validator’s current state with a single proof to claim all yield to date, massively reducing gas costs and allowing users to amortize the cost by accruing rewards and claiming them in bulk.

2. **Earn More Rewards**



EigenPod users can now complete a checkpoint to restake their beacon chain ETH rewards on top of their existing EigenPod balance. Beacon chain rewards are automatically restaked back into EigenLayer, enabling restakers to earn additional rewards.

3. **Utilize Improved UX and Tooling**



EigenPod users now have a simplified user interface on our UI console, a fully integrated and self-service CLI, and clean dependencies for generating proofs for integrated applications. Here is a screenshot of what the new UI console looks like:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfO5OmWViulITCAtUwvGqe_KihLuOCa6lR1jCmG8YbUfhwebjYMm1JqvN9f--_-JfiRhrSt0zjwnwBu7BC7qrEnoSsVLh8sb-8XTrl9bhHfemXcw-sG9Ti8TFyogQv8XboMQc5Fb73xOT5xwfSTL8OSQ042?key=y6zPZQUpBuWO-it3jOooSQ)

**Learn More**

- Details on how our EigenPod system works and on our redesign ( [documentation](https://hackmd.io/U36dE9lnQha3tbf7D0GtKw))
- Audit findings on our smart contracts ( [documentation](https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/audits))
- EigenPod smart contracts ( [documentation](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/EigenPod.md))
- Details on potential gas savings ( [documentation](https://hackmd.io/f5vHPzMRSg-Fam7QeqvuWw))
- Protocol: EigenPod Enhancement upgrade details ( [documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-developer-guide#pepe-release-holesky-testnet))
- EigenPod upgrades and pending consensus rewards ( [documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-developer-guide#eigenpod-upgrades-and-pending-consensus-rewards))
- Common user flows ( [github](https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/docs#common-user-flows))
- EigenPod checkpointing validators ( [github](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/EigenPod.md#checkpointing-validators))
- Follow along:
  - [Discord](https://discord.com/invite/eigenlayer)
  - [X](https://x.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/#/portal)

SubscribeAgents are a powerful new vector for personalizing users' interface with next-generation applications. In combination with crypto rails, agents become verifiable actors, giving their owners transparency and control over how the agents are used to augment the users' experience navigating the web.

The possibilities have captured the imagination of the world’s largest companies and spurred interest in [OpenAI’s Operator](https://openai.com/index/computer-using-agent/), [Anthropic’s Computer Use](https://www.anthropic.com/news/3-5-models-and-computer-use) product, and frameworks like [Eliza](https://elizaos.github.io/eliza/). They have also sparked a new wave of debate about how these agents and the models powering them can be held accountable for their assumptions and inferences.

Unlike those built on web2 rails, onchain agents can be autonomous, verifiable, and censorship-resistant. However, they also need to be practically cheap, something the onchain environment can’t provide even with improvements to underlying L1s and L2s.

AI software implemented as [EigenLayer AVSs](https://avsecosystem.eigenlayer.xyz/) directly and elegantly addresses these limitations. In this post, we explore the following:

- What is an agent?
- What are onchain agents?
- How EigenLayer addresses the core challenge of running onchain agents
- How to start building a “Level 1” agent

## **What’s an Agent?**

An “agent” is a central orchestrator that manages (in a closed loop) the entire operation a user wants performed in some environment. The more capable the agent is of reaching the goal, the more autonomous the agent. It can receive a user’s **goal** or **task** and decide how to proceed by consulting the Orchestrator, Model (LLM), Memory, and Tools.

### **Core Components**

Thus, in its entirety, an agent is a composition of:

- Agent Runtime
  - LLM
  - Tools
  - Orchestrator
  - Memory / State
- Goal(s)
- Environment

Visually, this might look like the following architecture (diagram from Google’s [Agent Whitepaper](https://www.kaggle.com/whitepaper-agents)).

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcXBkFIxeJP4bOwLHo-_ZmcddS69mX553LOxRIl--kb2nWiJuEvxmfGc6ncIDb8B343r5yEtui27x354095QwzJ1BU2xpYo7CL8sS6jA8x-guyCwrRU61TU2Tsk9XIJdeROqHWj5Q?key=k-zVJT_4wjCpDWFV3oX6niRb)

### **How Components Work Together**

Much like a well-orchestrated team, each component of an agent plays a specific role while maintaining continuous communication with other parts to achieve the overall objective.

#### **LLM Interface**

The LLM is the agent's “brain” and is treated as if it has reasoning capabilities. LLMs like Claude, LlaMa, and the GPT series have different training data, and they can be swapped under the hood as better models are released. The agent deployer can, at runtime, specify whether a certain open-source LLM should be used or if it has the option to specify a closed-source model provider.

#### **Tools**

LLMs are self-contained and can only “reason” based on the data the model was trained on. To connect to _live_ data and real-time information, agents rely on tools that can **read and write** information using APIs exposed as plugins or extensions by the agent framework.

#### **Orchestrator**

The orchestrator consults the LLMon how to proceed with the task step by step. This mechanism can be a part of the prompt or a separate module, often a framework (like Eliza) that acts as the “glue” between the rest of the components.

Note that depending on the sophistication of the LLM, a significant part of this orchestration function may be embedded in it. Directionally, this is the approach that Anthropic describes in its [“Building Effective Agents”](https://www.anthropic.com/research/building-effective-agents) blog post, which sometimes involves multiple LLMs per agent.

#### **Memory / State**

This is a mix of short-term memory (the context provided in the current session), the results of outside tool calls, and, optionally, long-term memory to persist information across multiple invocations/sessions.

### **Execution Flow**

Given these components, a high-level flow might be:

1. **Agent** receives a high-level goal or user query.
2. **Agent** consults **Memory** (context) and **LLM** (for analysis or explanation).
3. **Orchestrator** decides the next **Action** (e.g., “call a specific Tool”).
4. **Agent** uses that **Action** to invoke the **Tool**, possibly with parameters.
5. **Tool** provides a result (e.g., an API response), which is stored in **Memory**.
6. **Agent** (and **Orchestrator**) iterate until the goal is completed.
7. Finally, **Agent** presents the outcome to the user.

Below is a sequence diagram of how an agent would use tool calling to serve a user prompt.

![](https://www.blog.eigenlayer.xyz/content/images/2025/02/image-620a1d5a2766fc06.png)Full zoomable diagram [here](https://www.mermaidchart.com/raw/249d7fee-0d4b-4913-ad60-261115ea0a00?theme=light&version=v0.1&format=svg)

### **Key Implementation Considerations**

- **Tool Awareness**: The LLM “knows” about **ToolA**, **ToolB**, etc., because the agent **injects** a description of these tools into the **LLM prompt** or context.
- **Planning vs. Execution**: Some systems/frameworks combine planning and execution in one step (the LLM itself decides). Like the diagram above, others have a separate **Orchestrator** to validate or interpret the LLM’s suggestions.
- **Memory Updates**: Storing intermediate results is crucial for multi-step tasks so the LLM can build on previous actions.
- **Iterative Loop**: The agent can call **multiple** tools in sequence before reaching a final answer, repeating steps 2–9 as needed.

### **Practical Example: Trip Planning**

What would it look like if the agent was tasked with planning a weekend trip? A very simplified flow could look like this:

[![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcc9TgsIcxqqqxhrDNoHJxOxE1Rek_8VxoALTGZT5iRxQJ24-P-dKNgZfvxwUuebLCeErmglx9R5W8HPOJt6D6j_cz1cRc6l84y1XPXodat7mEJZQhzyelkvKuShPykBYhr66Gycg?key=k-zVJT_4wjCpDWFV3oX6niRb)](https://www.mermaidchart.com/raw/b0026093-faa4-4b95-8d42-20ab64f46bc2?theme=light&version=v0.1&format=svg) Full zoomable diagram [here](https://www.mermaidchart.com/raw/b0026093-faa4-4b95-8d42-20ab64f46bc2?theme=light&version=v0.1&format=svg)

The full (zoomable) diagram can be seen [here](https://www.mermaidchart.com/raw/b0026093-faa4-4b95-8d42-20ab64f46bc2?theme=light&version=v0.1&format=svg).

This example demonstrates how agents decompose complex tasks into manageable steps, coordinate multiple tools, and maintain context throughout the process.

## **Onchain Agents: Bringing Verifiability to AI**

Just as we covered agent architecture above, if we were to have the agent take on the properties of an onchain environment, we’d say the agent is verifiable. As such, each component of the architecture would need to be then verifiable, which would give us the following diagram:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXemOiSNd8Xj0yGz77rSGaqIPp6S-8ZllDqAJglCxPoKorQYbyjkLUgGXxbAivNBgor9RvrX8Akhei5xUeqyDjHtOfzeDVxOVL92aYfJuDQ3ICq4AyU4idTg9FTKylVmR1TuBjHdmw?key=k-zVJT_4wjCpDWFV3oX6niRb)

If all the aforementioned components lived onchain, we would have onchain contracts acting as the Agent Runtime.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfRSt3k6yPPK_-rss0cprpESrekrCVXz-_lGhxXxXlLhITf_jDjse67m0V41Okq9aWXQCw0AoYXnTXjrKEQsxmm2ezs2JziFdOwZzyIig5wePfDDwt57YK0qm4pbrYHXiX-ZosoMg?key=k-zVJT_4wjCpDWFV3oX6niRb)

### **What are the main benefits of bringing agents onchain?**

Onchain deployment transforms AI agents from black boxes into verifiable systems with cryptographic guarantees of correctness and transparency.

1. **Verifiability:** The user directly interacts with the agent’s _onchain entry point_, ensuring a verifiable workflow. As agents become more mature economic actors, they may have biases embedded in them to benefit certain stakeholders at the expense of users of the agents. Verifiability as a mechanism is a tool to discern these biases. It helps enforce commitments made by agents and agent developers to enable more user-aligned products ultimately. In particular, this means:
1. **Verifiable Tool Calling**: An onchain record of what tools the agent used to achieve a specific outcome (as well as its communication with other agents)
2. **Memory Availability**: When the agent’s memory is stored in a censorship-resistant manner, it means however the agent’s runtime materializes, it will _always_ have access to the state of the agent
2. **Availability**
1. The **AgentSC** is the entry point that encapsulates the components (Orchestrator, Memory, LLM, Tools) as modular, composable smart contracts. As better orchestrators, LLMs, and tools come to market, the agent developer (and probably soon the agent itself) can swap out whichever components it wants. But the onchain nature of these modules would mean the agent will always have access to that functioning module.

Unlike traditional AI systems where trust is assumed, onchain agents provide cryptographic guarantees of their behavior, making them suitable for high-stakes applications where verification is crucial.

### **What are the technical challenges of onchain agents?**

While onchain agents offer powerful capabilities, they face several critical challenges that developers must understand and address. Let's examine the key limitations and their implications:

1. **Memory Costs:** for long prompts (aka sequences), an LLM’s attention mechanism becomes extremely memory-hungry. The mechanism requires computing a sequence length x sequence length matrix, which grows quadratically with sequence / prompt length and is one of the main bottlenecks of scaling LLM usage past ~100k tokens in a sequence. Doing these memory operations onchain would become infeasible very quickly.
2. **Inference Compute Costs:** Forany practical workload, even with small LLMs, inference simply can’t happen in a smart contract (as shown in LLMSC) as it’s too expensive to run a forward pass of a neural network in the (E)VM. This will particularly become more worrisome as frontier models explore techniques like inference-time computing.

The gas fees for workflows can also quickly add up across Orchestrator calls and tool interactions.

### **What if we move inference offchain?**

The alternative route we explore is having inference happen offchain. This comes with a wide set of trade-offs that we’ll cover in a follow-up post.

As co-processors are categorically the set of offchain services that expand the compute capabilities of onchain contracts, we term an offchain service providing inference in this context as an Inference Co-processor.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe_frYDfpw_SuRbG0C-4KeQjUhGn4jEzUxwqTzSMm2dXUsmK7N7O3P9eYteHEf1bYUTzOA8m9hxK2gF8bj9B_wGtdcKmtZQz3QNkcZs2tZW1m0QZH6oiVGe95Whk8a3F70iPlOcZQ?key=k-zVJT_4wjCpDWFV3oX6niRb)

### **Key Technical Constraints**

Given the memory state access pattern between offchain model execution and the onchain memory read/write patterns, moving inference offchain remains technically and economically impractical. While we moved the LLM to be inside a stateless inference co-processor, keeping the state in a contract and passing the state to the co-processor would incur a large overhead, making it impractical again. This is exacerbated by the remaining components, like tool calling, incurring even more gas costs.

These limitations suggest a more comprehensive solution: moving entire components offchain while maintaining verifiability through EigenLayer's AVS architecture.

## **EigenLayer’s Innovation: The Level 1 Agent**

EigenLayer transforms the onchain agent challenge into an opportunity through its AVS (Autonomous Verifiable Services) architecture. By leveraging Ethereum's cryptoeconomic security in a novel way, EigenLayer enables a new category of verifiable AI systems.

Twitter Embed

[Visit this post on X](https://twitter.com/sreeramkannan/status/1875963448607330500?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

[![](https://pbs.twimg.com/profile_images/1866862776637161472/mVf8f6FK_normal.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

[Sreeram Kannan\\
\\
![](https://pbs.twimg.com/profile_images/1635879999264940033/_pozth32_bigger.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

[@sreeramkannan](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

·

[Follow](https://twitter.com/intent/follow?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F&screen_name=sreeramkannan)

[View on X](https://twitter.com/sreeramkannan/status/1875963448607330500?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

Agents are the ultimate AVS consumers
———————-
Agents should run as Autonomous Verifiable Services (AVS).
———————-
Why?
To get unstoppability, you need the agent to be able to run computation in a permissionless manner.
Since the “agent” is delegating [Show more](https://mobile.twitter.com/sreeramkannan/status/1875963448607330500?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

[Visit this post on X](https://twitter.com/AlanaDLevin/status/1875910203872559473?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es3_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

![](https://pbs.twimg.com/profile_images/1347645078618177538/lgaeveTv_mini.jpg)

Alana Levin

@AlanaDLevin

Agent oracles

If agents start interacting with each other -- especially relying on other agents for information or data feeds (we're already starting to see categories of "info agents" emerge) -- verifying the legitimacy of that info will be important

Who's working on this?

[5:51 PM · Jan 5, 2025](https://twitter.com/sreeramkannan/status/1875963448607330500?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

[X Ads info and privacy](https://help.twitter.com/en/twitter-for-websites-ads-info-and-privacy)

[315](https://twitter.com/intent/like?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F&tweet_id=1875963448607330500) [Reply](https://twitter.com/intent/tweet?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F&in_reply_to=1875963448607330500)

Copy link

[Read 36 replies](https://twitter.com/sreeramkannan/status/1875963448607330500?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1875963448607330500%7Ctwgr%5E7f5def8d5c0cb83d7b8788180341ff0eb36c3a90%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fintroducing-verifiable-agents-on-eigenlayer%2F)

@sreeramkannan on [X](https://x.com/sreeramkannan/status/1875963448607330500)

### **AVSs Unlock the Level 1 Agent**

Developers on EigenLayer can use the cryptoeconomic security of Ethereum to build cloud services (AVSs) to verify any off-chain activity or inference. Just like legos come in different shapes, sizes, and colors to build with, AVSs, too, provide a wide range of verifiable building blocks to app and agent builders.

_The “Level 1 Agent” is a standardized way for agents to integrate with verifiable tools by using AVSs._

![](https://www.blog.eigenlayer.xyz/content/images/2025/02/Table---Light.png)

While this post focuses on Level 1 Agents and how AVSs enable verifiable tool calling, AVSs also enable verifiability across the rest of the AI stack, as non-exhaustively shown above, a topic we’ll dive into in a follow-up post.

#### **Level 1 Agents**

As verifiable agents will need their tool calling to be verifiable, this raises a need for the services or APIs called by these tools to be verifiable. By definition, AVSs are autonomous _verifiable_ services. More concretely, we say:

A service is _verifiable_ because its operators validate its output (safety), given the operators’ incentives to 1) not get slashed and 2) lose access to future revenue.

As such, AVSs can help fulfill the verifiability needs of agent tools. As agent frameworks mature, their plugins can seamlessly integrate with AVSs, so agents deployed with said frameworks are Level 1-compliant. This is similar to Anthropic’s [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction). Just as you can think of MCP like a "USB-C port for AI applications,” you can think of Level 1 Agents as AI applications with a verified USB-C port.

In this post, we’ll introduce how AVSs enable Level 1 behavior for LLM, memory, tool calls, and agent runtime.

#### **LLM**

The inference for the model can happen via a co-processor service. These AVSs could have:

- LLMs running locally on operators’ machines
- A proxy to a closed-source provider (via zkTLS)
- Some kind of consensus
- Prompts routed to a model that’s best optimized for (cost, accuracy, latency)
- Or some other design with a different trust model

Example: There’s an extensive range for the design of **Inference AVSs** (from hardware / TEEs and ZK to crypto-economic, game theory, and model architecture considerations), so we’ll dive deeper into this component in a follow-up post.

#### **Memory**

We covered how onchain memory management would quickly get extremely expensive, a _nd that was only considering the agent's short-term state_. As agents learn to evolve and become more aware of themselves and the rest of the world, where would they store their long-term state? What about keeping the memory private?

The verifiable offchain counterpart? A data availability service such as the [EigenDA](https://www.eigenda.xyz/) AVS offers agents a cheap, fast (15 MBps, and increasing) memory store. It can be used by agents to dump their (encrypted) memory and fetch it from anywhere that the agent needs to respawn to life.

#### **Tool Calls**

So, what else might agents want to do? Or…as underlying models are getting smarter, what might agent developers want or not want their agents to do? We divide this into 2 categories: **_read and write capabilities._**

Let’s dive into just a few capabilities! Note that these capabilities paint a directional picture of how these tools can be modularly composed and that the underlying trust model per capability could vary depending on the implementation, leading to the capabilities themselves being updated per exploration of new trust models.

- **Write onchain**: The capability can affect the agent’s ability to broadcast a transaction.
- **Read onchain**: The capability can offer the agent enhanced ability to consume onchain state.
- **Write offchain**: The capability can allow the agent to update an offchain record.
- **Read offchain**: The capability can allow the agent to query for offchain data.

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| Capability | Write onchain | Read onchain | Write offchain | Read offchain |
| Policies (aka Guardrails) | Yes | Yes | No | Yes |
| Event-driven activations | No | Yes | Yes | Yes |
| Access web2 data | No | No | No | Yes |

### **Policy Enforcement Through AVSs**

Policies can provide input and output filters to agents. This can help guide agents' behavior by reducing the input and output space they operate on.

Operators in a policy AVS are aware of the policies registered with the agent. They enforce them at the input layer by only forwarding approved prompts and at the output layer by approving the agent transactions that abide by the policies. If these policies aren’t enforced, there are grounds for slashing the responsible operators. [An example of a leading policy AVS is Predicate.](https://predicate.io/)

Policies can be very useful in mitigating social engineering attacks. Since the naive interface to agents is natural language, they can be manipulated into reasoning against their own (and their owners’) interests and giving up their assets.

Specifically, some of the policy enforcement behavior could look like:

- Watching for jailbreak attempts in the input prompt (Such as [“many-shot jailbreaking,” as reported by Anthropic](https://www.anthropic.com/research/many-shot-jailbreaking))
- Rate limit spending funds
- Check for sanctioned addresses
- Check for buying verified tokens (as opposed to honeypot tokens)

**Jailbreak Attempt**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfqbwiHiJfoX-BYcMr0nJpSkESeFC3G1wlIJ1mYDhmteZqIfWTxnGJHb326OAbJ_xiLLZ0RB4ylnabnAYwClxrAQiUzp2W7VbZWgOE3PZwvxLZIH8QfpwRiX4XKnvTbiXOTo8oILw?key=k-zVJT_4wjCpDWFV3oX6niRb)

**Agent is persuaded to send funds to sanctioned address**

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcb4B0P2NpihL0AFPoYanpZEP5m-1weukM2EzZaMxYt6A0hJFdR5z_X0aBMaCCDPBBXjcG3klqnWyuYjCVc1RjALacMFPsPULekpLxbnqNRAE-fJFttiVkEDoDnMdjTCr4Bg2LB?key=k-zVJT_4wjCpDWFV3oX6niRb)

#### **Example: The Freysa Experiment**

Take [Freysa](https://www.freysa.ai/act-i) as an example. In this experiment, an agent was strictly forbidden from sending funds anywhere, and the challenge was to persuade the agent to break this constraint.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdVfdz7dzwTHmIJuCy1OyFColTlfeuyrN4Hz90bhKzinRcbNvXB6s-ao5ABi5fiKZCtsmZrSxXa3o9gpBQDHtIjwI0GZff2-UXZZsm8aUuwaHp2Oo2fixBEUCHoxwUYXiNydiPo?key=k-zVJT_4wjCpDWFV3oX6niRb)

After 482 break attempts, Freysa ultimately sent ~$47k to the participant who persuaded Freysa to approve the transfer. A Policy AVS could be used to implement an output policy, which, in Freysa's case, could have been a safeguard that prevented the user from persuading Freysa to transfer the funds.

### **Event-Driven Activations (EDA)**

In an EDA AVS (like [Ava Protocol](https://app.eigenlayer.xyz/avs/0x18343aa10e3d2f3a861e5649627324aead987adf)), agents can register to be pinged about onchain or offchain events by operators of an EDA AVS, who will then watch for said events and be authorized to prompt an agent based on said events.

This allows agents to have very expressive feedback loops instead of just being limited to direct input from users, other agents, or onchain protocols.

Combined with policies, if designed correctly, the agent could be allowed to safely set its own notifications, expanding the agent’s ability to reason about interacting with the external world.

### **Web2 Access**

With zkTLS AVSs such as [Opacity](https://www.opacity.network/) and [Reclaim](https://www.reclaimprotocol.org/), agents can now tap into Web2 data and services and have guarantees that the response indeed came from the expected servers. A very naive example is that while agents are starting to build their own verifiable reputations, being able to refer to the reputations that people have built across apps like Uber and Airbnb could be valuable in helping an agent assess whether it wants to use its resources to serve that user or not.

### **Agent Runtime**

As it becomes easier to deploy agents, it will become important to verify where and how they are running. If they’re running on centralized servers, can they be taken down? What privacy properties does the agent operator uphold?

With the decentralized network of operators in EigenLayer, many of whom run TEEs and GPUs, developers can permissionlessly deploy agents as AVSs.

Example: Projects like [Autonome](https://www.autono.meme/) are quickly enabling this type of workflow.

# **What’s Next: Expanding the Level 1 Agent Vision**

We’re just starting to explore the various functionalities of Level 1 agents and how EigenLayer seamlessly acts as the control plane for verifiable agentic workflows.

We will soon release more posts that cover agents' verifiable needs (oracles, FHE, MPC, and signing, etc.), as well as how EigenLayer’s AVS verifiability extends to the rest of the AI pipeline to give end-to-end verifiability to agents. Stay tuned for more!

For now, you can check out a few of the integrations we’ve open-sourced on the path toward fully end-to-end verifiable agents:

- zkTLS for verifiably accessing web2 APIs (such as OpenAI inference API) via Opacity: [https://github.com/elizaOS/eliza/tree/develop/packages/plugin-opacity](https://github.com/elizaOS/eliza/tree/develop/packages/plugin-opacity)
- Storing zkTLS proofs on EigenDA so anyone can retrieve and verify them locally: [https://github.com/elizaOS/eliza/pull/2926](https://github.com/elizaOS/eliza/pull/2926)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/#/portal)

Subscribe

Twitter Widget Iframe![](https://www.blog.eigenlayer.xyz/content/images/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM-2.png)

We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.

In June we launched [Stage 1](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/), introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has been restaked on the EigenLayer protocol. Caps on restaking of Liquid Staking Tokens (LSTs) remain in place, though we plan to add [several new supported LSTs](https://twitter.com/eigenlayer/status/1722618501519876390) over the coming months, as well as continue to raise and eventually lift the caps.

Stage 2 is an exciting evolution of the protocol. For the first time, **Operators** can register to the network and begin validating for the first AVS, **EigenDA**; **Restakers** can delegate their stake to **Operators** and start putting shared security to work with **EigenDA**; and **Rollups** can integrate with **EigenDA** to begin experimenting with cost-effective, hyperscale throughput use cases.

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/EigenDA-1.png)Restake ETH > delegate to operators > operators validate EigenDA -> provides DA to rollups

Please note that in this testnet, our primary objectives are to test the protocol and its security and continue to iterate and improve its functionality. We’re eager for your feedback – your participation will enable EigenLayer to make progress rapidly towards mainnet.

Below, we first describe how to get involved with the **EigenLayer** **testnet** as a restaker, operator, or AVS developer. Second, we describe how to get involved with the **EigenDA** **testnet** as an operator or rollup developer.

## I. Using the EigenLayer Testnet

Below are the different components of the EigenLayer testnet for restakers, operators, and AVS developers.

### Restakers: Restake & Delegate to an Operator

As a restaker, you can first restake Native Goerli ETH or LSTs, then directly delegate to the operator of your choice. Operators choose which AVSs to support; when you choose an operator, you are also choosing the AVSs you’d like to support. For this testnet, EigenDA is the only AVS option available. You can easily undelegate your stake from any operator, and redelegate it on demand.

- Use the testnet application: [goerli.eigenlayer.xyz](https://goerli.eigenlayer.xyz/)
- Read the [restaking and delegation documentation](https://docs.eigenlayer.xyz/restaking-guides/restaking-user-guide/stage-2-testnet)

![](https://www.blog.eigenlayer.xyz/content/images/2023/11/image--6-.png)View operator profiles from the [operator dashboard](https://goerli.eigenlayer.xyz/operator) and restake to them directly.

### Operators: Register to EigenLayer

As an operator, you can now register on the EigenLayer network via the [Operator CLI](https://github.com/NethermindEth/eigenlayer). Operator registration is permissionless, and we look forward to welcoming all kinds of participants, from solo stakers to large institutions.

- Read the [EigenLayer operator documentation and guides](https://docs.eigenlayer.xyz/operator-guides/operator-introduction)

[![](https://www.blog.eigenlayer.xyz/content/images/2023/11/image--5-.png)](https://goerli.eigenlayer.xyz/operator) Node operators registered to the EigenLayer testnet.

### AVS Developers: Explore Live AVS Code

As an AVS developer, you can explore the first testnet example of an AVS, EigenDA. In addition, we built a demo AVS called Incredible Squaring, which includes sample onchain contracts and offchain node software. Using these examples, developers can learn how to build their own AVSs.

- EigenDA [AVS Packaging](https://github.com/Layr-Labs/eigenda-operator-setup) and [AVS Source Code](https://github.com/Layr-Labs/eigenda)
- [Incredible Squaring repo](https://github.com/Layr-Labs/incredible-squaring-avs)
- Read the [AVS documentation and guides](https://docs.eigenlayer.xyz/avs-guides/avs-developer-guide)
- Read the latest [AVS research](https://www.blog.eigenlayer.xyz/tag/avs-research/)
- If you have an AVS idea, contact the EigenLabs Research Team via [this form](https://bit.ly/avsquestions)

## II. Using the EigenDA Testnet

EigenDA is the first AVS to launch on EigenLayer.

EigenDA empowers rollups with cost-efficient, hyperscale-throughput data availability, benefiting from shared cryptoeconomic security provided by EigenLayer restakers. To read more about EigenDA and the technologies it employs, such as erasure coding, direct unicast distribution, and dual quorum staking, please see the comprehensive [Intro to EigenDA](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/) blog post.

For this testnet, we’re adopting a measured rollout of EigenDA’s full feature set and functionality. EigenDA will launch with an initial throughput target of 1Mbps facilitated by 30 operators, and incrementally scale to a target throughput of 10MiB/s with many more operators participating.

[![](https://www.blog.eigenlayer.xyz/content/images/2023/11/Screen-Shot-2023-11-16-at-1.49.23-PM.png)](https://goerli.eigenlayer.xyz/avs/eigenda) EigenDA's AVS summary shows restake distribution and participating operators.

### Operators: Validate for EigenDA

As an operator, once you’ve registered to EigenLayer via the [Operator CLI](https://github.com/NethermindEth/eigenlayer) (see the _“Using the EigenLayer Testnet”_ section above), you can also opt in to validating for EigenDA.

Any operator can permissionlessly opt in to validating for EigenDA. For now, only the top 30 operators by delegated stake can actively participate in validating for EigenDA. This limit on the number of active operators will be raised progressively as we test the system.

- Read the [EigenDA operator documentation and guides](https://docs.eigenlayer.xyz/operator-guides/avs-installation-and-registration/eigenda-operator-guide)
- Check out the [EigenDA blob explorer](https://blobs-goerli.eigenda.xyz/)

[![](https://www.blog.eigenlayer.xyz/content/images/2023/11/Screen-Shot-2023-11-16-at-3.42.39-PM.png)](https://blobs-goerli.eigenda.xyz/) EigenDA Goerli blob explorer.

### Rollup Developers: Integrate EigenDA

As a rollup developer, you can now integrate EigenDA as your data availability layer. EigenDA is intended to enable hyperscale throughput use cases such as high-frequency trading, social networks, gaming, and more.

For this testnet launch, we’re inviting interested rollups of all kinds to come collaborate with our team and test the system’s fit for your use case. Potential benefits of working with us now include strategic partnerships, co-marketing opportunities, and joint research and development.

We look forward to hearing from you and learning about your rollup.

- Read the [EigenDA developer documentation and guides](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-overview)
- To learn more about EigenDA, read this [intro blog post](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)
- Get in touch with our EigenDA team via this [contact form](https://contact.eigenda.xyz/)

## III. Technical Support

If you're looking for technical support as a restaker, operator, AVS developer, or rollup developer, we have the following support channels available:

- Visit [#m2-testnet-support](https://discord.gg/g7k3JZzj2y) in the EigenLayer Discord
- Fill out this [support form](https://share.hsforms.com/1GsJ-rDqmRKeSRlnfOaCa8gein6l)

## IV. The Path to Mainnet and Beyond

Our goal with this Stage 2 testnet is to learn, iterate, and improve the user experience for restakers, operators, AVS developers, and rollup developers. We have twin North Stars: innovation and security. With those as our guide, we expect the Stage 2 mainnet to launch in the first half of 2024.

Stage 3 will introduce AVSs beyond EigenDA. We expect Stage 3 to go to testnet and mainnet in 2024.

We’re proud of what this Stage 2 testnet represents - the culmination of an immense amount of research and hard work - yet recognize fully that we are still very early on the journey to realizing the full EigenLayer vision. We’re excited to keep building out a universe with open innovation at its core, powered by Ethereum programmable trust.

This vision can only be realized with your support, interest, and engagement. To our Discord members, Forum posters, X/Twitter followers, Telegram groups, summit attendees, online supporters, IRL friends, and many more, and beyond - thank you for being an integral part of the EigenLayer story.

### Official Links

- [Website](https://eigenlayer.xyz/) (including new [Ecosystem Page](https://www.eigenlayer.xyz/ecosystem-category-avs?category=AVS%2CRollup%2COperator))
- [Documentation](https://docs.eigenlayer.xyz/overview/readme)
- Blog Posts: [EigenLayer](https://www.blog.eigenlayer.xyz/tag/eigenlayer/), [EigenDA](https://www.blog.eigenlayer.xyz/tag/eigenda/), and [AVS Research](https://www.blog.eigenlayer.xyz/tag/avs-research/)
- [X/Twitter](https://twitter.com/eigenlayer)
- [Discord](https://discord.gg/eigenlayer)
- [Discourse](https://forum.eigenlayer.xyz/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/#/portal)

SubscribeIn recent months, liquid restaking protocols and liquid restaking tokens (LRTs) have seen tremendous growth, and are beginning to become systemically important in the EigenLayer ecosystem. There are many different flavors of LRT, each with different values, features, and offerings. All of these LRT projects are independent of the underlying EigenLayer protocol, and are built by teams unaffiliated with Eigen Labs.

Our core value is open innovation, so we are excited to see a host of projects innovating on and around EigenLayer. At the same time, LRTs introduce additional complexity, and therefore risk, to restaking. One particular bad outcome we seek to avoid is an arms race of opaque risk taking, so that if one project engages in it, other projects are forced to do the same or lose market share. This blog post seeks to shed light on some of the potentially more severe or imminent risks in the LRT ecosystem, and spur the community to analyze and consider them more deeply.

We speak often about _maximizing open innovation_ \- but equally important is our unwavering commitment to education and transparency so that users can make fully informed decisions about the ways they participate in the ecosystem.

## Risks of Liquid Restaking Protocols

Liquid restaking protocols offer benefits that may be attractive to users, but also add complexity on top of restaking. This complexity creates additional risk, which varies by LRT depending on implementation and usage. This [blog post](https://ethresear.ch/t/the-risks-of-lrts/18799) discusses some of these risks. These risks are both technical and governance related, as different systems employ different types of technology and governance. Users should consider both angles on the sustainability of any particular liquid restaking protocol.

Enumerating and quantifying all relevant risks is challenging, and users are urged to  make their own risk assessments before engaging with these systems. Here, we want to highlight the existence of two specific risks:

### 1) Financialization of LRTs

Recently, it has become possible to engage with some of the LRTs in a recursive borrowing strategy, colloquially called “looping”. This strategy clearly results in leverage, i.e., small downward fluctuations in market price of these LRTs can lead to amplified losses.  Like leverage in other asset classes, these trading strategies can involve very significant risks, including volatility and deleveraging risks that could result in the loss of some or all of the assets.

In certain market conditions or if there are technical or implementation flaws, certain LRTs could trade at prices much lower than Ether, resulting in significant losses exacerbated by the looping mechanism. We urge users to understand the various risks and proceed with caution before engaging with LRTs and any looping or leverage.

### 2) Lack of Liquidity

EigenLayer contracts are designed with the functionality for restakers to withdraw from the system with a known one-week delay. However, per our understanding, some liquid restaking protocols do not yet have functionality for unstaking, and in some cases, the policies for when and how such functionality will be implemented and empower users to withdraw funds is unclear.

As a result, users who want to withdraw their funds may be forced to sell their LRTs on thinly traded markets with low liquidity. If the outflows are significant enough, this could result in the LRT being priced lower than ETH. As noted above, given the interdependence of some of these LRTs in DeFi and the nascency of these markets, that could have significant knock-on effects and losses for LRT holders. Again, these risks are significantly increased when involving looping or leverage.

## A Bright Future

We appreciate the community's engagement with EigenLayer and are excited by the permissionless development that is flourishing in and around the EigenLayer ecosystem. We're hopeful and confident that the LRT ecosystem will continue to mature, introducing both new features and functionality that greatly benefit users, and mitigating risks that we observe today.

Along the way, we urge – and always will do so – users to proceed with caution, and developers to design and launch protocols responsibly. When users win, we all win. When users lose, we all lose.

We're excited to keep exploring the new and exciting frontier of restaking together!

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/liquid-restaking/#/portal)

Subscribe![](https://www.blog.eigenlayer.xyz/content/images/2024/04/lightbox_launch_0409-5.jpeg)

## The Start of Infinite Sum Games

Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on EigenLayer. In the past few weeks, 70% of all new Ethereum validators restaked to EigenLayer. The LRT ecosystem is booming. The EigenLayer and EigenDA Goerli and Holesky testnets have been successful, with hundreds of active operators and dozens of onboarded rollups. We’ve heard you loud and clear: you want EigenLayer!

Today, we deliver. With the help of the entire community – restakers, operators, AVSs, and rollups – we are proud to launch EigenLayer and EigenDA on mainnet.

**Visit the upgraded** [**EigenLayer App**](https://app.eigenlayer.xyz/) **and** [**Documentation**](https://docs.eigenlayer.xyz/) **, and help us continue to supercharge open innovation on Ethereum.**

New functionality as of today:

- Restakers can delegate their entire stake to an Operator of their choice.
- Operators can register with EigenLayer and opt in to run AVSs.
- AVSs can register with the protocol, starting with EigenDA (currently in beta).
  - AVSs are actively validated services - middleware, services, chains, networks, and PoS systems - secured by restakers and run by operators.

Until now, restaking in EigenLayer meant simply depositing LSTs, or setting validator withdrawal credentials to an EigenPod. Now that operators and an AVS (EigenDA) are live, being an active restaker means delegating to one of the top 200 operators running AVSs.

Over the next few weeks, we expect high operator turnover in the top 200 active set as new operators, delegations, and AVSs come online. Please make an informed decision when delegating. You can always redelegate to a different operator later, but will have to [wait seven days](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/restaker-delegation/undelegate-from-an-operator-and-initiate-withdrawal) to complete the redelegation.

Importantly, this mainnet launch does not include: (1) in-protocol payments from AVSs to operators; and (2) slashing. We are allowing the EigenLayer marketplace to develop and stabilize before introducing in-protocol payments and slashing to mainnet later this year.

### **Details for Restakers**

In order to restake to an AVS (e.g. EigenDA), you must delegate your entire stake to an operator that has opted into that AVS. If an operator opts into multiple AVSs, the stake delegated to the operator secures all of the AVSs. While delegation is non-custodial (operators do not hold your stake), the EigenLayer protocol does not guarantee the suitability of any operator to run AVS software properly.

In the short-term, EigenLayer does not present the risk of slashing while operators establish themselves in the ecosystem and harden their processes. Eventually operators will be subject to slashing if they do not run AVSs correctly.

The LRT DAO would manage your delegation if you restaked in EigenLayer via an LRT. Each LRT is different, and at a different stage of readiness to manage restaker delegations. Since none of the LRTs are created by or affiliated with Eigen Labs, please contact each LRT for their plans regarding restaker delegations.

Learn more via the [delegation documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/restaker-delegation/).

### **Details for Operators**

New operators can run AVSs in two steps:

1. Register as a node operator on EigenLayer: [Node Operator Installation](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-installation)
2. Opt in to run each AVS, starting with EigenDA: [EigenDA Operator Mainnet Installation](https://docs.eigenlayer.xyz/eigenda/operator-guides/networks/mainnet)

EigenDA's active operator set at any given time comprises the largest 200 operators (by delegated stake) that have attempted to register. This operator set will expand over time; we've limited it to 200 for now due to a gas cost constraint on EigenDA's signature verification capability and to ensure a smooth and secure protocol launch. We're actively working on solutions that allow us to expand our operator set greatly. To learn more about how the active operator set is managed, read about the [EigenDA Churn Approver](https://docs.eigenlayer.xyz/eigenda/operator-guides/overview#eigenda-churn-approver).

### **Details for Rollups Using EigenDA**

Onboarding rollups to EigenDA on the mainnet will be available after the operator set and restaker delegations stabilize. In the meantime, please continue to test on [Holesky](https://docs.eigenlayer.xyz/eigenda/networks/holesky). We will announce when rollups can be onboarded to EigenDA on mainnet.

Learn more via the [EigenDA documentation](https://docs.eigenlayer.xyz/eigenda/overview).

### **Details for AVSs**

There are 10+ AVSs actively testing on Holesky testnet, and we will announce more EigenLayer AVSs on mainnet soon. If you’re interested in launching as an AVS on EigenLayer, please contact via the [AVS Intake Form](https://bit.ly/avsquestions).

Learn more via the [AVS Developer documentation](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-developer-guide).

## EigenDA: The Endgame for Data Availability

We often refer to EigenDA as “the first AVS on EigenLayer,” but it is much more than that. We are building towards a future in which EigenDA is the most performant, scalable, and cost-effective solution to the crypto industry’s bottleneck problems of data availability, throughput, and storage. It is intended to be the endgame for data availability, and the key to eliminating Ethereum blockspace constraints.

EigenDA is also integral to the long-term EigenLayer vision: in order to have the ability to penalize bad behavior, the protocol must have access to the data associated with related transactions.

Benefits for rollups include:

- Hyper scalability: Future-proof by design, to enable a world in which there are 1,000x+ more transactions on rollups.
- Cost flexibility: Pricing that allows for on-demand and reserved bandwidth options, as well as native token payments.
- Ease of integration: Integrations with [OP Stack](https://docs.eigenlayer.xyz/eigenda/rollup-guides/op-stack/) and [Arbitrum Orbit](https://docs.eigenlayer.xyz/eigenda/rollup-guides/orbit/) are readily available for deployment via RAASs such as AltLayer, Conduit, Caldera, and Gelato. Integrations for other stacks, such as Polygon CDK and Starkware, are in the works.
- (Future) Native token staking: Rollups can customize EigenDA to use their native token in a separate quorum to verify data availability.

We believe that there are as many possibilities for rollups as there are internet applications. By ushering in an era of cost-effective rollup scalability, we hope EigenDA plays a role in enabling a new universe of previously impossible services and applications. These might include the following:

- Completely onchain orderbooks
- Real-time gaming
- Social network transactions
- AI coprocessors storing outputs in DA
- Atomic data swaps, where DA serves as escrow node
- … and much more!

We are thrilled to be already working with a number of rollups planning to deploy on EigenDA. Over the coming weeks and months, we look forward to highlighting these flagship partners to the EigenLayer and EigenDA communities.

With EigenDA’s unprecedented performance, we encourage you to rethink what’s possible when it comes to building application-specific rollups. The paradigm shifts, starting today. If you’d like to deploy your rollup with EigenDA, please contact the team via the [EigenDA Contact Form](https://contact.eigenda.xyz/).

To get started on testnet, [read the EigenDA documentation](https://docs.eigenlayer.xyz/eigenda/overview).

## EigenLayer is Open Innovation

The mission of EigenLayer is to build coordination engines that maximally empower open innovation. This mission has a beginning, but no end. We consider all of us players in an infinite sum, infinite horizon game, empowered by Ethereum.

This launch is an important step forward for EigenLayer, but there is much further to go: restaking, operators, and EigenDA are live on mainnet; in-protocol payments and slashing are in development; and many more AVSs are launching soon, alongside other major announcements. We’re absolutely thrilled to continue on this journey with your support, and look forward to building the future together for many years to come.

Please find official EigenLayer and EigenDA resources below:

- [EigenLayer App](https://app.eigenlayer.xyz/) (app.eigenlayer.xyz)
- [Documentation](https://docs.eigenlayer.xyz/) (docs.eigenlayer.xyz)
- [Discord](https://discord.com/invite/eigenlayer) (discord.com/invite/eigenlayer)
- [Immunefi Bug Bounties](https://immunefi.com/bounty/eigenlayer/) (immunefi.com/bounty/eigenlayer)
- X: [@eigenlayer](https://twitter.com/eigenlayer), [@eigen\_da](https://twitter.com/eigen_da)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/#/portal)Following the EigenDA [mainnet launch announcement](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/) on April 9, we are pleased to announce that **rollups can now onboard onto EigenDA mainnet for test traffic**. We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable the next generation of blockchain applications. Today, we are onboarding the first batch of EigenDA rollup partners to usher in the next era of scalable and cost-effective data availability.

**This is the final stage of testing and hardening before EigenDA onboards production traffic for any rollup.** Over the coming days and weeks we will be sharing more about the first set of EigenDA rollup partners, and our plans for general availability.

**Upon general availability, any rollup may use EigenDA with throughput similar to today's in-production L2s for free.** This is meant to showcase the advantages of the EigenDA product and be competitive with the EIP-4844 Dencun upgrade. For performance beyond these rates, and to reserve bandwidth in advance, please get in touch with the team.

If you’d like to deploy your rollup with EigenDA, please contact the team via the [EigenDA Contact Form](https://forms.gle/Fn3Y2zGjuBrM3kfCA). As always, a good place to get started is with the [EigenDA documentation](https://docs.eigenlayer.xyz/eigenda/rollup-guides/tutorial) and the [Holesky](https://docs.eigenlayer.xyz/eigenda/networks/holesky) testnet.

Onboarding the first rollups onto EigenDA mainnet is the next phase of a multi-stage launch towards the endgame for data availability for the Ethereum ecosystem. Read more at our [mainnet launch announcement](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/) about new use cases enabled by EigenDA, and other benefits such as hyperscalability, cost flexibility, ease of integration, and native token staking.

Having rollups run on EigenDA mainnet also engages restakers delegating ETH as well as operators running AVSs. Today, **EigenDA has 115 operators and over 42,000 stakers delegating over 400,000 ETH** to secure the network. Not only does this introduce the first alt-DA solution running in production secured by restaked ETH, it also holistically engages many pieces of the EigenLayer cryptoeconomic ecosystem to form a critical piece of infrastructure for the next era of services and applications.

We are delighted to announce this next step forward and have much more to come! Customers interested in testing or launching on mainnet, please fill out our [EigenDA Contact Form](https://forms.gle/Fn3Y2zGjuBrM3kfCA).

Please find official EigenLayer and EigenDA resources below:

- [EigenLayer App](https://app.eigenlayer.xyz/) (app.eigenlayer.xyz)
- [EigenDA Documentation](https://docs.eigenlayer.xyz/eigenda/overview)
- [EigenLayer Documentation](https://docs.eigenlayer.xyz/) (docs.eigenlayer.xyz)
- [EigenDA Github](https://github.com/Layr-Labs/eigenda/)
- [Discord](https://discord.com/invite/eigenlayer) (discord.gg/eigenlayer)
- X: [@eigenlayer](https://twitter.com/eigenlayer), [@eigen\_da](https://twitter.com/eigen_da)
- [Immunefi Bug Bounties](https://immunefi.com/bounty/eigenlayer/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/#/portal)![EigenLayer Blog](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/04/EigenLayer_Ghost_Background_01-2.png)

Research, Updates, and Announcements

[![Community Update: Airdrops and the EigenLayer Ecosystem Network](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/communityupdate.png)](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[CoinDesk published an article on airdrops in our ecosystem and Eigen Labs employee participation in those airdrops. We’ve written a community update on what happened, why it happened, and steps we’ve taken. \\
\\
We want to make clear that we have no knowledge or evidence of any employee at](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[![Coming Soon: Permissionless Token Support on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/comingsoonpermissionless.png)](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[Eigen Labs is proud to introduce Permissionless Token Support, an upcoming update, to the EigenLayer protocol. This feature will enable any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[![Introducing EigenDA Base Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Base-Rewards-5.png)](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[Last week EigenLayer experienced a major upgrade. The Rewards protocol, the onchain system that enables AVSs to distribute rewards to their stakers and operators, went live on mainnet on August 6th. The Rewards protocol upgrade grants AVSs the ability to begin rewarding stakers and operators for their past, present, and](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[![Principles and Best Practices to Design Solidity Events in Ethereum and EVM](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/mainheader.png)](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[Solidity events are a crucial feature of Ethereum and EVM blockchains, with a vast number of use cases within the ecosystem. Primary use cases, for example, include but not limited to\\
\\
\\* Logging: where events provide a mechanism to log critical actions and state changes inside smart contract, for track contract](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[![Coming Soon: AVS Rewards and EIGEN Programmatic Incentives](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/b-1.png)](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[Summary\\
\\
\\* Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.\\
\\* At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.\\
\\* These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[![Request for AVS: Uniswap v4 Hooks](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/1200x630.png)](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[Occasionally, you hear an idea so exciting that you see the future with utter clarity. At Eigen Labs, we’re excited to work with exceptional builders and hear their special insights about the world on a daily basis. We want to share some of ours with you. We’re starting](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[![Holesky Launch: AVSs Can Now Test Restaker and Operator Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/rewards-claim5-630bdf4627a2efba655108a1f5648581.png)](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[We're proud to announce the launch of the MVP experience for Rewards on the Holesky testnet. This MVP allows AVSs to begin integrating with the rewards system, and test setting up incentives for restakers and node operators. It also allows restakers to test claiming functionality for (worthless) testnet](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[![EigenDA Dual Quorum and Production Traffic Announcement](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/Screenshot-2024-05-20-at-12.53.32-PM.png)](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[We are supporting rollup production traffic on EigenDA mainnet available immediately. Anyone can now deploy their rollup and be whitelisted on our free tier. Interested rollup customers should fill out the EigenDA Contact Form in order to be approved for the initial free-tier usage phase. Please provide the ETH address](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[![Onboarding Rollups to EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/eigenda-Blog-post---1.png)](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[Following the EigenDA mainnet launch announcement on April 9, we are pleased to announce that rollups can now onboard onto EigenDA mainnet for test traffic. We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[![EIGEN: The Universal Intersubjective Work Token](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Exploring-EIGEN-Banner.png)](https://www.blog.eigenlayer.xyz/eigen/)

[Over the past few years, we at Eigen Labs have been developing a platform for advancing the concept of open, verifiable digital commons. This blog post summarizes the intersubjective forking protocol enabled by the EIGEN token. We will break down the significance of EIGEN, its core ideas, its high-level implementation,](https://www.blog.eigenlayer.xyz/eigen/)

[![Accelerate Rollup Deployment with EigenDA's RaaS Marketplace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/b2--1-.png)](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[Teams are launching high performance, low cost rollups and rollup services on EigenDA. In this post we’ll take a look at some of them and learn how you can get started building your own rollup.\\
\\
Rollups as a Service (RaaS) provide everything you need to build, customize, and deploy](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[![Unpausing Restaking Deposits on April 16th!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Ghost_05.png)](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[We're excited to announce the culmination of our phased rollout with the removal of all Liquid Staking Token (LST) caps and unpausing deposits, effective April 16th, 2024, at 9:00 AM PST. This milestone marks the next chapter in EigenLayer's journey, fostering a wide-open and dynamic](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[![EigenLayer AVS Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/medium-banner_option4.png)](https://www.blog.eigenlayer.xyz/avs-launch/)

[EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs\\
\\
The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s MACH restaked rollups with Xterio being](https://www.blog.eigenlayer.xyz/avs-launch/)

[![Mainnet Launch Announcement: EigenLayer ∞ EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/lightbox_launch_0409-3.jpeg)](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[The Start of Infinite Sum Games\\
\\
Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[![Fhenix: FHE Coprocessor on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/image1.png)](https://www.blog.eigenlayer.xyz/fhenix/)

[Fhenix and EigenLayer Join Forces to Pioneer FHE Coprocessors, Revolutionizing Onchain Confidentiality on Ethereum\\
\\
We are excited that FHE Coprocessor will be building on EigenLayer and to announce the development of FHE-based coprocessors in collaboration with Fhenix.\\
\\
FHE coprocessors are secured by Fhenix’s optimistic FHE rollup infrastructure and EigenLayer’](https://www.blog.eigenlayer.xyz/fhenix/)

[![EigenLayer Holesky Testnet Launch + Dual Quorum Support for EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/Twitter---22.png)](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[The EigenLayer and EigenDA Holesky Testnet is now up and running. This marks an important milestone for the EigenLayer ecosystem, and we're keen for operators, stakers and rollup developers to continue testing on Holesky as we gear up for the upcoming mainnet launch. As a reminder: points are](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[![On Liquid Restaking: Risks & Considerations](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/2024-03-21-13.07.06.jpg)](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[In recent months, liquid restaking protocols and liquid restaking tokens (LRTs) have seen tremendous growth, and are beginning to become systemically important in the EigenLayer ecosystem. There are many different flavors of LRT, each with different values, features, and offerings. All of these LRT projects are independent of the underlying](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[![Ethos: Powering the Convergence Era of Blockchains](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/ethosxeigen.png)](https://www.blog.eigenlayer.xyz/ethos/)

[Blockchains are entering an era of convergence, characterized by the dissolution of boundaries between different network architectures. Cosmos started as a network of appchains, with each chain independently establishing its validator set and trust protocol using native tokens. Ethereum, on the other hand, chose a shared security approach where every](https://www.blog.eigenlayer.xyz/ethos/)

[![EigenLayer Mainnet: Preparation for launch!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/x--2-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[We're thrilled to announce the next chapter in our journey towards a secure and successful EigenLayer mainnet launch! Our primary focus remains a smooth launch that prioritizes both security and performance. To achieve this, we're introducing a multi-phased approach starting now and ongoing for the next](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[![Security Bounty Competition on EigenLayer by Cantina](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/IMAGE-2024-03-07-12-54-55.jpg)](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[Cantina is a marketplace for web3 security that gives protocols the flexibility to easily book a security review with their desired team, price, and timeline. They have recently launched a competition focused on EigenLayer. This competition presents an exciting opportunity for security researchers and professionals to contribute to the safety](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[![Ritual ♾️ EigenLayer: AI × Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[Summary\\
\\
The worlds of artificial intelligence and onchain protocols are increasingly intersecting as permissionless protocols look to unlock new customer behavior around ownership and markets by utilizing AI models. We’ve witnessed how in offchain settings, AI models could dramatically improve on the status quo of problem solving across various](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[![Inco: Building an Universal Confidential Computing L1 on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/----5-.png)](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[One of the open challenges in the blockchain industry is achieving trustless confidentiality. The inherent transparent nature of public blockchains prevents the development of applications requiring on-chain confidentiality across gaming, decentralized finance (DeFi), governance, and identity without relying on a trusted third party.\\
\\
Current approaches to providing privacy on the](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[![Balancing Neutrality and Decentralization in EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/EigenLayer_Ghost_balancing2_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[Today, EigenLayer has unpaused token restaking and removed TVL caps for every token. While the unpause is temporary this time, in the coming months the pause and caps will be lifted permanently. In this post, we reflect on the challenges of balancing neutrality and decentralization in the protocol, and suggest](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[![With New Arbitrum Orbit Integration, EigenDA and AltLayer Bring Horizontal Scalability to the Ethereum Ecosystem](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/Untitled.png)](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[Feb 2, 2024 – EigenLabs, along with Offchain Labs and AltLayer, today announced EigenDA support for Arbitrum Orbit chains, bringing scalability to the Ethereum ecosystem without sacrificing on security. The integration offers developers the ability to build EigenDA-based Orbit rollups that bridge from Arbitrum One, Arbitrum Nova, and Ethereum, and boast](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Polymer x EigenDA: Open, Neutral Interoperability for Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/BlogPost-Partnership-polymer.jpg)](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[EigenDA is a data availability store made by EigenLabs and built on top of EigenLayer. Currently live on the Goerli testnet, EigenDA is on a path to launch on mainnet later this year, with support for up to 10 MB/s in write throughput. As a rollup developer, if you&](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Cosmos. Ethereum. EigenLayer.](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Screenshot-2024-01-09-at-7.49.43-AM.png)](https://www.blog.eigenlayer.xyz/cosmos/)

[By connecting Ethereum and Cosmos, EigenLayer will bring in a new wave of innovations.](https://www.blog.eigenlayer.xyz/cosmos/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Announcing EigenDA x OP Stack Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/v2--1-.png)](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[We built EigenDA to help rollups scale, and thus help Ethereum scale. Rollups need somewhere decentralized, scalable, and secure to post transaction data so they can scale their systems to the next billion users. To achieve this mission we intend to make it as easy as possible for the next](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[![Versatus x EigenDA: The First Stateless Rollup](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/BlogPost-Partnership-versatus.png)](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[EigenDA launched on testnet a few weeks ago, alongside the EigenDA Launch Partner Program featuring eight rollup infrastructure providers: AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.\\
\\
We kicked off a series of case studies on these launch partners starting with Layer N x EigenDA: A Case](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Layer N x EigenDA: A Case Study in Hyperscale DA for Finance](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/BlogPost-Partnership-02--2-.png)](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[Now that EigenDA is live on testnet, we're proud to announce the first participants in the EigenDA Launch Partner Program. The first eight rollup infrastructure providers who are actively working to deploy EigenDA as a data availability option for their end users include: AltLayer, Caldera, Celo, Layer N,](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![Dual Staking: secure a PoS network with two tokens](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/dualstaking.png)](https://www.blog.eigenlayer.xyz/dual-staking/)

[One of the most powerful features of EigenLayer is the notion of dual staking. In this article, we will discuss what dual staking is, how it increases the robustness and decentralization of any PoS network, how it mitigates the death spiral problem with network token volatility, and how these networks](https://www.blog.eigenlayer.xyz/dual-staking/)

[![You Could've Invented EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/i-made-eigenlayer-copy-2.jpg)](https://www.blog.eigenlayer.xyz/ycie/)

[In this blog post, we will take you through the evolution of the protocol, by covering how EigenLayer's architecture emerged from the initial concept.](https://www.blog.eigenlayer.xyz/ycie/)

[![Announcing EigenLayer Research Fellowship](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/ERFwhite.png)](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[Update: The EigenLayer Research Fellowship (ERF) structure has been updated on October 25th, 2023, particularly in selection criteria and program dates.\\
\\
We believe EigenLayer is about to kickstart a new era of secure innovative infrastructure applications that will transform how blockchain applications operate. We’ve been diving deep into these](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[![The EigenLayer Universe: Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/Eigen-Universe-3.png)](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[EigenLayer empowers builders to develop innovative distributed systems without worrying about how to build the underlying trust networks for these systems. We call these distributed systems AVSs - actively validated services. We have categorized AVSs into 5 types:\\
\\
1\. Rollup Services: augmenting the Ethereum rollup ecosystem with services that inherit](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![The Three Pillars of Programmable Trust: The EigenLayer End Game](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/image--1--1.png)](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[Thanks to Alex Obadia, Austin Griffith, Dan Elitzer, David Phelps, Jon Charbonneau, Soham Zemse, and Waylon Jepsen from the community for reviewing and giving feedback. \\
\\
Today, if any developer wants to build a smart contract protocol such as a DEX, a lending protocol, etc., on Ethereum, they can inherit security](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[![Intro to EigenDA: Hyperscale Data Availability for Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/09/EigenDA_Ghost_Header_IntroToEigenDA_1200x675_01.png)](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[If you're interested in integrating your rollup with EigenDA, please fill out the EigenDA questionnaire!\\
\\
EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA will be the first actively validated service](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[![Censorship Resistance with Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_Liveness-firstRelay_1200x675_01.png)](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[MEV-Boost protects Ethereum’s validator set from Maximum Extractable Value’s (MEV) centralization pressures. It is an open-source project designed by the Flashbots team. MEV-Boost’s relays simplify the interaction between builders and proposers in this system, streamlining the system and eliminating the need for complex cryptography.\\
\\
There are however](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

Subscribe![EigenLayer Blog](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/04/EigenLayer_Ghost_Background_01-2.png)

Research, Updates, and Announcements

[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Polymer x EigenDA: Open, Neutral Interoperability for Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2024/01/BlogPost-Partnership-polymer.jpg)](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[EigenDA is a data availability store made by EigenLabs and built on top of EigenLayer. Currently live on the Goerli testnet, EigenDA is on a path to launch on mainnet later this year, with support for up to 10 MB/s in write throughput. As a rollup developer, if you&](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Cosmos. Ethereum. EigenLayer.](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2024/01/Screenshot-2024-01-09-at-7.49.43-AM.png)](https://www.blog.eigenlayer.xyz/cosmos/)

[By connecting Ethereum and Cosmos, EigenLayer will bring in a new wave of innovations.](https://www.blog.eigenlayer.xyz/cosmos/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Announcing EigenDA x OP Stack Support](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/12/v2--1-.png)](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[We built EigenDA to help rollups scale, and thus help Ethereum scale. Rollups need somewhere decentralized, scalable, and secure to post transaction data so they can scale their systems to the next billion users. To achieve this mission we intend to make it as easy as possible for the next](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[![Versatus x EigenDA: The First Stateless Rollup](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/12/BlogPost-Partnership-versatus.png)](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[EigenDA launched on testnet a few weeks ago, alongside the EigenDA Launch Partner Program featuring eight rollup infrastructure providers: AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.\\
\\
We kicked off a series of case studies on these launch partners starting with Layer N x EigenDA: A Case](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Layer N x EigenDA: A Case Study in Hyperscale DA for Finance](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/11/BlogPost-Partnership-02--2-.png)](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[Now that EigenDA is live on testnet, we're proud to announce the first participants in the EigenDA Launch Partner Program. The first eight rollup infrastructure providers who are actively working to deploy EigenDA as a data availability option for their end users include: AltLayer, Caldera, Celo, Layer N,](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![Dual Staking: secure a PoS network with two tokens](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/dualstaking.png)](https://www.blog.eigenlayer.xyz/dual-staking/)

[One of the most powerful features of EigenLayer is the notion of dual staking. In this article, we will discuss what dual staking is, how it increases the robustness and decentralization of any PoS network, how it mitigates the death spiral problem with network token volatility, and how these networks](https://www.blog.eigenlayer.xyz/dual-staking/)

[![You Could've Invented EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/i-made-eigenlayer-copy-2.jpg)](https://www.blog.eigenlayer.xyz/ycie/)

[In this blog post, we will take you through the evolution of the protocol, by covering how EigenLayer's architecture emerged from the initial concept.](https://www.blog.eigenlayer.xyz/ycie/)

[![Announcing EigenLayer Research Fellowship](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/ERFwhite.png)](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[Update: The EigenLayer Research Fellowship (ERF) structure has been updated on October 25th, 2023, particularly in selection criteria and program dates.\\
\\
We believe EigenLayer is about to kickstart a new era of secure innovative infrastructure applications that will transform how blockchain applications operate. We’ve been diving deep into these](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[![The EigenLayer Universe: Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/Eigen-Universe-3.png)](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[EigenLayer empowers builders to develop innovative distributed systems without worrying about how to build the underlying trust networks for these systems. We call these distributed systems AVSs - actively validated services. We have categorized AVSs into 5 types:\\
\\
1\. Rollup Services: augmenting the Ethereum rollup ecosystem with services that inherit](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![The Three Pillars of Programmable Trust: The EigenLayer End Game](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/10/image--1--1.png)](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[Thanks to Alex Obadia, Austin Griffith, Dan Elitzer, David Phelps, Jon Charbonneau, Soham Zemse, and Waylon Jepsen from the community for reviewing and giving feedback. \\
\\
Today, if any developer wants to build a smart contract protocol such as a DEX, a lending protocol, etc., on Ethereum, they can inherit security](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[![Intro to EigenDA: Hyperscale Data Availability for Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/09/EigenDA_Ghost_Header_IntroToEigenDA_1200x675_01.png)](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[If you're interested in integrating your rollup with EigenDA, please fill out the EigenDA questionnaire!\\
\\
EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA will be the first actively validated service](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[![Censorship Resistance with Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/08/EigenLayer_Ghost_Liveness-firstRelay_1200x675_01.png)](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[MEV-Boost protects Ethereum’s validator set from Maximum Extractable Value’s (MEV) centralization pressures. It is an open-source project designed by the Flashbots team. MEV-Boost’s relays simplify the interaction between builders and proposers in this system, streamlining the system and eliminating the need for complex cryptography.\\
\\
There are however](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)Eigen Labs is proud to introduce Permissionless Token Support, an upcoming update, to the EigenLayer protocol. This feature will enable any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic security of unlimited ERC-20 tokens on EigenLayer.

## **What**

With this update, AVSs can choose to accept any ERC-20 token as a restaked asset to provide cryptoeconomic security for their AVS. This allows AVSs to evaluate the supply and utility of all available tokens to create cross-ecosystem partnerships while ensuring the safety and security of their services. This increases alignment and connectivity across the ecosystem, moving us closer to the ultimate goal of open innovation.

## **Why**

1. **Expanded Cryptoeconomic Security**



Any ERC-20 asset will be able to be utilized for cryptoeconomic security, providing a more rich and diverse security landscape. This enhances the overall security options within the network, allowing AVSs to collaborate with community-owned tokens. Categories of potential ERC-20 assets include, but are not limited to, native AVS tokens, stablecoins, and BTC-denominated tokens.

2. **Customized Security Profiles**



AVSs will have the ability to fully customize their cryptoeconomic security profiles. They can choose to distribute work and rewards to a broad array of tokens supported by EigenLayer operators.

3. **AVS Token Utility**



The ability for AVSs to use their native token for cryptoeconomic security unlocks new utility for AVS tokens including gaining essential security functionality and being used for rewarding restakers and operators.

## How

At the protocol level, new Strategies (smart contracts that define which tokens can be restaked on EigenLayer) can be created using a factory contract for any ERC-20 token. These tokens can then be included in quorums by AVSs for utilization as cryptoeconomic security and the distribution of operator tasks in return for AVS rewards.

In an upcoming update for the staking app user interface, restakers will be able to deposit and delegate these tokens to operators.

## When

Currently, this feature is on testnet for a permissioned testing phase. [EigenDA](https://www.eigenda.xyz/) will be the first AVS to test and use permissionless token support.

**Mainnet deployment, a protocol-level update, is scheduled for next week**. **User interface support for restakers on the** [**EigenLayer web app**](https://app.eigenlayer.xyz/) **will be added later in Q3.**

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/permissionless-token-support/#/portal)

Subscribe## **Key Highlights**

- 8.25% of Total EigenLayer TVL
- Supporting 20+ AVSs
- 4 LRT partnerships
- $1B+ in delegated ETH and EIGEN
- Launch partner since June 2023

![](https://www.blog.eigenlayer.xyz/content/images/2025/02/PierTwo-Restaking-Economy--1--1.png)

## **Overview**

As one of APAC’s leading institutional staking providers, [Pier Two](https://piertwo.com/) recognized significant barriers preventing teams from building and deploying services in the crypto ecosystem. The challenge was not just technical - it extended to accessibility, capital efficiency, and infrastructure scalability for institutional clients.

## **Opportunity & Action**

Pier Two identified EigenLayer as a strategic opportunity to:

1. Enhance capital efficiency across operators, protocols, and services
2. Lower barriers to entry for new AVS deployments
3. Provide institutional-grade access to restaking opportunities
4. Scale infrastructure while maintaining security standards

By becoming an early EigenLayer launch partner in June 2023, Pier Two was able to expand rapidly, support 20+ AVSs, and secure over $1B in delegated assets.

## **Implementation Approach**

Pier Two took a methodical approach to integration:

1. Allocated dedicated resources for EigenLayer support
2. Developed feedback loops based on testnet infrastructure experience
3. Built highly configurable and scalable Infrastructure as Code (IAC)
4. Customized deployments for different LRT partner needs (Renzo, Ether.fi, Swell, Level)
5. Established robust registration and scaling processes

## **Results & Impact**

Since mainnet launch in April 2024, Pier Two has achieved:

- Market Leadership: Became one of the largest EigenLayer operators
- Ecosystem Growth: Supporting 20+ AVSs and 4 LRT partners
- Capital Efficiency: Managing over $1B in delegated ETH and EIGEN
- Knowledge Leadership: Developed deep expertise in restaking infrastructure
- Strategic Position: Early mover advantage in institutional restaking services

## **Future Vision & Roadmap**

Pier Two plans to continue collaborating with EigenLayer to:

- Expand APAC adoption and bring EigenLayer to a broader institutional audience
- Test and refine new products as an early adopter and core partner
- Support new AVS onboarding, helping operators scale seamlessly

![](https://www.blog.eigenlayer.xyz/content/images/2025/02/Pier-2-Case-Study.png)

> "EigenLayer not only had a first-mover advantage, with consistent messaging, but it was clearly pushing the boundaries across finance and technology. Pier Two wanted to be aligned on that mission, and continue to deliver value to our customers today and in the future."

> Jake Denny, Chief Commercial Officer, Pier Two

## Explore EigenLayer Operator Opportunities

Pier Two’s journey highlights the significant growth potential for operators within the EigenLayer ecosystem. By addressing key challenges in scalability, capital efficiency, and security, EigenLayer empowers operators to achieve new levels of success.

_👉 Ready to become an EigenLayer operator and unlock new opportunities?_

_Start your journey today by visiting our_ [_Operator Guide_](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-introduction) _for step-by-step instructions on how to onboard and scale within the EigenLayer ecosystem._

### **Contact Information**

**Pier Two:**

- [Website](https://piertwo.com/)
- [Twitter/X](https://x.com/PierTwo_com)

**EigenLayer:**

- [Website](https://www.eigenlayer.xyz/)
- [Twitter/X](https://x.com/eigenlayer)

**Pier Two Operators on EigenLayer:**

- [Pier Two](https://app.eigenlayer.xyz/operator/0x2e68d03f2234895b3ba5899b80785e2598ed7fac)
- [Ether.Fi - Pier Two](https://app.eigenlayer.xyz/operator/0xfb487f216ca24162119c0c6ae015d680d7569c2f)
- [Renzo - Pier Two](https://app.eigenlayer.xyz/operator/0x5dcdf02a7188257b7c37dd3158756da9ccd4a9cb)
- [Swell - Pier Two](https://app.eigenlayer.xyz/operator/0xdb69c57e9ec197a59d8144a42ecdfb37641be80d)
- [Level - Pier Two](https://app.eigenlayer.xyz/operator/0xd9e5d6f650d84ac58849be2ba07f7304b1ea87d7)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/#/portal)_EigenDA is a data availability store made by EigenLabs and built on top of EigenLayer. Currently live on the Goerli testnet, EigenDA is on a path to launch on mainnet later this year, with support for up to 10 MB/s in write throughput. As a rollup developer, if you'd like to learn more about EigenDA, read the_ [_Intro Blog Post and Litepaper_](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/) _, check out the_ [_Rollup User Guides and Documentation_](https://docs.eigenlayer.xyz/eigenda-guides/eigenda-overview) _, and submit your information via_ [_https://contact.eigenda.xyz/_](https://contact.eigenda.xyz/) _. We provide integrated partners with both technical and ecosystem support. We'll be in touch soon!_

## Introduction to Polymer

Polymer, the "Interoperability Hub of Ethereum", is paving the way for collaboration among Ethereum rollups through its native IBC (Inter-blockchain Communication) technology. As the Ethereum ecosystem grapples with scaling challenges and the need for composability, Polymer emerges with a customizable model of interoperability that connects Ethereum with its integrated Layer 2 solutions.

Since 2015, Ethereum has faced scalability issues, despite the success of its Layer 2 solutions. While these layers improve scalability and user experience, they introduce execution environments that hinder liquidity and complicate the developer journey. Ensuring composability across Layer 2s remains a challenge due to the absence of a standardized native message-passing solution that the Polymer team is tackling using IBC.

### Understanding IBC

IBC has several properties that distinguish it as an interoperability standard versus alternatives.

- **Credible neutrality.** It is not primarily associated with any one company or any DeFi products.
- **Large contributor base.** IBC currently has 17+ different companies and 120+ individual contributors.
- **No token or value capture at the protocol level.** Interoperability standards with a token or encoded value capture may affect credible neutrality and be unsuitable for enshrinement or native integrations.
- **Application composability.** IBC has a solid core feature set with opt-in middleware that extends its core functionality for building composable applications.

Polymer positions itself not as a bridge but as a Layer 2 dedicated to serving as Ethereum's Interoperability Hub. Polymer provides IBC execution to connected Layer 2s without requiring a native IBC integration, enabling new L2s to permissionlessly connect to the IBC network.

Polymer enhances composability between Ethereum rollups while leveraging IBC’s expanding network of applications. Although initially focused on the Ethereum ecosystem, the Polymer network looks to scale and connect all major blockchains.

Polymer will support various modes of verification or IBC clients between connected rollups. The default mode of verification will be to use the Ethereum L1 for verification. Additionally, any AVS that performs state transition verifications of rollups can be turned into an IBC client and used for pre-confirmations.

### Key Elements of Polymer

Polymer's modular stack consists of a few main building blocks.

- **Settlement (OP Stack).** Provides settlement and chain derivation logic to and from Ethereum.
- **Execution (Cosmos SDK).** Polymer’s virtual IBC protocol enables developers to build application-specific rollups with the Cosmos SDK on Ethereum providing builders with more optionality.
- **Data Availability (EigenDA).** Ensures scalable, secure, and cheap data availability.
- **Proving (OP Stack’s Fault Proof System).**
  - Interactive Fraud Proofs (IVG) for secure verification games.
  - ZK Validity Proofs using Rust ZK program by RiscZero.

## Using EigenDA for Security and Scalability

The core insight of EigenDA is that the problem of data availability does not require independent consensus to solve. If we are building a decentralized transient data store for Ethereum, we can use Ethereum for aspects of coordination required, and for everything else address EigenDA operators directly.

EigenDA has the following characteristics:

- **Scalable.** EigenDA write throughput scales linearly with its number of operators. On mainnet launch EigenDA will provide 10 MB/s of write throughput.
- **Secure.** EigenDA is made up of hundreds of operators registered in EigenLayer, whose delegated stake imposes an economic cost to misbehavior. EigenDA expects to have billions of dollars of economic security at launch.
- **Cheap.** The abundance of L2 blockspace provided by EigenDA will demand a new kind of pricing model. Where the pricing of L1 blockspace is primarily constrained by supply, EigenDA blockspace pricing is primarily constrained by cost-of-security, which is shared with Ethereum and other EigenLayer AVSs. As a result we expect EigenDA to be orders of magnitude cheaper than alternatives.
- **Ethereum-centric.** EigenDA's design is inspired by Danksharding, which promises to scale Ethereum-native DA beyond EIP-4844. EigenDA blob writes are registered with contracts on Ethereum, which natively subject operators to certain slashing risks. Ethereum L2s using EigenDA avoid any trust assumption on another chain's light client.

Choosing EigenDA aligns with Polymer's commitment to security and scalability, taking advantage of security measures from Ethereum staking and the validator set. The flexible cost model of EigenDA ensures affordable data availability services, crucial for high throughput applications like cross-chain interoperability.

### Integrating EigenDA with OP Stack

Polymer is building on top of Optimism's OP Stack. EigenLabs recently announced [integration support of EigenDA with OP Stack](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)! Let's understand briefly how the integration works.

The components of OP Stack fall into four categories:

1. **L1 Contracts:** These contracts manage bridging to and from Ethereum. In the future, they will include logic to verify fraud proofs.
2. **Node Components:** OP Stack nodes are responsible for scaling L2 reads and verifying that the sequencer is acting honestly. They scale L2 reads by having verifier nodes independently derive the L2 chain state from the **DA** layer and serving read traffic. In the future, they will be able to challenge a sequencer on-chain if their derived state root does not match the state root posted on-chain by the sequencer.
3. **Sequencer Components:** The sequencer is a node that has two additional jobs: to post batches of transactions to **DA**, and to post state roots of executed transactions to Ethereum.
4. **Execution Component**: The execution engine of the OP stack is by default op-geth. Polymer replaces op-geth with the Cosmos SDK for execution instead which comes with native IBC interoperability and Polymer’s virtual IBC protocol for permissionless IBC network expansion.

![](https://www.blog.eigenlayer.xyz/content/images/2023/12/SVG-for-Teddy.svg)

The OP Stack batcher is the sequencer module responsible for posting batches of L2 transaction data to DA. We modified this component to write batches to EigenDA instead of Ethereum. When a batch is successfully written to EigenDA, the disperser returns a unique blob key (4), which can be used to later retrieve the data that was written. The batcher then posts this blob key to Ethereum calldata (4), so that Ethereum remains the source of truth for the L2 ordering of EigenDA blobs.

Importantly, the OP Stack x EigenDA fork supports writing each batch to multiple EigenDA quorums, enabling redundant security and mitigating data withholding attacks.

**Node EigenDA Reads**

OP Stack full nodes derive the L2 state from transaction data posted to DA, downloading batches and executing them. We modified the OP Stack node to seamlessly handle retrieving EigenDA blobs using blob keys indexed in the Ethereum calldata. This involves a set of scatter-gather requests to EigenDA Operators storing chunks of the requested blob. If the retrieval of the blob from one quorum fails, the next will be tried, until the blob is retrieved or there are no more quorums to try.

**Limitations**

At the time of writing, OP Stack does not support fraud proofs, and neither does the EigenDA x OP Stack fork. When fraud proofs are enabled for OP Stack, we plan to update the contracts in OP Stack to support verifying the correctness and availability of L2 inbox data using KZG verification and calls to EigenDA contracts.

## Polymer Innovates on Cutting-Edge Technology

Polymer combines OP Stack’s settlement capabilities, Cosmos SDK’s interoperability, and EigenDA’s data availability solution. In doing so, Polymer is delivering a leading hub for domain-specific interoperability, which improves the developer experience, facilitates interconnectivity, and prioritizes security across Ethereum.

Not only is Polymer addressing Ethereum's current challenges, but it also facilitates collaboration with Cosmos. By bringing IBC to Ethereum and allowing for direct connectivity and standardized composability between the two ecosystems, Polymer unites the strengths of both platforms. Ethereum and Cosmos, previously built in siloes, now share innovations, accelerating advancements for both chains.

In summary, Polymer is leading the way toward achieving interoperability and scalability for Ethereum and its Layer 2 chains. It tackles composability issues while fostering collaboration among ecosystems. And finally, by using EigenDA, it is able to achieve greater security, scalability, and cost-effectiveness in achieving its mission.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/polymer-eigenda/#/portal)

SubscribeSolidity events are a crucial feature of Ethereum and EVM blockchains, with a vast number of use cases within the ecosystem. Primary use cases, for example, include but not limited to

- Logging: where events provide a mechanism to log critical actions and state changes inside smart contract, for track contract behaviors and debugging
- Off-Chain Notification: where offchain applications can listen to and monitor specific onchain actions of smart contracts, and trigger downstream logic
- Data Indexing, Processing and Analytics: where events can be indexed, stored, processed, aggregated and analyzed, empowering developers and analysts to gain data points, insights and patterns of smart contract
- Cross-Contract Communication: where smart contract can use events as a lightweight mechanism to communicate with each other
- Security and Monitoring: where events provide a detailed record of actions taken by smart contracts, that can be monitored in real time for emergency and security

Solidity events are critical to EigenLayer. The protocol emits a wide range of events on Ethereum, their use cases span across the protocol, stack and ecosystem. For example

- EigenLayer smart contracts log events for testing and debugging
- Several offchain applications listen to onchain events and trigger event driven logic, like subscribing to Beacon chain state oracle in order to process EigenPods related logic
- Events are indexed into data store and data lake, to power analytics both internally like internal BI dashboards and externally like community boards on Dune, answering wide range of questions from all angles about EigenLayer, like how many restakers are there and how much have they delegated, how many Operators and AVSs are registered and what’s their registration mapping relationship
- A variety of events are processed to support critical protocol features like calculating reward, and soon slashing
- Withdrawal events are being monitored in real time to alert team on any unexpected behaviors and potential risks

Thus, designing Solidity events in an efficient, scalable, cost-effective way and establishing a set of guidance and best practices, are critical for EigenLayer, as well as other protocols on Ethereum and EVM.

There has been a lot of content online of best practices and how-to of designing smart contracts, but little to no content on best practices of designing events.

Today I present the methodologies and best practices of designing Solidity events, so you can maximize their value as a foundation layer for empowering web3.

## 1/ Descriptive

Events describe what have happened onchain, thus should be self descriptive. Others should be able to immediately understand what the event is about by reading its name and schemas. When defining a new event, be deliberately specific. Avoid acronyms in naming.

### DONT:

```
URE {
  id
}
```

what is URE? is the id a user id, transaction id, or some other id?

### DO:

```

UserRegisteredEvent {
	user_wallet_address
}
```

I immediately understand what this event is entirely about, thanks to full event name and its fields in schema.

## 2/ Factual, by semantics

Events should match exactly what happened onchain. No mask, avoid any unconscious intention, be brutally honest of what the event means by semantics.

Ambiguous and mismatched semantics will result in disasters in downstream applications.

Think of an example where a system allows users to request deletion of their account. The safest way is to let users submit a request to delete their account, enqueue the request, and schedule actual deletion 7 days (or some other time period) later as buffer in case the user regrets and withdraws the request.

### DONT:

Developers may unconsciously think the initial request is an actual deletion action and come up with an event like:

```
UserDeletionEvent {
  user_id
}
```

But when downstream application developers see the event, they may highly likely interpret it as the account should be deleted ASAP or already been deleted, and doing unintended actions like delete user data, which will result in user data loss.

### DO:

Match events exactly as what they mean by semantics, by clarifying it’s only a “request” event

```
UserDeletionRequestedEvent {
	user_id
}
```

## 3/ Atomic and Composable

Actions and behaviors sometimes can consist of sub actions. Event design should maintain the fine granularity of behaviors by breaking down these complicated actions into smaller, atomic behaviors which can’t be further broken down. These atomic events together can then restore the overall picture of history.

Continue with the example above.

### DONT:

The developer takes a shortcut by combining the two steps in a single deletion action and event, trying to save some time and effort

```
UserDeletionEvent {
	user_id
	request_time
	deletation_time // which is request time plus 7 days
}
```

However, there are obvious flaws:

1. the 2nd steps may not happen, or may happen at a different time due to whatever reason, eg, system failure and retry, so the event will actually be inaccurate
2. the end-to-end “deletion” process actually consist of several events rather than two. e.g. request may be withdrawn later which will result in a 3rd event type as “UserDeletionWidthdrawnEvent” while the above monolithic event has not considered or covered

### DO:

Break the seemingly-simple action into actual atomic sub steps

```
UserDeletionRequestedEvent {
	user_id
	request_time
}

UserDeletionWithdrawnEvent {
	user_id
	withdraw_request_time
}

UserDeletionCompletedEvent {
	user_id
	deletation_time
}

```

## 4/ Self-contained

An event should define itself well and contain all necessary information to interpret it, without requiring external information or as minimal external dependency as possible.

Plus, events are usually used by downstream applications and developers, thus making events self-contained is key to simplify downstream developers’ life and overall system complexity, without requiring extra development and maintenance of pipelines to join with external data via database or rpc calls.

### DONT:

```
UserDepositEvent {
	user_id
	deposit
}
```

Wait! Where does it deposit from and to? Deposit what? I need to join other events to get this information?!

### DO:

```
UserDepositEvent {
	user_id
	erc20_token
	amount
	from_address
	to_address
}

```

Whereas with above event schema, you got everything you need to interpret this event!

### DONT 2:

externalize data by requiring retrieving them via external database or rpc calls, making it self-incomplete.

```
XEvent {
	external_url
}

```

there are so many problems with this event:

- Content inside the external url is not accessible or verifiable onchain, subject to fraud and security breach if anyone consumes the url directly
- Content inside the url may change overtime and you have no visibility of what has changed or know what it was originally
- The url may be even inaccessible from offchain anymore if the owner brings it down, so what was actually in there? We lost the entire context and facts about it

So DO NOT put external link onchain or in an event!

### DO:

For whatever you needed onchain, put them onchain and into an event directly!

## 5/ Symmetric

Many onchain actions are symmetric, for example, users can register and deregister, deposit and withdraw, numbers can increase and decrease.

When mapping these actions to events, developers should do their best to make them symmetric and keep the beauty of engineering to reflect real world situations.

Think of an example of a user interacting with a wallet.

### DONT 1:

```
WalletBalanceChangedEvent {
	wallet_address
 	balance_changed_amount // the delta
	// ...
}
```

First, this doesn’t describe what the actual action is, only describes a side effect of the balance is changed.

Second, the source action is unclear since it can come from a large set of potential actions like deposit, withdrawal, refund, reimburse, penalty, etc. Downstream users of the event do not know which exactly it is. It violates principle number 2 “factual” that an event should be factual.

Last but not the least, such event design creates a lot of frictions for downstream developers to use it. Eg. they can’t tell if the balance increases (positive delta) or decreases (negative delta), unless actually reading the value and comparing it with 0, which always comes with extra cost and complexity. In very common use case of “I wanna check how much I have withdrawn this week”, developers would have to go thru all events one by one, rather than just a subset of events explicitly named \`Withdrawal\`

### DONT 2:

```
WalletDepositEvent {
    wallet_address
    from_address
    amount
    // ...
}

WalletWithdrawEvent {
    wallet_address
    []to_addresses
    []amounts
    // ...
}

```

It is bad because:

First, it violates principle 3 “Atomic” because each withdraw is an independent action/event itself regardless of whether your bank or dex or protocol allows batching

Second the asymmetry makes processing this data much harder - processing, storing, and retrieving these events requires different handling logic throughout your codebase, with one handling one value V.S. the other handling an array of values

### DO:

```
WalletDepositEvent {
    wallet_address
    amount
    from_address
}

WalletWithdrawEvent {
    wallet_address
    amount
    to_address
}
```

Definition of the events are symmetric, which makes the E2E data handling easy.

## 6/ Flat, not deeply nested

Another anti pattern in above DONTs example is the event contains nested fields.

```
WalletWithdrawEvent {
    wallet_address
    []to_addresses // nested fields
    []amounts // nested fields
    // ...
}
```

It’s not the worst until you see even more deeply nested fields like

```
X {
    []Y
}

Y {
    []Z
}
```

This is a nightmare for downstream developers to work with!

Nested raw events are not usable at all, 10 out of 10 times have to be flattened before it can be used, whether in SQL or any programming language. Believe it or not, flattening nested objects is not a fun job anywhere, especially SQL. Not to mention you have to figure out different SQL dialects for flattening. Plus, in order to work with the flattened results efficiently, they usually have to be materialized and stored somewhere as intermediate results, with additional burden of operating a processing pipeline and extra compute/storage cost.

To make it easier for your developer community, make the event flat.

```
WalletWithdrawEvent {
    wallet_address
    to_address
    amount
    // ...
}
```

Some may argue that emitting flat events are too costly on Ethereum, and nested events will help save cost. In my honest opinion, if developers see the event cost is a major blocker, they should probably consider moving to an L2 and flatten your events there!

## 7/ Entities and Domain Oriented

Categorize events into entities or domains. There might be actors like “users”, “stakers”, or “operators”, or domains like “servers”, “clients”. They are usually the subjects to initiate actions.

It’s highly recommended to evaluate and design your domains and entities, before actually programming anything. It helps with overall design and coming up with event names, e.g. for a “staker”, you can then think of all valuable actions or informations to capture around them.

Naming would be easier as well, a simple naming convention is “<Entity/Domain><Action>Event”, like “UserLoginEvent” and “UserRequestDeletionEvent”

Defining events around entities and domains make downstream usage much easier. Events can be organized and stored together by entities/domains, to provide domain/entity specific dataset and services; data discovery is easier by nature hierarchies; associations and relationships to link different events are simple, etc.

## 8/ Other Technical considerations

1. Control event size and frequency - don’t blow it up
2. Consider cost - emit from onchain vs offchain
3. etc

## Conclusion

In summary, Solidity events are mission-critical to EigenLayer and its ecosystem, as well as any protocol on Ethereum and EVM, and serve many primary use cases.

We described a variety of design principles and best practices, with concrete examples, to design Solidity events in an efficient, scalable, cost-effective and developer friendly way.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/#/portal)

SubscribeThis week, we updated our documentation to reflect an [evolution in our thinking](https://x.com/sreeramkannan/status/1870232724503703898) about AVSs. These services are about more than the act of validation. What developers are building are **Autonomous Verifiable Services**.

When we introduced the concept of Actively Validated Services (AVS) back in [February 2023](https://docs.eigenlayer.xyz/assets/files/EigenLayer_WhitePaper-88c47923ca0319870c611decd6e562ad.pdf), we intended to describe _how_ EigenLayer ensured correctness and security for services built in decentralized environments. Since its initial Mainnet launch, EigenLayer has empowered developers to quickly build scalable, secure services by leveraging decentralized, shared security, freeing them to focus on innovation rather than trust and infrastructure.

As our understanding of this technology’s potential has grown, we realized there is a better way to describe what AVSs represent.

Twitter Embed

[Visit this post on X](https://twitter.com/sreeramkannan/status/1870232724503703898?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F) [Visit this post on X](https://twitter.com/sreeramkannan/status/1870232721169277180?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[![](https://pbs.twimg.com/profile_images/1866862776637161472/mVf8f6FK_normal.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[Sreeram Kannan\\
\\
![](https://pbs.twimg.com/profile_images/1635879999264940033/_pozth32_bigger.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

·

[Dec 20, 2024](https://twitter.com/sreeramkannan/status/1870232721169277180?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[@sreeramkannan](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

·

[Follow](https://twitter.com/intent/follow?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F&screen_name=sreeramkannan)

[View on X](https://twitter.com/sreeramkannan/status/1870232721169277180?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[Replying to @sreeramkannan](https://twitter.com/sreeramkannan/status/1870232719210500295?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es2_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

Why slashing?
In a permissionless system of staking, you need accountability of actors who try to attack the system. Otherwise, your system will be overrun by attackers.

Outside of slashing, there is another incentive for negative behavior: the token holders staking are

[![](https://pbs.twimg.com/profile_images/1866862776637161472/mVf8f6FK_normal.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[Sreeram Kannan\\
\\
![](https://pbs.twimg.com/profile_images/1635879999264940033/_pozth32_bigger.jpg)](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[@sreeramkannan](https://twitter.com/sreeramkannan?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

·

[Follow](https://twitter.com/intent/follow?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F&screen_name=sreeramkannan)

Autonomous Verifiable Services
——————-
We earlier used the expansion Actively Validated Services - we are migrating to the expansion Autonomous Verifiable Services.

They are autonomous because they run on their own - anyone can stake and run the services. They are verifiable [Show more](https://mobile.twitter.com/sreeramkannan/status/1870232724503703898?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[10:20 PM · Dec 20, 2024](https://twitter.com/sreeramkannan/status/1870232724503703898?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

[X Ads info and privacy](https://help.twitter.com/en/twitter-for-websites-ads-info-and-privacy)

[78](https://twitter.com/intent/like?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F&tweet_id=1870232724503703898) [Reply](https://twitter.com/intent/tweet?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F&in_reply_to=1870232724503703898)

Copy link

[Read 2 replies](https://twitter.com/sreeramkannan/status/1870232724503703898?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1870232724503703898%7Ctwgr%5Ebe74b9f3eeca525acbae77e3b096b11872958734%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2Fredefining-avs-from-actively-validated-to-autonomous-verifiable-services%2F)

## Why the Change?

"Actively Validated" describes how these services are continuously monitored and validated to ensure they function correctly. The "Autonomous Verifiable" framing highlights two critical characteristics of these services:

1. _**Autonomous:**_ These services run on their own. Once deployed, they don’t rely on any central authority or ongoing intervention.
2. **_Verifiable:_** If a commitment is violated, it can be detected, tested, and verified, triggering consequences like slashing to maintain system integrity.

Combining autonomy and verifiability creates a powerful dynamic, which is precisely what makes EigenLayer unique. It’s not just about building services; it’s about building services that are open, scalable, and secure by design.

_Note: There’s no technical change to the implementation at this time - we are simply evolving our terminology to better reflect the potential of these services._

## Let’s Break It Down: Autonomous + Verifiable

### Autonomous: Permissionless and Self-Sustaining

When we say these services are autonomous, we mean they’re designed to operate without constant supervision. They’re open to anyone who wants to participate. You can stake, run, and contribute to them without needing special permission.

This autonomy isn’t just a technical feature—it’s a principle. It ensures that innovation remains open and that no single party controls the system’s functionality. This is a core aspect of what makes decentralized ecosystems so powerful.

### Verifiable: Ensuring Safety and Correctness

"Verifiable" means that any deviation from expected behavior can be objectively identified. Whether a service fails to meet its promises or engages in malicious behavior to exploit the system, these violations can be verified and penalized.

In EigenLayer, this often involves operator ejection or slashing mechanisms. If a service violates its conditions, participants who allocate stake to it can be penalized, ensuring that bad behavior has real consequences. This keeps the ecosystem honest and secure.

### A Look Ahead

As we continue to refine and expand the role of Autonomous Verifiable Services with EigenLayer, this shift in definition will help set the tone for the future of decentralized infrastructure more broadly. Our goal is to create systems that don’t just work today but can adapt and thrive in the evolving landscape of Web3.

We believe this change better reflects that vision, and we’re excited to see how developers, stakers, and consumers interact with these new and improved AVS-powered services.

Thanks for being part of this journey. Let’s keep building!

👉

[Meet with our team](https://share.hsforms.com/1BksFoaPjSk2l3pQ5J4EVCAein6l) to discuss your AVS idea.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/#/portal)

Subscribe

Twitter Widget Iframe_Restaked rollups: A New AVS Category_

_Proposed by Altlayer and supported on EigenLayer_

Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.

1. **Decentralized sequencers** to obtain low-latency censorship resistance.Decentralized sequencers come in different flavors - some perform fully shared sequencing, and some are rollup-specific. They also follow different MEV management: MEV auctions, threshold encryption, and first-come, first-served are some examples of MEV policies for sequencers.
2. **Fast finalization** requires economic safety guarantees
3. **Watcher networks** incentivize nodes to watch optimistic rollups and raise fault-proof challenges.
4. **External DA services** such as EigenDA can help achieve different points in the security-cost tradeoff.
5. **Fast composability and bridging services** can empower fast bridging across different rollups based on economic stake. Some fast bridging services require a new hub that needs economic stake for safety, whereas others are direct peer-to-peer but also need economic safety.

EigenLayer, as a general-purpose system for programmable trust, can be utilized to build several of these functionalities. EigenLayer provides the following features:

1. **Pooled economic security**: i.e., if $10B is restaked across 1000 services, then to attack any one service, the attacker needs a majority of $10B of stake
2. **Attributable economic security**: if malicious validators compromise any one service, that service can redistribute funds proportional to the amount of attributable economic security it borrowed. The protocol ensures that even if malicious validators attack multiple services simultaneously, it can honor the amount of redistribution to all the services, i.e., it ensures protocol solvency.
3. **Decentralized operator sets**: that can be utilized by services to obtain some amount of credible neutrality and decentralization

Restaked rollups are a new category of EigenLayer AVS services that combine several of the services into a single bundle so that rollup users can benefit from a single-point integration while benefiting from several new features.

We are really excited to announce a partnership with AltLayer on this new category of AVSs, bringing together these features into a “restaked rollup” framework. This novel framework combines the ETH staking mechanism with AltLayer’s end-to-end infrastructure bundle, combining decentralized sequencing, verification, and fast finality based on the EigenLayer shared security substrate.

We believe this will provide rollup developers with powerful features, a one-click integration, and a streamlined developer experience and greatly accelerate the adoption of Ethereum's rollup-centric roadmap.

Read a longer version of this post with Altlayer [here](https://blog.altlayer.io/introducing-restaked-rollups-ac6a1e89b646).

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/restaked-rollups/#/portal)

Subscribe![...](https://docs.eigenlayer.xyz/assets/images/rewards-claim5-630bdf4627a2efba655108a1f5648581.png)

We're proud to announce the launch of the [MVP experience for Rewards](https://holesky.eigenlayer.xyz/) on the Holesky testnet. This MVP allows AVSs to begin integrating with the rewards system, and test setting up incentives for restakers and node operators. It also allows restakers to test claiming functionality for (worthless) testnet tokens.

Over the next few weeks, the full rewards experience will roll out on testnet. Besides allowing restakers to claim protocol rewards, it will let restakers see and discover estimated potential payments from AVSs. Subsequently, the full rewards experience will launch on mainnet.

Note that on testnet, most reward tokens will not have a price, and the testnet experience will reflect that.

### **Significance of the Rewards MVP**

The Rewards MVP is an opportunity for the EigenLayer community to participate in the following ways:

- **For AVSs:** Test the functionality of the rewards system, experimenting with incentives for node operators and restakers on the Holesky test network.
- **For Node Operators:** Explore how to earn rewards for participating in AVSs and providing essential services to the EigenLayer ecosystem.
- **For Restakers (Delegators):** Experience how to delegate stake to operators and claim your share of the rewards generated from AVSs.

This collaborative testing environment will refine the rewards system, and ensure a successful mainnet launch, ultimately driving shared success within the EigenLayer ecosystem.

### **Key Benefits and Features**

For the full details, please see the [Rewards documentation.](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/rewards-claiming-overview)

- **AVS Token Distribution Channel:** Streamlined distribution of any ERC-20 token directly to operators and stakers for each AVS. AVSs set their own band rewards (between 7- 90 days) paid in 7 day increments.
- **Flexible rewards API:** AVSs can easily schedule recurring token rewards with a single on-chain transaction.
- **New In-App Claiming Experience & API:** Simplified mechanisms for operators and restakers to claim their earned rewards using the API or the application.
- **Efficiency:** Secure and scalable rewards infrastructure with weekly settlements and batch posting to L1.
  - Restakers can claim rewards weekly or let them accrue.
  - Operators and Restakers can set their claim address to an address of their choosing. This Enables compliance, safe hot wallet practices, and protocol integrations.

### **Future Roadmap:**

We're committed to rapid iteration and improvement. Here's a glimpse of what else is on the roadmap:

- **Additional Rewards Testnet Features:** Rewards discovery experience for restakers.
- **Mainnet Launch:** Following successful testing and validation on the Holesky testnet, Rewards will launch on EigenLayer mainnet.
- [**EigenPod Redesign**](https://hackmd.io/U36dE9lnQha3tbf7D0GtKw) **:** EigenPods are getting a major redesign in Q3, bringing checkpoint proofs to enhance the accuracy and efficiency of ETH holdings.
  - The Q3 upgrade will introduce batched withdrawal claiming, making the process up to 100x cheaper! It will also be possible to restake both consensus and execution rewards, and enjoy more flexible proving windows.
- **Objective & Intersubjective Slashing:** Enhanced security mechanisms to ensure operator reliability.
- **Unique Security:** AVSs can purchase "unique security" to protect themselves and their stakeholders. In the event of a failure, this feature gives AVSs the exclusive right to slash a portion of restaked capital. This supplementary layer of protection, alongside pooled security, allows AVSs the ability to enjoy the benefits of pooled security while limiting collateral damage to node operators, restakers, and other AVSs in the event one fails.

These are just a few examples of the functionality you’ll see on EigenLayer in the coming months, with much more yet to be announced.

### **Get Involved!**

- **AVSs:** Explore the rewards API to incentivize your operator and restaker network.
- **Operators:** Register your nodes for AVSs and start claiming rewards on Holesky.
- **Restakers:** Delegate to operators running AVSs to test out claiming restaker rewards.
- **Developers:** Experiment with our APIs and provide valuable feedback.

Learn more in the [Rewards documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/testnet/rewards-claiming/rewards-claiming-overview), or try claiming via the [testnet app](http://holesky.eigenlayer.xyz/) now.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/#/portal)

SubscribeWe are excited to announce the arrival of the Rewards v2 protocol upgrade on Mainnet. It is designed to bring greater flexibility, efficiency, and customization to rewards within the EigenLayer ecosystem. This upgrade is also the first EigenLayer Improvement Proposal ( [ELIP](https://github.com/eigenfoundation/ELIPs/tree/main)), ELIP-001: Rewards v2, using the EigenLayer Governance process ( [EigenGov](https://blog.eigenfoundation.org/introducing-eigengov-part-1/)) recently announced by the Eigen Foundation! It is also the first Mainnet upgrade to be [proposed and executed by the Protocol Council](https://www.blog.eigenlayer.xyz/eigenlayer-2024/).

**What**

The Rewards v2 upgrade proposes an expansion to platform rewards functionalities introducing several powerful enhancements:

- **Operator-Directed Rewards:** AVSs can reward Operators based on performance or custom logic
- **Variable Operator Fees:** Operators can set their own fee rates per AVS, providing economic flexibility and attracting diverse participation
- **Batch Rewards Claiming:** Stakers and Operators can claim multiple rewards in a single transaction, reducing gas costs

These upgrades enable AVSs to incentivize operators dynamically, allowing for tailored reward mechanisms that better align with their specific needs.

There are no breaking changes for existing AVS and Operator code or contracts. The functionality in Rewards v2 is additive of the existing rewards functionality. If you are running the Eigen Sidecar to view, audit, or verify rewards data, you will need to update to v2.0.0. More information on the Sidecar update [here](https://github.com/Layr-Labs/sidecar/releases/tag/v2.0.0).

**When**

- **Testnet Launch:** Live on testnet as of December 14, 2024.
- **Mainnet Launch:** Upgrade is slated for execution by the Protocol Council on January 21st, 2025.

**Why**

Rewards v2 is an optional, lightweight addition to the rewards structures and contracts that address key rewards use-cases gathered through feedback from AVSs and operators following the release of the [first iteration](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/) of Rewards.

With greater flexibility for AVSs, Operators, and stakers, Rewards v2 will be a crucial value driver for the Eigen Economy.

**How**

The upgrade is live today on the EigenLayer Holesky testnet. Protocol Council execution on Mainnet will occur on or around the 21st. Following Mainnet, the up-take of Rewards v2 will be tracked by indexing the events of rewards on-chain. This can help determine the success of this ELIP and track the improvements to the protocol.

**Learn More**

- [ELIP-001: Rewards v2](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md)
- [Rewards v2 Technical Documentation](https://docs.eigenlayer.xyz/eigenlayer/rewards-claiming/rewards-claiming-overview#rewards-v2-currently-in-tesnet)
- [EigenLayer Governance (EigenGov)](https://blog.eigenfoundation.org/introducing-eigengov-part-1/)
- [EigenLayer Protocol Council](https://forum.eigenlayer.xyz/t/introducing-the-protocol-council/)
- [EigenLayer ELIPs](https://github.com/eigenfoundation/ELIPs/tree/main)
- [EigenLayer Forum](https://forum.eigenlayer.xyz/c/protocol-council/)
- Follow along:
  - [Discord](https://discord.com/invite/eigenlayer)
  - [X](https://x.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/rewards-v2/#/portal)

Subscribe## Summary

The worlds of artificial intelligence and onchain protocols are increasingly intersecting as permissionless protocols look to unlock new customer behavior around ownership and markets by utilizing AI models. We’ve witnessed how in offchain settings, AI models could dramatically improve on the status quo of problem solving across various domains, thus helping usher in new waves of products with better UX and cheaper costs.

As the industry saw the impact of open source software with the proliferation of Linux, Postgres, Docker and many other popular tools and systems, it became clear that AI could reap similar benefits. Platforms, products, and models in open source AI as a result have advanced very rapidly in the past few years. Hugging Face, Stability, Llama (introduced almost exactly a year ago), Mistral and the likes have all accelerated the open innovation rate of AI. However, until very recently, the most significant model and product advancements have been restricted to proprietary settings, reducing the pace of open innovation. The open source models and platforms to-date are directionally positive-sum, however…

- They usually need clients to run models on their own. Alternatively, if a third party runs models for the clients, they need to build trust with the clients which concentrates the market significantly
- There is no privacy relative to the model serving node (OpenAI has a full log of all queries ever served to any user)
- They do not encode an incentive mechanism for the model creator, model server, or any infrastructure in between.

EigenLayer helps lift this restriction by being the permissionless coordination substrate on top of which AI projects can easily offer unprecedented access to development and deployment of underlying models, as well as help create economically-sustainable paths to their productization.

## Ritual

Case in point is **Ritual**, building a sovereign execution chain optimized for AI-native operations to bridge the worlds of crypto and AI. Ritual recently launched **Infernet**, a decentralized oracle network, and the first product of its kind to allow smart contracts to natively access off-chain model outputs, while the **Ritual Chain** is slated to be rolled out later this year. Ritual Chain is composed of a customized VM that enshrines model actions like inference, finetuning, training, distillation, and more. It can serve these operations and models to any other execution layer via general message passing protocols.

Ritual implements a modular architecture across its stack and as it heads into the launch of its chain, EigenLayer will play a critical role in a few ways.

### Bootstrapping Operators

By helping bootstrap the network with operators, especially those with in-demand hardware, EigenLayer will help the Ritual Chain increase its availability and liveness across execution of AI model actions (inference, finetuning, distillation, quantization, training, etc). The decentralized distribution of this operator set will also help provide a strong level of censorship resistance for services and applications built on top of the chain. In turn, as these operators help serve AI requests permissionlessly, participants of Ritual Chain will contribute fees and revenue to the EigenLayer ecosystem and marketplace.

### Economic Security

As with any new chain launch, having assurances around the security of a chain is desired for providing a good end-user experience, whether that’s for application developers or their consumers. EigenLayer will help on this front by providing economic security from Ethereum to Ritual Chain’s deployment and by providing the network with slashing capabilities on this security. This mechanism ties seamlessly into Ritual’s proof marketplace as Ritual customers can combine proofs and slashing to gain confidence against dishonest behavior in the network.

### Dual Staking

While EigenLayer enables a robust initial deployment of the Ritual Chain, it also allows Ritual Chain to use dual staking to accrue value to its own network, a feature described [here](https://www.blog.eigenlayer.xyz/dual-staking/). The network can create richer incentive structures as its R&D design space expands for any actors in it, whether that’s model servers featuring action-based fee pricing, model creators, agents, RLHF participants, or any other role. Specifically, Ritual Chain is focused on providing immediate utility, and this north star will influence research & development into an incentive structure that could create a healthy open AI ecosystem. As discussed, exploring the incentive modeling for such a network that touches the many nuances of fulfilling the entire lifecycle of AI-native workflows is one of the most interesting and promising areas of the AI and crypto intersection.

### Model Routing

As the network matures alongside a variety of models and node types, abstractions over how to fulfill a user’s requests given some set of available models will become the norm. With the expressivity of the AVS design space, Ritual can explore the notion of how to best route requests optimally to the most relevant model providers. Factors that could matter here depend on the nature of the request: sensitivity to latency (node geography), inference performance (specific hardware), SLA / uptime (whether the model is being used for a financial application or entertainment purposes). etc. Not only that, the model routing mechanism can explore the incorporation of privacy-preservation such that the model servers can’t distinguish any consumers by their set of queries. These properties can create a seamless UX for Ritual Chain’s customers.

![](https://lh7-us.googleusercontent.com/kQFQKHrKk0clpUB_ljSBuF5InRqOIHJR1edNSvq-fCLNXQmDjgGAtwjVUaM8xTI59b16MjZMnhSRQpEq7lE4fhCKr706N2ies6mz-yN5dpH-LJghypGpYxRwsJen5Om6pPMht-In__FBdYHQ0lvyOf8)

There’s a combination of factors that the integration between Ritual Chain, Infernet, and EigenLayer can continually consider to provide the best experience possible for a permissionless inference network. We believe uniting the capabilities of both Ritual and EigenLayer to unveil new AI workflows will bring us closer to fulfilling the vision we set out to achieve: lifting the restrictions on open innovation, and encouraging more streamlined coordination for artificial intelligence development and usage. You can read more about the specifics of how some of these AVS designs could work on the Ritual blog post [here](https://ritual.net/blog/eigenlayer).

### Disclaimer

This post is for general information purposes only. It does not constitute investment advice or a recommendation, offer or solicitation to buy or sell any investment and should not be used in the evaluation of the merits of making any investment decision. It should not be relied upon for accounting, legal or tax advice or investment recommendations. The information in this post should not be construed as a promise or guarantee in connection with the release or development of any future products, services or digital assets. This post reflects the current opinions of the authors and is not made on behalf of Eigen Labs, Inc. (“Eigen Labs”) or its affiliates and does not necessarily reflect the opinions of Eigen Labs, its affiliates or individuals associated with Eigen Labs. All information in this post is provided without any representation or warranty of any kind. The opinions reflected herein are subject to change without being updated.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/#/portal)

SubscribeCantina is a marketplace for web3 security that gives protocols the flexibility to easily book a security review with their desired team, price, and timeline. They have recently launched a competition focused on EigenLayer. This competition presents an exciting opportunity for security researchers and professionals to contribute to the safety and integrity of the EigenLayer protocol while earning rewards for their efforts.

## **What is EigenLayer?**

EigenLayer is the first generalizable network providing programmable trust from Ethereum. Technically, it is a set of smart contracts on Ethereum as well as a set of off-chain node software that allows solo stakers, staking service providers and LST stakers to opt into running new offchain distributed systems.

## **Prize Distribution and Scoring**

The competition will follow a detailed scoring system outlined in the competition scoring page. **Only findings of Medium or higher severity will be accepted for consideration.**

## **Scope of the Review**

The review will focus on the upcoming M2 mainnet upgrade for EigenLayer and EigenDA. This upgrade, scheduled for end Q1/early Q2, includes a contract upgrade of the current core contracts from the M1 version to their M2 versions. Additionally, the deployment of EigenDA contracts (and off-chain infrastructure) will integrate with the M2 core contracts. Reviewers are tasked with examining the smart contracts for the M2 mainnet upgrade and analyzing the upgrade path from the current mainnet deployment to the target contracts.

## **Competition Details**

- Status: Now Live
- Total Reward: $100,000 USDC
- Start Date: 26 Feb 2024 10:00pm (local time)
- End Date: 18 Mar 2024 4:00pm (local time)
- _Log-in Required: Researchers must be logged in and registered on Cantina to participate._

## **How to Participate**

To participate in the competition, log in as a researcher and join the EigenLayer security bounty competition. For any issues or concerns, please reach out to the #talk-smart-contracts channel on Discord.

## **Conclusion**

Don't miss this opportunity to contribute to the security of EigenLayer and earn rewards for your efforts. Join the Cantina security bounty competition today and help make the blockchain ecosystem a safer place for all users.

For more information, visit the Cantina website and [explore the competition details.](https://cantina.xyz/competitions/4b6f08a7-e830-4499-9977-08e2c3b32068)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/#/portal)

Subscribe## **What’s Happening?**

In December, Eigen Labs [announced Slashing on Testnet](https://www.blog.eigenlayer.xyz/introducing-slashing/). The launch of Slashing on mainnet is a major milestone for the EigenLayer protocol. This April, Slashing will go live on mainnet. We want all of our users to be properly prepared for the mainnet launch.

## **What is Slashing?**

Slashing is the latest major protocol release in EigenLayer. With this release, AVSs can set specific conditions under which Operators must run their service. Operators can opt into those conditions when running AVS code. If Operators do not meet the conditions set, the AVS may penalize them. But, if the Operator runs the service successfully, AVSs can reward the Operator’s performance and incentivize specific activity. This is a major step forward in the EigenLayer protocol because it allows for a free marketplace where Operators can earn rewards for their work and AVSs can launch verifiable services.

## **Timing & Process**

### **Testnet**:

- Today - Slashing is live on Holesky Testnet. Users can continue testing today on Holesky.
- Coming Soon - Slashing will be live on Sepolia and Hoodi Testnets. The Hoodi EigenLayer Testnet is dependent upon smart contract infrastructure being deployed by third parties.
- Ongoing - Slashing will remain live on EigenLayer testnets so users can continue to test slashing.

### **Mainnet**

- April 7th - [The Protocol Council](https://blog.eigenfoundation.org/the-protocol-council/) will queue the transaction for the Slashing Upgrade to go live after a 10 day timelock.
- April 17th - Slashing will be live on EigenLayer on mainnet.

## **Key Reminders**

- Slashing adoption on mainnet will not happen all at once. On Day 1, slashing likely will not be fully adopted by all AVSs and their Operators. We expect this process to unfold over time as AVSs gradually implement it.
- Operators and Stakers will not be automatically opted in to slashing with the AVSs they are currently running. Operators will be able to opt into the slashing criteria that each AVS sets.
- With the launch of slashing on mainnet, there is a requirement for all stake withdrawals to take 14 days after being queued. This allows for AVSs to properly account for and slash, if necessary, stake pending withdrawal. Detailed rationale can be found [here](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#why-is-withdrawal_delay-set-to-14-days-worth-of-blocks).

## **What Each User Group Can Expect**

- [AVS Slashing Guidebook](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)
- [Operator Slashing Guidebook](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)
- [Staker Slashing Guidebook](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

## **Resources**

- [Read the Slashing ELIP here](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md).
- [Slashing is live on Testnet blog.](https://www.blog.eigenlayer.xyz/introducing-slashing/)
- [Review the Documentation on slashing](https://docs.eigenlayer.xyz/developers/HowTo/build/slashing/implement-slashing).
- Reach out with questions on [Discord](https://discord.com/invite/eigenlayer) or [Forum](https://forum.eigenlayer.xyz/t/elip-002-slashing-via-unique-stake-operator-sets/14275).

**Dive into _How Slashing Unlocks a New Era in Blockchain Security,_ where experts from LayerZero, Infura, and Eigen Labs reveal how this groundbreaking feature creates new opportunities for AVSs, Operators, and Stakers, while strengthening the entire ecosystem:**

How Slashing Unlocks a New Era in Blockchain Security - EigenLayer X Infura X LayerZero - YouTube

EigenLayer

1.95K subscribers

[How Slashing Unlocks a New Era in Blockchain Security - EigenLayer X Infura X LayerZero](https://www.youtube.com/watch?v=xFJkKsyCTms)

EigenLayer

Search

Watch later

Share

Copy link

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

More videos

## More videos

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

[Watch on](https://www.youtube.com/watch?v=xFJkKsyCTms&embeds_referring_euri=https%3A%2F%2Fwww.blog.eigenlayer.xyz%2F)

0:00

0:00 / 35:59•Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=xFJkKsyCTms "Watch on YouTube")

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/#/portal)

Subscribe​​

​​Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders to join the ecosystem and embrace this new paradigm.

The launch of EigenLayer will be phased in three stages to onboard various participants into the ecosystem, and ensure focus and a smooth experience for all participants:

- **Stage 1: Stakers** \- where stakers will be onboarded into restaking on EigenLayer
- **Stage 2: Operators** \- where node operators will be onboarded and can receive delegation from restakers
- **Stage 3: Services** \- where the first actively validated services on EigenLayer will be onboarded

The current launch of the Stage 1 testnet is built on the Ethereum Goerli network. We caution that this is an early and non-incentivized testnet, with code undergoing active development. We expect to make improvements in advance of mainnet, and we hope you'll contribute your feedback, comments, and ideas to help us do so.

EigenLayer's design is based on the core principle of credible neutrality - enabling intersubjectivity by allowing participants to make subjective decisions instead of being subject to protocol decisions. Thus, the EigenLayer protocol empowers services built on EigenLayer to choose which restaking modalities they value and trust, including specifying whether to accept native restaking and/or liquid restaking for security, as well as specifying which liquid staking tokens to accept.

The Stage 1 testnet supports the following restaking modalities:

**1\. Liquid Restaking**

EigenLayer supports various liquid staking tokens to be restaked on the EigenLayer contracts. The testnet supports stETH and rETH as examples. Assets that will be supported on mainnet will be announced in advance of the mainnet and based on community input.

**2\. Native Restaking**

- **New ETH stakers:** Can set their withdrawal credentials to their own EigenLayer smart contract “pods” when creating new validators.
- **Existing ETH stakers**: The upcoming Shapella upgrade to Ethereum, scheduled for April 12, 2023, enables existing native stakers to participate in EigenLayer:
- **Existing stakers with withdrawal credentials beginning in 0x00** can repoint their withdrawal credentials to an execution layer address such as their own EigenLayer smart contract "pods", after the upgrade goes live. This does not require exiting from the beacon chain.
- **Existing stakers with withdrawal credentials beginning in 0x01** can participate in EigenLayer by withdrawing from the beacon chain and depositing as a new validator, after the upgrade goes live. It is not possible currently to simply repoint withdrawal credentials for those with 0x01 credentials.

The [EigenLayer contracts](https://github.com/Layr-Labs/eigenlayer-contracts) are under active audits, and a bug bounty program will be launched after audit completion. The code is licensed under a [BSL license](https://spdx.org/licenses/BUSL-1.1.html) for two years, after which it converts into an MIT license. We welcome the community to provide any comments regarding the codebase on the GitHub repository.

**Guardrails**

The initial EigenLayer design optimizes for caution and security of user funds by using training wheels, which can eventually be removed. The EigenLayer contracts have two different multisigs: a team multisig, which controls limited functionality, such as pausing and adding new restakable tokens; and a community multisig, which controls upgradability. For this testnet, both these multisigs are handled by the early builders of the EigenLayer protocol.

**Why Participate?**

Users interested in EigenLayer can familiarize themselves with the EigenLayer restaking module on the testnet, as well as the associated interfaces. While the Stage 1 testnet is mainly intended for stakers, other ecosystem participants, such as operators and builders, can learn more about EigenLayer, as well as understand the basic structure of the codebase. In addition, the testnet offers teams building staking functionality the opportunity to understand various ways to integrate EigenLayer restaking into their protocols.

An important consideration for the phased launch is that it enables the community to start engaging in stewarding the protocol and code design. Please join us in [Discord](https://discord.gg/eigenlayer) to discuss.

How to participate:

- Check out the [testnet](http://goerli.eigenlayer.xyz/)
- Read through the [documentation](https://docs.eigenlayer.xyz/)
- Look through the codebase and provide comments and feedback on [GitHub](https://github.com/Layr-Labs/eigenlayer-contracts)
- Provide feedback and discuss ideas in [Discord](https://discord.com/eigenlayer)
- Participate in research discussions on the [Forum](https://forum.eigenlayer.xyz/)
- Follow @eigenlayer on [Twitter](https://twitter.com/eigenlayer)

We are thrilled to be taking these first steps towards the launch of the EigenLayer protocol, and welcome the community to join in building the restaking collective, with the goal of creating a platform that maximizes the surface area of permissionless innovation.

Looking forward to building!

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/#/portal)

Subscribe# 404

Page not found

[Go to the front page →](https://www.blog.eigenlayer.xyz/)

SubscribeA collection of 3 posts


[![AI Beyond the Black Box: Inference Labs is Making Verifiable, Decentralized AI a Reality with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/AVS_Spotlight_Inference_BlogPost-Header.jpg)](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[Inference Labs is making AI verifiable on-chain with their Zero-Knowledge Verified Inference Network. By leveraging EigenLayer's security, they've created a system that makes dishonest behavior unprofitable while reducing proving times by 76%.](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[![The Trust Layer for AI Agents: How Ungate Wukong Leverages EigenLayer for Trust-Minimized Autonomous Agents](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/AVS_Spotlight_Ungate_BlogPost-Header.png)](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[What if you could know with certainty that an AI is acting independently? Ungate and EigenLayer build the trust layer for AI, where autonomous agents don't just claim independence—they prove it cryptographically through verifiable execution.](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[![Scaling to $1B in Delegated Assets: How Pier Two Unlocked Growth with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[From early launch partner to securing over $1B in delegated ETH and EIGEN, Pier Two's success demonstrates how institutional operators can scale efficiently with EigenLayer's restaking framework to build market-leading positions.](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

SubscribeA collection of 1 post


[![Coming Soon: AVS Rewards and EIGEN Programmatic Incentives](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/b-1.png)](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[Summary\\
\\
\\* Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.\\
\\* At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.\\
\\* These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

SubscribeA collection of 15 posts


[![Introducing Updated EigenDA Pricing: Unlocking Greater Value and Accessibility](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Pricing-Update.png)](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[EigenDA’s mission is to scale the decentralized world by making reliable, scalable, and secure data availability (‘DA’) abundant. In pursuit of this goal, and enabled by the scalability of its design, EigenDA strives to be the most price-performant DA solution.\\
\\
Today, we are pleased to announce a 10x reduction](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[![Introducing EigenDA Base Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Base-Rewards-5.png)](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[Last week EigenLayer experienced a major upgrade. The Rewards protocol, the onchain system that enables AVSs to distribute rewards to their stakers and operators, went live on mainnet on August 6th. The Rewards protocol upgrade grants AVSs the ability to begin rewarding stakers and operators for their past, present, and](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[![EigenDA Dual Quorum and Production Traffic Announcement](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/Screenshot-2024-05-20-at-12.53.32-PM.png)](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[We are supporting rollup production traffic on EigenDA mainnet available immediately. Anyone can now deploy their rollup and be whitelisted on our free tier. Interested rollup customers should fill out the EigenDA Contact Form in order to be approved for the initial free-tier usage phase. Please provide the ETH address](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[![Onboarding Rollups to EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/eigenda-Blog-post---1.png)](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[Following the EigenDA mainnet launch announcement on April 9, we are pleased to announce that rollups can now onboard onto EigenDA mainnet for test traffic. We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[![Accelerate Rollup Deployment with EigenDA's RaaS Marketplace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/b2--1-.png)](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[Teams are launching high performance, low cost rollups and rollup services on EigenDA. In this post we’ll take a look at some of them and learn how you can get started building your own rollup.\\
\\
Rollups as a Service (RaaS) provide everything you need to build, customize, and deploy](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[![Mainnet Launch Announcement: EigenLayer ∞ EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/lightbox_launch_0409-3.jpeg)](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[The Start of Infinite Sum Games\\
\\
Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[![EigenLayer Holesky Testnet Launch + Dual Quorum Support for EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/Twitter---22.png)](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[The EigenLayer and EigenDA Holesky Testnet is now up and running. This marks an important milestone for the EigenLayer ecosystem, and we're keen for operators, stakers and rollup developers to continue testing on Holesky as we gear up for the upcoming mainnet launch. As a reminder: points are](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[![With New Arbitrum Orbit Integration, EigenDA and AltLayer Bring Horizontal Scalability to the Ethereum Ecosystem](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/Untitled.png)](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[Feb 2, 2024 – EigenLabs, along with Offchain Labs and AltLayer, today announced EigenDA support for Arbitrum Orbit chains, bringing scalability to the Ethereum ecosystem without sacrificing on security. The integration offers developers the ability to build EigenDA-based Orbit rollups that bridge from Arbitrum One, Arbitrum Nova, and Ethereum, and boast](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[![Polymer x EigenDA: Open, Neutral Interoperability for Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/BlogPost-Partnership-polymer.jpg)](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[EigenDA is a data availability store made by EigenLabs and built on top of EigenLayer. Currently live on the Goerli testnet, EigenDA is on a path to launch on mainnet later this year, with support for up to 10 MB/s in write throughput. As a rollup developer, if you&](https://www.blog.eigenlayer.xyz/polymer-eigenda/)

[![Announcing EigenDA x OP Stack Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/v2--1-.png)](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[We built EigenDA to help rollups scale, and thus help Ethereum scale. Rollups need somewhere decentralized, scalable, and secure to post transaction data so they can scale their systems to the next billion users. To achieve this mission we intend to make it as easy as possible for the next](https://www.blog.eigenlayer.xyz/announcing-eigenda-x-op-stack-support/)

[![Versatus x EigenDA: The First Stateless Rollup](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/BlogPost-Partnership-versatus.png)](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[EigenDA launched on testnet a few weeks ago, alongside the EigenDA Launch Partner Program featuring eight rollup infrastructure providers: AltLayer, Caldera, Celo, Layer N, Mantle, Movement, Polymer Labs, and Versatus.\\
\\
We kicked off a series of case studies on these launch partners starting with Layer N x EigenDA: A Case](https://www.blog.eigenlayer.xyz/eigenda-versatus/)

[![Layer N x EigenDA: A Case Study in Hyperscale DA for Finance](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/BlogPost-Partnership-02--2-.png)](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[Now that EigenDA is live on testnet, we're proud to announce the first participants in the EigenDA Launch Partner Program. The first eight rollup infrastructure providers who are actively working to deploy EigenDA as a data availability option for their end users include: AltLayer, Caldera, Celo, Layer N,](https://www.blog.eigenlayer.xyz/eigenda-layer-n/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![Intro to EigenDA: Hyperscale Data Availability for Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/09/EigenDA_Ghost_Header_IntroToEigenDA_1200x675_01.png)](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

[If you're interested in integrating your rollup with EigenDA, please fill out the EigenDA questionnaire!\\
\\
EigenDA is a secure, high throughput, and decentralized data availability (DA) service built on top of Ethereum using the EigenLayer restaking primitive. Developed by EigenLabs, EigenDA will be the first actively validated service](https://www.blog.eigenlayer.xyz/intro-to-eigenda-hyperscale-data-availability-for-rollups/)

SubscribeA collection of 38 posts


[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

SubscribeA collection of 38 posts


[![Slashing on Mainnet is Coming Soon - What You Need to Know](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog-Header_slashing_mainnet.jpg)](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[Slashing goes live on EigenLayer mainnet April 17th. Get essential details on what slashing is, the implementation timeline, key reminders for all users, and specific guidance for AVSs, Operators, and Stakers in this comprehensive update.](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[![The Future of EigenLayer Testing: New & Improved Testnets & Tooling Coming Soon](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[Eigen Labs expands EigenLayer protocol testing capabilities with multiple new testnets and developer tools. Explore Hoodi deployment, improved CLI, Self Slasher AVS, and local devnet toolchains—empowering AVS builders with robust, versatile environments for seamless integration.](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[![AI Beyond the Black Box: Inference Labs is Making Verifiable, Decentralized AI a Reality with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/AVS_Spotlight_Inference_BlogPost-Header.jpg)](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[Inference Labs is making AI verifiable on-chain with their Zero-Knowledge Verified Inference Network. By leveraging EigenLayer's security, they've created a system that makes dishonest behavior unprofitable while reducing proving times by 76%.](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[![EigenLayer Update: Holesky Network Instability and Upcoming Sepolia Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--3-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[Due to Holesky testnet instability following Ethereum's Pectra Upgrade, EigenLayer will launch on Sepolia Network (March 10) while maintaining Holesky support. EigenDA targeting Sepolia availability by late April.](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[![The Trust Layer for AI Agents: How Ungate Wukong Leverages EigenLayer for Trust-Minimized Autonomous Agents](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/AVS_Spotlight_Ungate_BlogPost-Header.png)](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[What if you could know with certainty that an AI is acting independently? Ungate and EigenLayer build the trust layer for AI, where autonomous agents don't just claim independence—they prove it cryptographically through verifiable execution.](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[![Redefining AVS: From Actively Validated to Autonomous Verifiable Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Header.png)](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[EigenLayer redefines AVS as Autonomous Verifiable Services, reflecting a powerful shift from how these services are validated to what they truly represent: systems that are self-sustaining, verifiable, and built for decentralized ecosystems.](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[![Scaling to $1B in Delegated Assets: How Pier Two Unlocked Growth with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[From early launch partner to securing over $1B in delegated ETH and EIGEN, Pier Two's success demonstrates how institutional operators can scale efficiently with EigenLayer's restaking framework to build market-leading positions.](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[![EigenLayer at ETHDenver 2025: Complete Event Guide](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/EigenLayer-at-ETHDenver_Blog-Header.png)](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[Your comprehensive guide to all things Eigen at ETHDenver 2025](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[![Introducing Verifiable Agents on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient.png)](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[The “Level 1 Agent” is a standardized way for agents to integrate with verifiable tools by using AVSs.](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[![EigenLayer 2024 Year in Review: Building the Future of Open Innovation](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/01/EigenLayer-2024-Year-in-Review.png)](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[2024 was a monumental year for EigenLayer.\\
\\
Since launching our first staking contracts in June 2023, and following the full mainnet deployment of the EigenLayer protocol in April 2024, we’ve made great strides toward our vision of building coordination engines for driving open innovation. \\
\\
We launched the EigenLayer protocol,](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[![Introducing: Slashing](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Slashing---Testnet.png)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[We are excited to introduce Slashing, a proposed protocol upgrade detailed in ELIP-002: Slashing, and the next significant step in the evolution of the EigenLayer protocol. This powerful upgrade introduces slashing, a critical tool for AVSs to enforce cryptoeconomic commitments.\\
\\
This proposed upgrade is the second EigenLayer Improvement Proposal (ELIP)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[![Introducing: Rewards v2](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Rewards-v2-2.png)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[We are excited to announce the arrival of the Rewards v2 protocol upgrade on Mainnet. It is designed to bring greater flexibility, efficiency, and customization to rewards within the EigenLayer ecosystem. This upgrade is also the first EigenLayer Improvement Proposal (ELIP), ELIP-001: Rewards v2, using the EigenLayer Governance process (EigenGov)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[![Introducing Programmatic Incentives v1](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Programmatic-Incentives-v1-1.png)](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

[The Eigen Foundation is excited to announce the upcoming Programmatic Incentives v1 release, a new EigenLayer protocol feature, providing programmatic EIGEN rewards to stakers and operators for their active participation in supporting AVSs.\\
\\
What\\
\\
Programmatic Incentives v1 will enable weekly programmatic rewards of newly-minted EIGEN tokens to qualifying stakers and](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

[![Introducing the EigenLayer Security Model: A Novel Approach to Operating and Securing Decentralized Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Security-Model-1.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[Eigen Labs is excited to introduce the EigenLayer Security Model, an evolution of our previous model, in preparation for the upcoming ‘Slashing’ protocol feature release.\\
\\
This blog post is a simple description of how EigenLayer empowers AVSs to operate more efficiently and create economically aligned incentives using three foundational concepts:](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[![Introducing the EigenPod Upgrade](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Introducing-EigenPod-Upgrade.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[Eigen Labs is pleased to announce the EigenPod upgrade, a major update to the EigenLayer protocol, making native ETH restaking on EigenLayer easier and more rewarding.\\
\\
What\\
\\
The EigenPod upgrade introduces a novel balance checkpointing system for managing Ethereum validator and EigenPod balances.\\
\\
When\\
\\
All EigenPods on mainnet have been](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[![Community Update: Airdrops and the EigenLayer Ecosystem Network](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/communityupdate.png)](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[CoinDesk published an article on airdrops in our ecosystem and Eigen Labs employee participation in those airdrops. We’ve written a community update on what happened, why it happened, and steps we’ve taken. \\
\\
We want to make clear that we have no knowledge or evidence of any employee at](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[![Coming Soon: Permissionless Token Support on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/comingsoonpermissionless.png)](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[Eigen Labs is proud to introduce Permissionless Token Support, an upcoming update, to the EigenLayer protocol. This feature will enable any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[![Coming Soon: AVS Rewards and EIGEN Programmatic Incentives](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/b-1.png)](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[Summary\\
\\
\\* Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.\\
\\* At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.\\
\\* These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[![EIGEN: The Universal Intersubjective Work Token](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Exploring-EIGEN-Banner.png)](https://www.blog.eigenlayer.xyz/eigen/)

[Over the past few years, we at Eigen Labs have been developing a platform for advancing the concept of open, verifiable digital commons. This blog post summarizes the intersubjective forking protocol enabled by the EIGEN token. We will break down the significance of EIGEN, its core ideas, its high-level implementation,](https://www.blog.eigenlayer.xyz/eigen/)

[![Unpausing Restaking Deposits on April 16th!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Ghost_05.png)](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[We're excited to announce the culmination of our phased rollout with the removal of all Liquid Staking Token (LST) caps and unpausing deposits, effective April 16th, 2024, at 9:00 AM PST. This milestone marks the next chapter in EigenLayer's journey, fostering a wide-open and dynamic](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[![EigenLayer AVS Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/medium-banner_option4.png)](https://www.blog.eigenlayer.xyz/avs-launch/)

[EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs\\
\\
The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s MACH restaked rollups with Xterio being](https://www.blog.eigenlayer.xyz/avs-launch/)

[![Mainnet Launch Announcement: EigenLayer ∞ EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/lightbox_launch_0409-3.jpeg)](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[The Start of Infinite Sum Games\\
\\
Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[![On Liquid Restaking: Risks & Considerations](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/2024-03-21-13.07.06.jpg)](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[In recent months, liquid restaking protocols and liquid restaking tokens (LRTs) have seen tremendous growth, and are beginning to become systemically important in the EigenLayer ecosystem. There are many different flavors of LRT, each with different values, features, and offerings. All of these LRT projects are independent of the underlying](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[![EigenLayer Mainnet: Preparation for launch!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/x--2-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[We're thrilled to announce the next chapter in our journey towards a secure and successful EigenLayer mainnet launch! Our primary focus remains a smooth launch that prioritizes both security and performance. To achieve this, we're introducing a multi-phased approach starting now and ongoing for the next](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[![Announcement: Update on Upcoming LST Additions and Restaking Unpause](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Update-on-Upcoming-1.png)](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[In preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise. \\
\\
Several staking parameters are set to be adjusted:\\
\\
\\* First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.\\
\\* Second, removal of the 200k](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/)

[![Introducing New LSTs and Unpausing Restaking Caps!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/EigenLayer_adding_lsts_raising_caps_jan24_Twitter_1920x1080_V1.jpg)](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[We're excited to bring in 2024 by adding restaking support for three new Liquid Staking Tokens (LSTs): Frax Ether (sfrxETH), Mantle Staked Ether (mETH), and Liquid Staked Ether (LsETH). In line with this, EigenLayer will reopen for restaking at the existing cap of 200k ETH for each LST.](https://www.blog.eigenlayer.xyz/introducing-new-lsts-and-unpausing-restaking-caps/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Adding LSTs to EigenLayer & Raising the Caps](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/EigenLayer_adding_lsts_raising_caps_Twitter_1920x1080_V1.png)](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[We are excited to share that the six LSTs, garnering over 15k ETH in support during the LST election contest, will imminently join the EigenLayer protocol as restaking assets, accompanied by an increase in restaking caps for current LSTs.](https://www.blog.eigenlayer.xyz/adding-lsts-to-eigenlayer-raising-the-caps/)

[![Launch of the Stage 2 Testnet: EigenLayer & EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/11/Screen-Shot-2023-11-14-at-11.41.19-AM.png)](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[We’re thrilled to announce the launch of a new Goerli testnet experience for EigenLayer and EigenDA, representing Stage 2 of development towards the full EigenLayer vision.\\
\\
In June we launched Stage 1, introducing restaking on mainnet Ethereum. Through a series of security-focused TVL cap increases, 170,000 ETH has](https://www.blog.eigenlayer.xyz/launch-of-the-stage-2-testnet-eigenlayer-eigenda/)

[![EigenLayer LST Addition Contest in Partnership with JokeRace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/EigenLayer_JokeRace-_Ghost_1200x675_V33-1.png)](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[In collaboration with JokeRace, we're excited to announce an upcoming contest, starting October 20, that will help select the next LST tokens to be listed as restaking assets on EigenLayer!\\
\\
EigenLayer's Vision: Our vision for EigenLayer is audacious - we envision a future where EigenLayer thrives](https://www.blog.eigenlayer.xyz/eigenlayer-lst-addition-contest-in-partnership-with-jokerace/)

[![Twelve Early Projects Building on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_EarlyProjects_04.png)](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[EigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/)

[![EigenLayer Announces New Cap Increase for Liquid Staking Tokens (LSTs)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[On August 22 at 7am Pacific Time, EigenLayer will take another step forward in its evolution. Restaking opportunities will be expanded by raising the caps on Liquid Staking Tokens (LSTs), including stETH, rETH, and cbETH. After the caps are lifted, users will be able to deposit any one of these](https://www.blog.eigenlayer.xyz/eigenlayer-announces-new-cap-increase-for-liquid-staking-tokens-lsts/)

[![EigenLayer to Increase Restaking Capacity for LST and Native Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/07/Twitter-post---42.jpg)](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[EigenLayer will expand restaking opportunities for users by increasing caps on Liquid Staking Tokens (LSTs). This upgrade enhances access to restaking while upholding asset safety and security.\\
\\
To modify the restaking limits, a protocol parameter change must undergo approval from the Multisignature Governance system. As of today, June 30, the](https://www.blog.eigenlayer.xyz/eigenlayer-to-increase-restaking-capacity-for-lst-and-native-restaking/)

[![EigenLayer Stage 1 Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/06/EigenLayer_Ghost_DeployedToEtheremMainnet_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[Overview of the Guarded Launch, Protocol Security, and Governance\\
\\
EigenLayer restaking has been deployed on Ethereum mainnet at the following contract addresses:\\
\\
\\* StrategyManager: 0x858646372CC42E1A627fcE94aa7A7033e7CF075A\\
\\* EigenPodManager: 0x91E677b07F7AF907ec9a428aafA9fc14a0d3A338\\
\\
To restake on mainnet, visit the EigenLayer app at: app.eigenlayer.xyz\\
\\
For this Stage 1 launch, the functionality aligns with what has been](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/)

[![EigenLayer Mainnet Launch: Benefits of Early Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/05/Twitter-post---41--1-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[Tldr;\\
\\
Interested in early restaking opportunities? Please fill out this Indication of Interest form to get in touch with the EigenLabs team.\\
\\
Introduction:\\
\\
EigenLayer Mainnet is set to launch soon, following the successful completion of the Stage 1 Testnet for restaking functionality. EigenLayer aspires to make a positive impact on](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-launch-benefits-of-early-restaking-2/)

[![The Shanghai/Capella Upgrade to Ethereum](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/Capella-Upgrade-to-Ethereum_1920x1080_02.png)](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[What Stakers Need to Consider Before Restaking on EigenLayer\\
\\
The Shanghai/Capella (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/)

[![The EigenLayer Stage 1 Testnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/04/EigenLayer_Ghost_Header_Announcing_1920x1080_03.png)](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

[Today we are excited to announce the release of the testnet for the first stage of the EigenLayer protocol. This marks an important milestone on the road toward the future of cryptoeconomic security, by creating a free market for decentralized trust. We can’t wait for restakers, validators, and builders](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/)

SubscribeA collection of 5 posts


[![Intro to Slashing on EigenLayer: AVS Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_AVS-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[EigenLayer's slashing on mainnet arrives April 17th, enabling AVSs to set custom conditions, manage operators through Operator Sets, and implement Unique Stake Allocation—creating stronger security guarantees with targeted accountability.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[![Intro to Slashing on EigenLayer: Stakers' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_stakers-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[Slashing arrives on EigenLayer mainnet April 17th. Learn how it impacts Stakers through opt-in requirements, allocation timelines, and monitoring responsibilities—plus why this feature completion enhances the risk/reward ecosystem for delegators.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[![Intro to Slashing on EigenLayer: Operators' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_operators-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[EigenLayer's slashing launches April 17th, giving Operators control through opt-in participation, Operator Sets, and Unique Stake Allocation—isolating risks while creating new reward opportunities aligned with your specific risk profile.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[![EigenLayer Update: Holesky Network Instability and Upcoming Sepolia Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--3-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[Due to Holesky testnet instability following Ethereum's Pectra Upgrade, EigenLayer will launch on Sepolia Network (March 10) while maintaining Holesky support. EigenDA targeting Sepolia availability by late April.](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[![Principles and Best Practices to Design Solidity Events in Ethereum and EVM](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/mainheader.png)](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[Solidity events are a crucial feature of Ethereum and EVM blockchains, with a vast number of use cases within the ecosystem. Primary use cases, for example, include but not limited to\\
\\
\\* Logging: where events provide a mechanism to log critical actions and state changes inside smart contract, for track contract](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

SubscribeA collection of 1 post


[![EigenLayer Update: Holesky Network Instability and Upcoming Sepolia Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--3-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[Due to Holesky testnet instability following Ethereum's Pectra Upgrade, EigenLayer will launch on Sepolia Network (March 10) while maintaining Holesky support. EigenDA targeting Sepolia availability by late April.](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

SubscribeA collection of 22 posts


[![Redefining AVS: From Actively Validated to Autonomous Verifiable Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Header.png)](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[EigenLayer redefines AVS as Autonomous Verifiable Services, reflecting a powerful shift from how these services are validated to what they truly represent: systems that are self-sustaining, verifiable, and built for decentralized ecosystems.](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[![Introducing Verifiable Agents on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient.png)](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[The “Level 1 Agent” is a standardized way for agents to integrate with verifiable tools by using AVSs.](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[![Fragmentation to Fusion: Intention is All You Need (Part 2)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents hold the potential to transform how we interact with Ethereum, yet today, they risk creating fragmented “walled gardens” where each intent-based application exists in](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[![Intention is All You Need](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents are transforming how we think about interactions on Ethereum.\\
\\
Instead of rigid transaction paths, intents open up a world where users declare their desired](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[![EigenLayer x LayerZero: The CryptoEconomic DVN Framework](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/LAYER-ZERO.png)](https://www.blog.eigenlayer.xyz/dvn/)

[Eigen Labs, in partnership with LayerZero Labs, is introducing a framework for CryptoEconomic Decentralized Verifier Networks (DVNs). Eigen Labs chose LayerZero because it is one of the most battle-tested protocols in crypto, handling millions of messages and securing billions for apps.\\
\\
As the first step of this integration, we are](https://www.blog.eigenlayer.xyz/dvn/)

[![Celebrating Commit-Boost’s Journey Towards Mainnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Commitboost.png)](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[Eigen Labs is excited to announce that Commit-Boost, an open-source public good that we have proudly supported since its inception, is moving towards audit and production on the Ethereum mainnet. This milestone, and the community now behind it, marks a significant step forward in ensuring the healthy growth of the](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[![Intelligent DeFi](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Intelligent-DeFi-B.png)](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[Thanks to Karthik, Ismael from Lagrange, Nikhil from Aethos, Soubhik, Gautham, and Brandon for reviewing drafts of this piece.\\
\\
Ethereum gave birth to DeFi with the launch of Maker in December 2017. Uniswap and Compound launched soon after, forming an economy around ETH and ERC20s. Since then, we’ve witnessed](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[![Request for AVS: Uniswap v4 Hooks](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/1200x630.png)](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[Occasionally, you hear an idea so exciting that you see the future with utter clarity. At Eigen Labs, we’re excited to work with exceptional builders and hear their special insights about the world on a daily basis. We want to share some of ours with you. We’re starting](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[![EigenLayer AVS Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/medium-banner_option4.png)](https://www.blog.eigenlayer.xyz/avs-launch/)

[EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs\\
\\
The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s MACH restaked rollups with Xterio being](https://www.blog.eigenlayer.xyz/avs-launch/)

[![Fhenix: FHE Coprocessor on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/image1.png)](https://www.blog.eigenlayer.xyz/fhenix/)

[Fhenix and EigenLayer Join Forces to Pioneer FHE Coprocessors, Revolutionizing Onchain Confidentiality on Ethereum\\
\\
We are excited that FHE Coprocessor will be building on EigenLayer and to announce the development of FHE-based coprocessors in collaboration with Fhenix.\\
\\
FHE coprocessors are secured by Fhenix’s optimistic FHE rollup infrastructure and EigenLayer’](https://www.blog.eigenlayer.xyz/fhenix/)

[![Ethos: Powering the Convergence Era of Blockchains](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/ethosxeigen.png)](https://www.blog.eigenlayer.xyz/ethos/)

[Blockchains are entering an era of convergence, characterized by the dissolution of boundaries between different network architectures. Cosmos started as a network of appchains, with each chain independently establishing its validator set and trust protocol using native tokens. Ethereum, on the other hand, chose a shared security approach where every](https://www.blog.eigenlayer.xyz/ethos/)

[![Ritual ♾️ EigenLayer: AI × Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[Summary\\
\\
The worlds of artificial intelligence and onchain protocols are increasingly intersecting as permissionless protocols look to unlock new customer behavior around ownership and markets by utilizing AI models. We’ve witnessed how in offchain settings, AI models could dramatically improve on the status quo of problem solving across various](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[![Inco: Building an Universal Confidential Computing L1 on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/----5-.png)](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[One of the open challenges in the blockchain industry is achieving trustless confidentiality. The inherent transparent nature of public blockchains prevents the development of applications requiring on-chain confidentiality across gaming, decentralized finance (DeFi), governance, and identity without relying on a trusted third party.\\
\\
Current approaches to providing privacy on the](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[![Cosmos. Ethereum. EigenLayer.](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/01/Screenshot-2024-01-09-at-7.49.43-AM.png)](https://www.blog.eigenlayer.xyz/cosmos/)

[By connecting Ethereum and Cosmos, EigenLayer will bring in a new wave of innovations.](https://www.blog.eigenlayer.xyz/cosmos/)

[![Introducing Restaked Rollups](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/12/restakedrollups-banner.png)](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[Restaked rollups: A New AVS Category\\
Proposed by Altlayer and supported on EigenLayer\\
\\
Ethereum proposed the rollup-centric roadmap in order to perform offchain execution and make credible proofs of correctness, via validity or fault proofs. Rollups need several auxiliary services to attain their full functionality.\\
\\
1\. Decentralized sequencers to obtain](https://www.blog.eigenlayer.xyz/restaked-rollups/)

[![Dual Staking: secure a PoS network with two tokens](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/dualstaking.png)](https://www.blog.eigenlayer.xyz/dual-staking/)

[One of the most powerful features of EigenLayer is the notion of dual staking. In this article, we will discuss what dual staking is, how it increases the robustness and decentralization of any PoS network, how it mitigates the death spiral problem with network token volatility, and how these networks](https://www.blog.eigenlayer.xyz/dual-staking/)

[![You Could've Invented EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/i-made-eigenlayer-copy-2.jpg)](https://www.blog.eigenlayer.xyz/ycie/)

[In this blog post, we will take you through the evolution of the protocol, by covering how EigenLayer's architecture emerged from the initial concept.](https://www.blog.eigenlayer.xyz/ycie/)

[![Announcing EigenLayer Research Fellowship](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/ERFwhite.png)](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[Update: The EigenLayer Research Fellowship (ERF) structure has been updated on October 25th, 2023, particularly in selection criteria and program dates.\\
\\
We believe EigenLayer is about to kickstart a new era of secure innovative infrastructure applications that will transform how blockchain applications operate. We’ve been diving deep into these](https://www.blog.eigenlayer.xyz/eigenlayer-research-fellowship/)

[![The EigenLayer Universe: Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/Eigen-Universe-3.png)](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[EigenLayer empowers builders to develop innovative distributed systems without worrying about how to build the underlying trust networks for these systems. We call these distributed systems AVSs - actively validated services. We have categorized AVSs into 5 types:\\
\\
1\. Rollup Services: augmenting the Ethereum rollup ecosystem with services that inherit](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)

[![The Three Pillars of Programmable Trust: The EigenLayer End Game](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/10/image--1--1.png)](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[Thanks to Alex Obadia, Austin Griffith, Dan Elitzer, David Phelps, Jon Charbonneau, Soham Zemse, and Waylon Jepsen from the community for reviewing and giving feedback. \\
\\
Today, if any developer wants to build a smart contract protocol such as a DEX, a lending protocol, etc., on Ethereum, they can inherit security](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)

[![Censorship Resistance with Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2023/08/EigenLayer_Ghost_Liveness-firstRelay_1200x675_01.png)](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

[MEV-Boost protects Ethereum’s validator set from Maximum Extractable Value’s (MEV) centralization pressures. It is an open-source project designed by the Flashbots team. MEV-Boost’s relays simplify the interaction between builders and proposers in this system, streamlining the system and eliminating the need for complex cryptography.\\
\\
There are however](https://www.blog.eigenlayer.xyz/censorship-resistance-with-restaking/)

SubscribeAs Eigen Labs continues to iterate and release new and improved versions of the EigenLayer protocol, our approach to testing also requires iteration. We are quickly approaching [50 AVSs](https://avsecosystem.eigenlayer.xyz/#:~:text=Mainnet-,Testnet,-A%20comprehensive%20list) on the EigenLayer test networks with hundreds more in development. These AVSs need to be able to comprehensively test their services alongside Operators. Eigen Labs has created a more robust testing strategy by launching new testnets and tooling improvements to meet these needs.

## Testnet Improvements

### Immediate Deployment to Hoodi Ethereum Testnet

The Ethereum community recently introduced the Hoodi testnet to address [challenges encountered with the Holesky testnet](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/) during the testing of the upcoming Pectra upgrade. Hoodi was designed as its replacement by providing an environment focused on staking-related protocols, with an open beacon chain validator set. You can learn about Hoodi [here](https://blog.ethereum.org/2025/03/18/hoodi-holesky). EigenLayer will be deployed on the newly launched Hoodi Ethereum testnet, targeting early April.

This deployment will allow builders to:

- Test the protocol end-to-end, including beacon chain / EigenPod validator integrations.
- Replace Holesky in their testing process as we shift infrastructure over.

This replacement is to be known as **staging-hoodi**.

### Long-Term Testnet Support

To cater to users’ diverse testing needs, we are establishing a multi-testnet approach. The networks Eigen Labs will support include:

#### EigenLayer Sepolia Testnet:

- **Namespace:** testnet-sepolia
- **Purpose**: Facilitate rapid development iterations and integration testing for AVS customers.
- **Benefits**: Provide a streamlined environment for quick testing cycles, enabling efficient troubleshooting and accelerated development timelines for AVSs and integrating customers. Sepolia is home to many existing apps and rollups, with an EigenLayer deployment helping test AVSs integrations.

#### EigenLayer Hoodi Testnet:

- **Namespace:** testnet-hoodi
- **Purpose**: Provide a platform for AVS developers, Liquid Restaking Tokens (LRTs), Operators, and integrators to test upcoming features of the EigenLayer protocol and end-to-end interactions between Operators, AVSs, and Ethereum. This includes new or upcoming EigenPod mechanics (e.g., proposer commitments or validator consolidations), beacon chain operations, rewards, and more.
- **Advantages**: Allows early access to new functionalities, which enables proactive issue identification and fosters a collaborative development atmosphere.

#### EigenLayer Staging Environment:

- **Namespace:** staging-hoodi
- **Purpose**: For AVSs to signal readiness of code and protocol. For Operators and AVSs to test production interactions. For Operators to test new AVSs they may want to run on Mainnet risk-free.
- **Features**: Offers an environment that mirrors the EigenLayer protocol on mainnet, ensuring that tests accurately reflect live conditions and the reliability of deployments.

#### EigenLayer Holesky Testnet

Eigen Labs will continue supporting Holesky Testnet until Q3 2025\*, when the testnet will sunset. Eigen Labs wants to ensure a smooth transition for its users with their testing environments, and by continuing to support Holesky, users can transition to the new testnets in a reasonable amount of time.

Here’s an overview of the upgrade pathing we plan to support from when a release is cut:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdWcp1MA0sNNL2LrrmdQvlUAo2q9gvguNUYh1RHsO15RIwYR829Mmo2Y4EbA-XDMjKR267m3BnprZlZKdOQlzfxincqcNqzHGuyfU7NIAOFp1PbnGRooh7_nMe2J3RgmnXJut1Y9g?key=4XDoM6NK4GJxBgjvQ35StMzJ)

The contract addresses will be provided in the [eigenlayer-contracts-metadata repo](https://github.com/Layr-Labs/eigenlayer-contracts-zeus-metadata), when available for each network.

Additionally, the Eigen Labs team has been provided HoodiETH and SepoliaETH. We can provide testnetETH on an individual basis.

_\*testnet-holesky will sunset in Q3; the Ethereum testnet itself reaches end of life in September_

## Tooling Improvements

### Improved CLI

We know multiple testnets may create overhead in management and infrastructure, so we will ship some additional tools to make this easier. Eigen Labs is launching new tooling to improve the user experience of building on EigenLayer. The [EigenLayer-CLI](https://github.com/Layr-Labs/eigenlayer-cli) helps spin up new Operators easily. AVSs can spin up Operator nodes themselves to test interactions in their Operator Sets, distribute tasks, and quickly iterate on business logic.

[→ **Learn more here**](https://docs.eigenlayer.xyz/operators/howto/operator-installation)

### Self Slasher

We will launch a Self-Slashing AVS on all supported test networks for both AVSs and Operators. This AVS will consume Operator meta-data and allow an Operator to allocate stake and slash itself, with user-specified parameters. This is intended to help participants trigger slashings and observe the impacts within the protocol (and out).

→ **Look out soon for the latest repository to get started using Self-Slashing.**

### Local Devnet Toolchains

AVS Devnet will transform how developers build, test, and deploy AVSs on EigenLayer. Powered by Kurtosis, this versatile CLI tool enables rapid setup of customizable local Ethereum devnets, complete with integrated consensus and execution clients, a pre-configured Blockscout explorer, and simulation-ready Operator accounts with keys. With AVS Devnet, developers gain the flexibility and efficiency to streamline local testing, reduce setup complexity, and accelerate innovation. This powerful addition to the EigenLayer testing toolkit ensures a seamless development experience from prototyping through deployment, perfectly complementing the upcoming enhancements to our testnet infrastructure.

→ **Check out the open source** [**AVS Devnet repository here**](https://github.com/Layr-Labs/avs-devnet) **to get started.**

## Get in Touch

Eigen Labs remains dedicated to providing robust and versatile testing environments for the EigenLayer protocol that cater to developers' needs. We hope the additional environments enhance the developer experience.

→ **Please contact us with any feedback and questions by joining our** [**Discord**](https://discord.com/invite/eigenlayer) **and** [**Forum**](https://forum.eigenlayer.xyz/) **.**

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/#/portal)_What Stakers Need to Consider Before Restaking on EigenLayer_

The [Shanghai/Capella](https://blog.ethereum.org/2023/03/28/shapella-mainnet-announcement) (Shapella) upgrade brings significant changes to the Ethereum ecosystem, including enabling validator withdrawals. As EigenLayer is set to launch on mainnet soon, stakers should carefully assess their withdrawal strategy in order to maximize future opportunities for restaking and minimize risks. This article discusses withdrawal credentials and their implications for participating in EigenLayer, and provides guidance for stakers considering restaking when EigenLayer launches.

**Understanding the Shapella Upgrade and Withdrawal Credentials**

Shanghai and Capella refer to the recent Ethereum hard fork, affecting the execution and consensus layers, respectively.

To participate in Ethereum's beacon chain and contribute to the network's consensus mechanism, validators must deposit 32 Ether per validator. The recent Shapella upgrade now allows validators to withdraw their deposited Ether, effectively stopping their participation in the network and unlocking their 32 Ether. In addition, validators now receive their earned rewards in excess of 32 Ether automatically, known as partial withdrawals. Access to withdrawals is managed by a set of withdrawal credentials, which begin with either 0x00 or 0x01 prefix.

_Check your prefix using [_ethdo_](https://github.com/wealdtech/ethdo)._

If you're planning to restake on EigenLayer, it's important to note that the two sets of credentials (0x00 and 0x01) have different paths for accessing native restaking on the platform when it launches. If you have 0x00-prefixed credentials, you can only migrate [once](https://notes.ethereum.org/@launchpad/withdrawals-faq#Q-What-are-the-two-types-of-withdrawals) to a 0x01-prefixed credential. So, if you update your credentials before considering restaking on EigenLayer, you'll have to go through the withdrawal queue to restake later.

This article aims to help stakers avoid pitfalls and make informed decisions when planning to restake their ETH.

**Stakers Preparing for EigenLayer**

EigenLayer enables ETH validators to restake their ETH via a smart contract called an EigenPod (see [docs](https://docs.eigenlayer.xyz/overview/readme) for more details), and permissionlessly opt into new staking opportunities for projects to be launched on EigenLayer and contribute towards accelerating open innovation.

EigenLayer recently launched its [Stage 1 Testnet](https://www.blog.eigenlayer.xyz/stage-1-testnet-announcement/) and plans to enable restaking on mainnet in the near future. Specific launch dates will be announced as they become available. For Ethereum stakers who wish to participate in EigenLayer soon after its mainnet launch:

- ETH stakers with 0x00 credentials who want to restake in EigenLayer may simply wait for EigenLayer's launch and update their credentials to EigenLayer at that time, instead of migrating to a 0x01 address now. Note that validators with 0x00 credentials forgo the ability to withdraw accrued staking rewards until they switch to a 0x01 address (which includes setting to an EigenPod). The option for 0x00 addresses to switch withdrawal credentials has no end date and can be done at any time in the future.

- ETH stakers planning to create new validators can use 0x00 prefixed withdrawal credentials for an easier transition to EigenLayer later. While these validators will forgo the ability to withdraw rewards accrued until they later change their withdrawal credentials to an EigenPod, the amount of rewards accrued before EigenLayer mainnet launch is expected to be small. In order to use the 0x00 prefix, do not set _--execution\_address_ or _--eth1\_withdrawal\_address_ flags [when setting up new validators.](https://github.com/ethereum/staking-deposit-cli#commands)

- If you want to collect your staking rewards accrued to date and plan to restake on EigenLayer, it's recommended to set up new validators with the 0x00 prefix. This will allow for a quicker transition to EigenLayer when it launches.

- If you have validators with 0x01 prefixes that are not pointed to an EigenPod, you will have to go through the withdrawal queue and may potentially miss out on some staking rewards. In this case, you'll need to withdraw your ETH, create a new Ethereum validator with the withdrawn ETH, and set the withdrawal credentials to an EigenPod to restake on EigenLayer.


When setting withdrawal credentials to an EigenPod on EigenLayer, it's important to note that validators can initiate a withdrawal of stake at any time and collect partial withdrawals. However, EigenLayer plans to implement a 7-day withdrawal delay in its own contracts after the stake has been withdrawn from Ethereum's withdrawal queue. This is a security measure during the early stages of the EigenLayer mainnet, to optimize for the safety of assets. It's possible that this mechanic may evolve or be deprecated in the future, so the additional delay on withdrawals may be less of a consideration.

**Conclusion**

EigenLayer offers restakers various benefits, such as securing innovative projects, fostering rapid innovation across the blockchain stack, and contributing to Ethereum's ecosystem growth. By restaking ETH on EigenLayer, stakers will be instrumental in supporting groundbreaking projects built in the Ethereum ecosystem, and access staking opportunities not previously available to them.

The Shanghai/Capella upgrade and the forthcoming EigenLayer protocol launch provide both opportunities and challenges for Ethereum stakers. By understanding the possible changes in withdrawal credentials and their implications for participation in EigenLayer, stakers can make informed decisions about their restaking strategies.

As the Ethereum ecosystem keeps evolving, staying up-to-date with developments and evaluating their impacts on staking operations are essential for maximizing opportunities and mitigating risks.

_Stay up to date with the latest on EigenLayer:_

- Visit the website: [eigenlayer.xyz](https://www.eigenlayer.xyz/)
- Check out the testnet: [goerli.eigenlayer.xyz](https://goerli.eigenlayer.xyz/)
- Read through the documentation: [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz/)
- Meet the community in Discord: [discord.gg/eigenlayer](https://docs.eigenlayer.xyz/overview/readmediscord.gg/eigenlayer)
- Contribute research ideas to the forum: [forum.eigenlayer.xyz](https://forum.eigenlayer.xyz/)
- Follow @eigenlayer on Twitter: [twitter.com/eigenlayer](https://twitter.com/eigenlayer)


## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/the-shanghai-capella-upgrade-to-ethereum-2/#/portal)

Subscribe_Thanks to [_Alex Obadia_](https://twitter.com/ObadiaAlex), [_Austin Griffith_](https://twitter.com/austingriffith), [_Dan Elitzer_](https://twitter.com/delitzer), [_David Phelps_](https://twitter.com/divine_economy), [_Jon Charbonneau_](https://twitter.com/jon_charb), [_Soham Zemse_](https://twitter.com/zemse_), and [_Waylon Jepsen_](https://twitter.com/0xjepsen) from the community for reviewing and giving feedback._

Today, if any developer wants to build a smart contract protocol such as a DEX, a lending protocol, etc., on Ethereum, they can inherit security from Ethereum’s trust network by simply deploying their contracts on top of Ethereum. This has ushered in a Cambrian explosion of innovative smart contract protocols on a secure platform, such as DeFi, NFTs, and more.

However, until now, Ethereum’s security has only extended to smart contract protocols, and it's been very difficult to offer that security to distributed systems like bridges, sequencers, and Data Availability layers. The developer has to go through the cumbersome process of bootstrapping their own trust network to get any security.

The requirement to bootstrap is arguably the most significant obstacle for every new idea to be the most significant obstacle preventing innovation in distributed systems from reaching a similar degree of speed, variety, and scale comparable to that of innovations in smart contract protocols.

This has been one of the biggest limitations of building blockchains and decentralized networks in the past. Until now. Because this is the promise of EigenLayer.

Imagine a world where instead of starting your network from scratch, you could tap into a network with [$45B of value at stake and 850k validators](https://beaconcha.in/charts) at the time of writing, by leveraging the security and trust properties of Ethereum programmatically based on the needs of their systems.

In such a world, developers would not have to worry about bootstrapping a trust network anymore. Instead, they could design systems that pay fees to the programmable network to get the amount of security they want, in order to build innovative distributed systems that can contribute to the creation of a new and better version of the Internet.

In this article, we will briefly review the EigenLayer protocol, then dig into three forms of fundamental trust that developers can use to program their protocols via EigenLayer:

- **Economic Trust**
  - Trust from individuals making commitments and backing their promises with financial stakes
- **Decentralized Trust**
  - Trust from having a decentralized network operating by independent and geographically isolated operators
- **Ethereum Inclusion Trust**
  - Trust that Ethereum validators will construct and include your blocks the way it has made promises to you alongside the consensus software they are running

![](https://lh5.googleusercontent.com/sobVWtJxfwyz3P9fegYtbAJv9sVmHOG59b0ycZckPaSOUOklf1HrN4trgWRddjFQvQGaREnTvGczGPmwKmCps31-wkdNiMi-IyWnSP8ge5fq69gOUlA4YohR-njhFFFjccQvLoamw6AQYhaoQcdEn6w)

_Figure: illustrates the three types of trust you can program via EigenLayer._

## Intro to EigenLayer

EigenLayer is the first generalizable network providing programmable trust from Ethereum. Technically, it is a set of smart contracts on Ethereum as well as a set of off-chain node software that allows solo stakers, staking service providers and LST stakers to opt into running new offchain distributed systems.

Today, Ethereum validators stake ETH in order to make capital-based commitments that they will not deviate from the Ethereum protocol. EigenLayer expands the scope of commitments these stakers can make to include opting in to actively participate and respond to tasks for new use cases and distributed systems. Stakers opt into these systems by running additional node software, and by granting the EigenLayer smart contracts the ability to impose additional slashing conditions (in a programmable manner) on their staked ETH or LSTs as specified by the distributed systems.

We call these new use cases and distributed systems “Actively Validated Services” - AVSs. [Examples of AVSs](https://x.com/eigenlayer/status/1696274774253846804) are fast finality layers, data availability layers, virtual machines, keeper networks, oracle networks, bridges, threshold cryptography schemes, AI inference/training systems, and trusted execution environment committees. We will soon post a blog to uncover these ideas.

A very powerful benefit in EigenLayer is that block proposers can make commitments in addition to Ethereum core consensus software such that new ideas around block construction and ordering and onchain activation can be experimented without in-protocol upgrades.

## Understanding Programmable Trust

There are three distinct models of trust that you can programmatically acquire from Ethereum via EigenLayer: 1) economic trust, 2) decentralized trust, and 3) Ethereum inclusion trust. In this section, we will go through each of them separately. In many cases, your AVS might require a combination of different types of trust. EigenLayer is a platform that allows AVS developers to mix and match these elements to create appropriate trust models.

### 1\. Economic Trust

Economic trust is trust based in capital. These types of assurances are accepted when certain commitments are backed by financial stake at-risk that makes it irrational for a rational economic actor to behave badly. The core feature of economic trust is that the AVS’s validation semantics is objective in nature.

What objectivity means is that if an operator maliciously deviates from the specified validation semantics while responding to the AVS’s task, then an observer can create an **objective onchain proof** to slash the malicious validator.

For example, a chain can adopt reorg-resistance from EigenLayer nodes if the nodes have placed assets behind the sequence it is committing to and be slashed if reorg happened.

Other applications of economic trust include the following:

**Optimistic Claims.** If there is enough value staked on EigenLayer to make a set of objectively verifiable claims, then those claims can be instantly treated as correct, acted upon, and later slashed if violated. Consider **Light Client Bridges** as an example: Run light clients of many other chains off-chain and make claims about the canonical fork of the other chain on Ethereum. The input is acted upon immediately without any latency, and later slashed if wrong.

**Arbitrary Slashing Conditions.** Rollups are usually thought to be limited to fraud proofs for state execution. On EigenLayer, any slashable violation (such as double signing) can be penalized, and thus EigenLayer can be used to extend functionality beyond state execution. As an example, one can build an ETH-staked oracle that is slashed based on a more expensive trusted input (for example, soliciting inputs from highly trusted sources or individuals). Although the second committee is not cryptoeconomic, the power of EigenLayer is to let different stakers only opt-in to trust assumptions that they are comfortable with.

**Latency Transformation.** If we consider Ethereum to be the one computing processor, its clock speed is limited by its finality period of at least 2 epochs (~12mins). If there is enough ETH or LST restaked on EigenLayer, it is possible to increase the clock speed so as to get a significantly larger amount of full economic finality at native network latency. As an example, a primitive where overclocking is needed is superfast finality. With EigenLayer, one can run a superfast finality chain on top of Ethereum which gives fast economic finality, and which settles optimistically on Ethereum. This superfast finality chain verifies ZK proofs and arrives at consensus within seconds.

### 2\. Decentralized Trust

Decentralized trust is trust based in decentralization. These assurances derive from having a large and distributed enough validator set that you can be comfortable that they are not all colluding.

Some AVSs have failure modes that are not objectively provable onchain easily identified and penalized on chain, so they cannot be built solely on a system of economic trust. These AVSs can be built on a system of decentralized trust, where the property of the AVS continues to hold as long as enough validating nodes act independently without collusion. One way to prevent collusion is to utilize a large decentralized validator set so that collusion becomes difficult or observable.

Each AVS can set specific entry conditions using **subjective oracles** to allow only a certain configuration of nodes (say home stakers) to ensure maximum decentralization of the nodes. By identifying and incentivizing decentralized nodes through additional fees, the overall rewards accrued to decentralized nodes can potentially increase compared to centralized validators. This approach of using subjective oracles to admit only decentralized nodes has the potential to create for the first time a “market value” of decentralization.

So, what AVSs can you build with decentralized trust? Here are some applications:

**Secure Multiparty Computation**. A simple example is Shamir secret sharing, where the user requires a secret to be split into shards and stored in as many non-colluding and decentralized nodes as possible. Any collusion here is a non-attributable fault and thus, the AVS can instead rely on decentralized trust.

[**EigenDA**](https://www.blog.eigenlayer.xyz/tag/eigenda/) **.** It is possible to combine economic trust and decentralized trust to create new services. EigenDA, built on EigenLayer, relies on decentralization to prevent collusion (the data remains available) and economic trust to punish deviations from the equilibrium (if a node is not storing data, it will get slashed through a proof of custody system). This model combines economic trust (losing ETH) with decentralized trust (difficulty of collusion).

If your AVS requires decentralized trust, then as an AVS developer you should ensure that the node software that is required for executing on the AVS’s validation semantics is lightweight in nature. Being lightweight means imposing a smaller resource cost on the stakers who are in your AVS’s decentralized quorum, enabling more stakers to participate, thus giving you more decentralized trust.

### 3\. Ethereum Inclusion Trust

While the first and second trust models absorb the economics and decentralization of the Ethereum trust network, the fact that it is the Ethereum validators who restake enables a whole suite of powerful new features where you can experiment with new opt-in features to the Ethereum protocol without modifying the core Ethereum protocol. These opt-in features open up new proposer commitments on top of the existing proposer commitments from the consensus protocol.

![](https://lh4.googleusercontent.com/_NZocdADH10GPTpTBu3KDIZO-ZtNsXH1mfIYdfVsjaO4SdY3xkgjlsCjIzwBTpnGhC4GpbG71FAXH-sg2-Zb6Yuub7hqHF_44L2ehJ0k2U-sYco3im_0DbMP0DFTLQ2qK04fWztss-LD_h74oANHrIs)

_Figure: illustrates the difference between with and without Ethereum inclusion trust. Not all services need Ethereum inclusion trust if they don’t need Ethereum inclusion in block construction and ordering. AVSs can also mix and match multiple trusts._

Below are some applications of Ethereum inclusion trust:

**MEV Management.** Ethereum block proposers can make credible commitments to order blocks according to different rules, creating a powerful MEV toolkit: (i) proposers committing to sell portions of blocks that they will propose in the future in the MEV market, (ii) proposers agreeing to do event-driven actions, for example, proposers agreeing to include certain transactions (collateral refueling for example) in their block and execute on it in case a certain event happened. This [video](https://youtu.be/zTPKnKsfxek) gives a high-level vision of what MEV management tools are unlocked by EigenLayer.

**Single-slot-finality.** The interaction with Ethereum’s consensus protocol “Gasper” needs careful consideration, however it is possible that when enough block proposers opt-in to a newEigenLayer task where they agree to not fork a block, we can get single-slot economic finality purely via opt-in.

## Building with Programmable Trust

As an AVS developer, in order to inherit trust from EigenLayer in a programmable manner, you must first ask yourself the following two questions:

1. What kind of trust is best suitable for your AVS and how would you penalize wrongdoers if that trust is broken?
2. How much trust (capital staked, number of distinct distributed validators, and number of Ethereum validators needed to make an Ethereum inclusion commitment) does your AVS need and how would you incentivize it?

There is no one-solution-fits-all to answer these questions and your answer will depend on your specific AVS design and security requirements. In this blog post, we have introduced a mental model for you to be able to answer the first question - what type or types of trust may benefit your system design. To answer the second question, we’re here to dive in with you today. Please submit your proposal via this form and we can give you feedback on your design and start the conversation: [https://bit.ly/avsquestions](https://bit.ly/avsquestions)

And if you’re excited about these primitives, join our private research discussion group here: [https://bit.ly/programmabletrust](https://bit.ly/programmabletrust)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/#/portal)

SubscribeEigenLayer is proud to highlight twelve early projects building on top of EigenLayer’s infrastructure to deliver innovative use cases within the Ethereum ecosystem. These projects include actively validated services (AVSs) as well as users of AVSs, including users of EigenDA, the first AVS that will launch on EigenLayer later this year.

Below, we highlight projects from across the crypto ecosystem including [AltLayer](https://www.altlayer.io/), [Blockless](https://blockless.network/), [Celo](https://celo.org/), [Drosera](https://twitter.com/DroseraNetwork), [Espresso](https://www.espressosys.com/), EigenDA (developed by EigenLabs), [Hyperlane](https://www.hyperlane.xyz/), [Lagrange](https://www.lagrange.dev/), [Mantle](https://www.mantle.xyz/), [Omni](https://blog.omni.network/the-block/), [Polyhedra](https://polyhedra.network/), and [WitnessChain](https://witnesschain.com/).

We feel fortunate to have the opportunity to work with such incredibly talented teams on such novel use cases. In addition to the publicly announced collaborations above, we continue to work privately with a large number of potential AVSs and EigenDA users. If you’re interested in exploring either of these paths, please contact us via the forms below:

- [AVS Developers Questionnaire](https://bit.ly/avsquestions)
- [EigenDA Users Questionnaire](https://tinyurl.com/eigenda)

## Benefits for Actively Validated Services

EigenLayer seeks to increase the rate and scope of open innovation by serving as a platform for borrowing programmable trust from the Ethereum trust network, enabling developers to launch a wide array of blockchain systems such as interoperability layers, modular execution layers, data availability services, and more.

EigenLayer provides these developers with the flexibility to customize the trust they want to borrow from Ethereum trust network in a programmable manner, based on decentralization and cryptoeconomics, tailored to their specific use cases.

By using EigenLayer, developers forgo the burden of building their own trust network, reducing both upfront and ongoing costs required to run a successful network. Developers can benefit from Ethereum’s large and decentralized base of validators, as well as Ethereum’s large staked capital base.

Benefits include: the ability to build blockchain systems without needing to set up and maintain your own validator network; avoiding the need for high token inflation rates to incentivize the provision of security; avoiding the need to launch native tokens at all, unless you choose to do so; and the ability to focus on the specific innovation that you as a developer want to enable, without needing to roll the entire blockchain technology stack from the ground up in order to innovate on just one part of it.

This capital-efficient model removes barriers to entry for new blockchain projects and deepens security for established protocols, paving the way for more agile, decentralized, and permissionless innovation on Ethereum.

## Projects Building on EigenLayer

The following projects are pioneers in using EigenLayer to secure their infrastructure, by harnessing the cryptoeconomic security and decentralization of Ethereum that the EigenLayer protocol aggregates, consolidates, and extends to AVSs.

### AltLayer

AltLayer is building Rollups-as-a-Service tools to scale execution at a fraction of the cost. AltLayer offers flash rollup by employing EigenLayer validators to quickly verify state transitions permissionlessly.

### Blockless

Blockless is an infrastructure platform to launch and integrate full-stack decentralized applications, enabling them to operate beyond smart contract limitations. With a globally distributed, trustless node infrastructure—secured and powered by EigenLayer’s restakers and operators—applications can achieve high-performance trustless computing, automatic horizontal scaling, and advanced load distribution.

Deep dive into the collaboration on Blockless’ post in the [EigenLayer forum](https://forum.eigenlayer.xyz/t/zkserverless-framework-x86-verifiable-compute-engine/6429).

### Celo

Celo is migrating from an EVM-compatible Layer-1 blockchain to an Ethereum Layer-2 to enable trustless liquidity sharing, decentralized sequencing and foster greater alignment with Ethereum.

Celo will utilize the data availability layer powered by EigenLayer and EigenDA that inherits architecture from Danksharding’s for increased throughput, lower costs, and reduced latency.

Check the [official migration proposal](https://forum.celo.org/t/clabs-proposal-for-celo-to-transition-to-an-ethereum-l2/6109/2) where Celo’s team expands on EigenLayer’s role in this big event for their ecosystem.

### Drosera

Drosera is a zero-knowledge automation protocol that provides emergency response infrastructure to Ethereum. EigenLayer is leveraged to bootstrap Drosera with a native trust network that becomes more decentralized over time.

Drosera aims to leverage the decentralized nature of Ethereum consensus to create a robust and responsive first responder collective. Protocols define emergency response logic and advanced validation checks for Operators to carry out. Eigenlayer slashing and reward mechanisms ensure honesty and accountability. This approach to security expands monitoring and bug bounty programs to a dynamic model.

### Espresso

Espresso is creating a shared sequencer solution that enables rollup decentralization, improved interoperability, and a robust, highly scalable data availability layer. It is leveraging restaking through EigenLayer to optimize node usage and capital efficiency while ensuring credible neutrality, security, and fast pre-confirmation in transaction validation.

Restaking enables alignment between Layer-1 validators and the Layer-2 ecosystems they support. In a centralized sequencer, nearly all of the rollup value (e.g., fees, MEV) is likely to be captured by the sequencer. If little or none of the value generated by a rollup is captured by the Layer-1 validators, it can destabilize the security of the rollup, since the Layer-1 might be tempted to act maliciously. By decentralizing the sequencer and involving Layer-1 validators in its operations, these security concerns are substantially mitigated.

Further details about this partnership can be found in [Espresso’s announcement](https://medium.com/@espressosys/espresso-systems-and-eigenlayer-announce-ecosystem-partnership-d5041a6bf883).

### EigenDA

EigenDA is a data availability service that offers high throughput and derives economic security via Ethereum operators and restakers. Built on the principles of danksharding, EigenDA is designed to expand the scope of programmability for rollups while pushing the throughput ceiling. Horizontal scaling will enable EigenDA to eventually scale as high as 1 TB/s with minimal cost and technical overhead on each operator. Flexible tokenomics, reserved bandwidth, modifiable signature schemes and elliptic curves, and other features enable EigenDA to support a variety of projects and use cases.

Learn more about EigenDA in [this talk](https://www.youtube.com/watch?v=NtFCliDucMs) from Sreeram Kannan, earlier this year.

### Hyperlane

Hyperlane is developing a permissionless interoperability layer that enables interchain composability, including native rollup bridges, inter-rollup communication, and multichain application architecture. It is bringing modular security derived through restaking via EigenLayer to enable permissionless, chain-agnostic application deployment to any environment.

You can see the scope of our partnership with Hyperlane in [this forum post](https://forum.eigenlayer.xyz/t/permissionless-interoperability-with-hyperlane-restaking/213).

### Lagrange

Lagrange is building infrastructure for zk-based cross-chain state and storage proofs. It is bringing super-linear security derived through restaking via EigenLayer, which provides a powerful primitive to dynamically scale the underlying security of state proof generation, overcoming the inherent security limitations that bridge face at scale.

Lagrange State Committees, composed of EigenLayer restaked validators, attest to the finality of a proposed block state transition submitted by optimistic rollups’ sequencers to Ethereum. These attested blocks are then used to generate zero-knowledge state proofs using Lagrange’s ZK MapReduce proof system. Messaging or bridging protocols can use these proofs to create a shared and permissionless zone of security for cross-chain state.

For more details, see the full announcement on [Langrage’s Medium article](https://medium.com/@lagrangelabs/lagrange-labs-and-eigenlayer-announce-a-strategic-partnership-28bb7d669f98) and their [forum proposal](https://forum.eigenlayer.xyz/t/lagrange-cross-chain-state-committees/628).

### Mantle

Mantle is building an Ethereum Layer-2 that enables fast and cost-effective transactions through innovative rollup architecture and modular data availability. Mantle is currently using MantleDA, a modified variant of EigenDA, and will migrate to EigenDA when it’s launched. This enables the Mantle ecosystem to have the high throughput and low gas cost necessary for next-generation applications in blockchain gaming, decentralized social networks, and more.

Read more about the collaboration in [this blog post](https://medium.com/0xmantle/how-mantle-network-advances-the-ethereum-roadmap-14035d4593aa) from the Mantle team.

### Omni

Omni is creating interoperability infrastructure to act as a unification layer for all rollups that enables the transmission of data from one to another. It is [building on EigenLayer](https://blog.omni.network/the-block/) to derive security for future use cases including cross-rollup stablecoins and primitives that can aggregate liquidity and facilitate rapid and cheap communication between rollups.

You can see how EigenLayer fits in Omni’s tech stack in their [launch blog post](https://blog.omni.network/omni-ethereums-interoperability-infrastructure/).

### Polyhedra

Polyhedra is developing novel zk proof-based infrastructure to enable trustless and efficient cross-chain interoperability using parallel and distributed computation. It is leveraging restaking through EigenLayer to enhance the security and efficiency of Layer-1 and Layer-2 chains using zkBridge, reducing on-chain verification costs on EVM-compatible networks.

You can read more in the [official blog post](https://mirror.xyz/0x9Fed8Bca5721851C21056344519e39B9a1C9E7AE/bZOispao2tR-BeRZkNESnHffXX6c_UiGswJS_jzSXv4) from the Polyhedra team.

### WitnessChain

WitnessChain is building transparency middleware for blockchains. By leveraging Eigenlayer’s decentralized network, WitnessChain is able to create a decentralized watcher network for provable monitoring of AVSs. This network will provide the first line of defense for optimistic rollups.

## Onboard your Project

Again, these publicly announced projects are just the earliest seeds in the budding EigenLayer ecosystem. We’re grateful to these teams for their ingenuity and close collaboration - their contributions are invaluable. We’re committed to permissionless innovation for all, and we’d love to work with you too!

Join the collective of AVSs building on EigenLayer, and projects building on EigenDA, and launch your project with increased functionality, security, and efficiency, by leveraging the base layer decentralized trust of the Ethereum network via EigenLayer. To submit your interest, follow one of the paths below:

- [AVS Developers Questionnaire](https://docs.google.com/forms/d/e/1FAIpQLSciC5BvE4G5eytF2xstn0kSiqX2_UwiYSYxozwBCWQ97o7oOg/viewform)
- [EigenDA Users Questionnaire](https://docs.google.com/forms/d/e/1FAIpQLSez6PG-BL6C6Mc4QY1M--vbV219OGL_0Euv2zhJ1HmcUiU7cw/viewform)

### Keep up with the latest on EigenLayer!

- Visit the website: [eigenlayer.xyz](http://eigenlayer.xyz/)
- Check out the restaking app: [app.eigenlayer.xyz](http://app.eigenlayer.xyz/)
- Read through the documentation: [docs.eigenlayer.xyz](http://docs.eigenlayer.xyz/)
- Meet the community in Discord: [discord.gg/eigenlayer](http://discord.gg/eigenlayer)
- Contribute research ideas to the forum: [forum.eigenlayer.xyz](http://forum.eigenlayer.xyz/)
- Follow @eigenlayer on Twitter: [twitter.com/eigenlayer](http://twitter.com/eigenlayer)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/twelve-early-projects-building-on-eigenlayer/#/portal)

Subscribe## **Overview**

We live in a world where we trust machines with our most important decisions—our money, our security, and even our identities. But trust without verification is just blind faith. What if you could know, with certainty, that an AI is genuinely acting on its own without hidden hands pulling the strings?

[Ungate Wukong](https://ungate.gitbook.io/ungate-wukong) transforms this paradigm by establishing absolute verification at the core. As a pioneering AI-focused AVS on EigenLayer mainnet, they've created the Verifiable Agents Cloud, where AI agents don't just claim autonomy; they prove it cryptographically. Running inside Trusted Execution Environments (TEEs) and validated through EigenLayer's decentralized network, these agents generate proof of every action they take.

The result is AI that is transparent, accountable, and trustworthy through cryptographic certainty. Founded in June 2024, Ungate has been secured by over $5 billion ETH restaked on its platform, marking a fundamental redefinition of what's possible when autonomous agents meet verifiable truth.

## **The Opportunity**

The current landscape of AI automation highlights critical gaps in verification. As agents increasingly control billions in digital assets and execute critical operations, the risks of unverified execution become apparent:

1. **Black-Box Execution:** Without cryptographic proof, there's no way to verify if an agent made a decision independently or was overridden by external intervention
2. **No Reputation Mechanism:** Agents can operate without verifiable accountability, allowing malicious or poorly designed systems to act without consequence
3. **Wallet Control:** Developers may retain access to an agent's private keys, compromising true autonomy and sovereign execution
4. **Legal & Financial Liability:** If developers maintain control over an agent's private keys, they become legally responsible for all agent actions—from trading to governance to fund management

Through EigenLayer's economic security model and decentralized validation framework, Ungate Wukong creates a new verification paradigm. Every agent operation is independently verified by the network of restaked ETH validators, establishing a more trust-minimized foundation for autonomous AI execution.

## **The Solution**

Ungate leverages EigenLayer's decentralized security infrastructure to build verifiable AI agents you can actually trust. Not through promises, but through cryptographic proof.

### **The EigenLayer Advantage**

When AI agents control billions in assets, verification can't be optional. EigenLayer's economic security and validation infrastructure ensures every agent operation is verified. This creates true accountability: agent developers can prove their systems operate autonomously, and users can verify this for themselves. How it works:

1. **Secure Deployment:** AI agents live inside TEEs– ensuring isolated and tamper-resistant execution
2. **Proof Generation:** Agents produce Proofs of Autonomy, Sovereignty, and Integrity, which are cryptographically signed, stored on EigenDA, and archived on FileCoin.
3. **Decentralized Verification:** EigenLayer's network of restaked ETH validators independently verify these proofs.
4. **On-Chain Record:** Verified executions are recorded onchain, creating an immutable audit trail anyone can verify.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcOz-Lcvnk-bjTwNAiUhA_7dH3d_q0Q3WGNnCaBn65iYVXUxjI9edgv6lWgnQ_ozGNA5pKYIkFjzPdMhZgd_JeZog8ap-njoBD9mlxapKvpk_G2MOtOjgCvyAoZQr2nOSuQCy-UAw?key=U7AYeLb4Gjd5nzCAL58-rVbY)

### **The Technical Innovation:**

- **Decentralized Validation:** Instead of trusting a central authority, verification comes from EigenLayer's network of independent validators.
- **Complete Infrastructure:** Ungate's Verifiable Agents Cloud provides everything needed for truly autonomous AI operations– from secure execution environments to cryptographic proof generation.
- **Economic Security:** EigenLayer's restaking mechanism ensures agent developers have real stakes in maintaining system integrity.

Through Ungate’s partnership with Nuffle Labs, this verification framework is expanding beyond Ethereum. Soon, projects will be able to restake SOL, NEAR, BASE, VIRTUAL, and other tokens, creating a unified verification layer for autonomous AI across all major blockchains.

## **What’s Next**

The launch of Ungate on EigenLayer is setting a new standard for verifiable agents. Through cryptographic transparency and decentralized attestation, this integration unlocks trust-minimized applications that will shape the future of Crypto x AI. With EigenLayer’s restaking infrastructure, Ungate is bringing autonomous, sovereign agents to life.

This is no longer a vision; it’s already happening. The demand for verifiable AI agents is growing exponentially. Successful projects are already leveraging TEE-powered AI agents to establish trust and drive adoption. The future of decentralized intelligence is here. Ready to deploy your own verifiable AI agents inside TEEs with decentralized verification?

**Deploy an agent today:** Raise a support ticket in Ungate's [Discord](https://discord.gg/ungate).

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/#/portal)

SubscribeOccasionally, you hear an idea so exciting that you see the future with utter clarity. At Eigen Labs, we’re excited to work with exceptional builders and hear their special insights about the world on a daily basis. We want to share some of ours with you. We’re starting a new series called Request for AVS to showcase how trustless offchain compute and data can change the onchain economy. We hope you join us for v1 😄

## Uniswap v4 is the Endgame

Uniswap v1 was a 0 to 1 innovation, bringing atomic swaps onchain.

Uniswap v2 extended v1’s functionalities, enabling us to trade ERC-20s in a single pool and enabling flash loans and oracles.

Uniswap v3 added concentrated liquidity, making LPing up to 4000x more capital efficient.

But Uniswap v4 is more. Rather than extend the functionalities of prior versions in an opinionated manner, it transitions Uniswap from an AMM to a platform for AMMs. It empowers builders to create any onchain exchange, using hooks to highly customize the trader and liquidity provider experience.

Previously, if builders wanted to innovate on AMM design, they had to build their own exchange, either competing with Uniswap or building on a new chain. But liquidity inherently has network effects, and exchanges are, therefore, a winner-take-all, or at least a winner-take-most game. In the past 24 hours, Uniswap has done 35% of all DeFi spot volume per DeFi Llama. On Ethereum and major L2s, Uniswap’s dominance increases to 62%.

Builders have to not just win on mechanism design, but attract enough liquidity to provide favorable prices to traders. This separation of liquidity and permissionless innovation has caused many exciting AMM designs to not take off. Uniswap v4 changes this, enabling anyone to tap into Uniswap’s liquidity with their mechanism.

## Case Study: Solving Coincidence of Wants with Uniswap v4

To prove that Uniswap v4 is flexible enough to create any exchange, let’s use it to solve a classic problem: coincidence of wants. In a single block, some traders will swap USDC for ETH while others will swap ETH for USDC. With most onchain exchanges, both will trade against the liquidity pool, paying 0.3% in swap fees. Instead, they should swap with each other – saving both of them 0.3%. This is called the coincidence of wants.

We can design a hook to solve for coincidence of wants. Traders will send in orders to Uniswap v4. Rather than immediately getting matched against the pool, a hook can send orders to a matching engine to settle coincidences of wants against each other, which then settles to the overall pool. Traders then receive their desired asset, occasionally paying less in fees.

It’s important to ask: who runs this matching engine? Unfortunately, computing coincidence of wants expends too much gas onchain. We could send our orders to a centralized server. But that defeats the purpose of trading onchain altogether, leading to potential censorship and liveness issues.

If only there were a way to access trustless offchain compute…

## EigenLayer: A Platform for Trustless Offchain Compute and Data

Fortunately, EigenLayer enables trustless offchain compute (and data)! At a high level, EigenLayer is an infrastructure platform that gives you:

1. A decentralized network of node operators,
2. Willing to run your arbitrary node software and
3. Economically aligned to act honestly with your protocol

We call these decentralized networks Actively Validated Services (AVS)!

These operators can do a variety of tasks to boost the capabilities of Uniswap v4 hooks like:

- Run a matching engine (helping in our coincidence of wants example)
- Batch many transactions together
- Run an auction
- Gain access to real-world information, like
  - Prices (like an oracle)
  - Volatility
  - Stablecoin depegs
  - Liquid staking yields
- (... and more)

By extending offchain compute and data to hooks, EigenLayer adds the missing piece for Uniswap v4 to recreate any onchain exchange.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXeaWn7dyKQo7TkWhg4kg8dsx528WACJ8LarLDYRyFUknL4apMhPNJDUt4AYp-1nVPrDWiiUZGMhEB3Hi8VeP-ukltb9g2IkdANahkiXNLBUlXKx_58wXyF6y_BgqSrqV3VqSbEgKal_vV55d-vfYi7K46D9?key=ge1Hs6TztWXBFBFnALwnOg)

## Ideas to Build with Uniswap v4 Hooks and EigenLayer AVSs

The design space with v4 hooks and AVSs is endless! We will share 6 ideas briefly, but if you’re interested in building any, please reach out to us! We’d love to chat more :)

### Idea: solve coincidence of wants to reduce taker fees

This is the example we talked about!

1. Taker flow will exist in both directions (buy/sell) for any given pool.
2. AVS operators can match these orders, enabling swappers to pay less fees and incur less price impact.

### Idea: dynamic fees for periods of high volatility

1. Uniswap LPs fare worse during times of high volatility.
2. To adjust, AVS operators can automatically increase fees during periods of high volatility. Operators can pull volatility data from a trusted external volatility source or calculate it themselves.

### Idea: automatically adjust LP positions for LST pools

1. Uniswap LPs face extra slippage from the natural yield-bearing nature of LSTs when LPing yield-bearing tokens (like stETH-ETH or rETH-USDC).
2. AVS operators could automatically read the yield from these liquid staking positions and move all LP positions up the curve by a constant amount.

### Idea: dark pools

1. Dark pools enable people to trade large orders privately without affecting the broader market. In traditional finance, people can send a private order to an exchange that is matched against other takers at the mid-market price (halfway between best bid and ask).
2. Traders can send their orders to AVS operators, who would share and match market orders together at the price of the most liquid exchange, predefined for that pair.

### Idea: run an auction to reduce Loss Versus Rebalancing (LVR)

1. LVR exists because of the price discrepancy between continuous offchain exchanges (Binance) and onchain exchanges (which only trade every block, 12 seconds for Ethereum mainnet).
2. AVS operators can run an auction to determine who gets the first transaction in a block.
3. The auction proceeds would go to LPs, instead of validators as they would with traditional MEV auctions. Proceeds can also go to swappers, to help with optimal price execution, thus driving more volume on chain

### Idea: run a shared state across all chains

1. AMM pools can have different prices across chains. This leads to less efficient price discovery and more arbitrage on smaller chains.
2. Have operators maintain an aggregate state of capital. When traders send orders, they trade against this combined pool and receive less slippage.

## Conclusion

With the biggest decentralized exchange transitioning into a platform, there’s never been a better time to build onchain. Create any exchange with offchain compute and data as a Uniswap v4 hook AVS. If you’re interested in building any of these (or any other AVS), please dm on [twitter](https://twitter.com/ishaan0x) @ishaan0x. Let’s chat :)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/univ4-hooks/#/portal)

SubscribeWe're excited to announce the culmination of our phased rollout with the **removal of all Liquid Staking Token (LST) caps and unpausing deposits, effective April 16th, 2024, at 9:00 AM PST**. This milestone marks the next chapter in EigenLayer's journey, fostering a wide-open and dynamic staking landscape.

**A Phased Approach for a Robust Ecosystem**

EigenLayer's development has been guided by a commitment to stability and growth. The successful launches of Stages [1](https://www.blog.eigenlayer.xyz/eigenlayer-stage-1-mainnet-launch/), [2](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/) and [3](https://www.blog.eigenlayer.xyz/avs-launch/) laid the groundwork for a thriving ecosystem of restakers, operators, AVSs, and rollups. With this solid foundation in place, it's time to unleash the full potential of the EigenLayer network by removing the LST caps.

**A Look Ahead: Protocol Award Discussions and Beyond**

The EigenLayer community is actively discussing a potential implementation of any future protocol awards. This proposal suggests capping them at 33% for any single LST (or user), to ensure a balanced and decentralized ecosystem.  You can find a detailed breakdown of this proposal and its rationale in this previous blog post: \[ [Balancing Neutrality and Decentralization Blog Post](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)\].

**Restakers:**

Head over to the EigenLayer app: [https://app.eigenlayer.xyz/](https://app.eigenlayer.xyz/) to explore the uncapped LSTs or to natively restake through an EigenPod.

**Start Restaking Now:** [EigenLayer App](https://app.eigenlayer.xyz/)

**Restaking Guide:** [Restaking Documentation](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/)

**Delegation Guide:** [Delegation Guide](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/restaker-delegation/)

To maximize the impact of your stake and contribute to a growing EigenLayer network of AVSs, [**delegation**](https://docs.eigenlayer.xyz/eigenlayer/restaking-guides/restaking-user-guide/restaker-delegation/) **is key**. The EigenLayer app allows you to easily delegate your entire stake to an Operator that aligns with your preferences. Operator profiles provide clear information about the AVSs they run, empowering you to make informed choices for your stake. You can read [operator introductions](https://forum.eigenlayer.xyz/t/operators-mainnet-campaign/12828/150) on the forum for more information on the available options.

**An Open Market for Innovation**

We are committed to building a future where anyone can participate and contribute to a more scalable and innovative Ethereum. With the removal of LST caps, we're excited to see the EigenLayer ecosystem flourish and unlock its full potential. Stay tuned for further updates as we continue to build a secure and scalable future for decentralized applications!

**Please find official EigenLayer resources below:**

EigenLayer App ( [app.eigenlayer.xyz](https://app.eigenlayer.xyz/))

Documentation ( [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz/))

Discord ( [discord.com/invite/eigenlayer](http://discord.com/invite/eigenlayer))

Immunefi Bug Bounties ( [immunefi.com/bounty/eigenlayer](https://immunefi.com/bounty/eigenlayer))

X: [@eigenlayer](https://twitter.com/eigen_da)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/#/portal)

SubscribeIn preparation for the mainnet launch for Operators and EigenDA, there's an important update regarding the next cap raise.

Several staking parameters are set to be adjusted:

- First, the introduction of three new LSTs to the EigenLayer restaking ecosystem: sfrxETH, mETH, and LsETH.
- Second, removal of the 200k ETH individual caps on LSTs. This modification is designed to invite organic demand for restaking across various LST options.
- Third, for future consideration, it is recommended to cap the participation and influence of any LRT or LST, or single participant, at a maximum of 33% within the protocol's governance and incentive structures. This proposed measure seeks to prevent dominance by any single entity, thus ensuring equity and diversity within the ecosystem. The aim is to encourage broad engagement and maintain the protocol's commitment to neutrality and decentralization.

Note that future payments from AVSs to LSTs will not be affected by these changes.

To incorporate these modifications, the schedule for LST additions and the unpause phase has been shifted, now **delayed by one week.**

**Revised Schedule**

- [Restaking Window](http://app.eigenlayer.xyz/): Unpaused from **February 5, 12 PM PT to February 9, 12 PM PT**, with new LSTs sfrxETH, mETH, and LsETH, and no caps for any LST as described above **.** After this time, all deposits will again be paused.
- These changes align with our ongoing commitment to security, decentralization, and a robust staking environment.

Get ready to engage in these exciting opportunities in ETH restaking starting February 5th. This transition signifies a major shift, as it marks the first instance of eliminating the TVL caps for each token for a fixed period of time. This change ushers in a period filled with extensive restaking possibilities, available from February 5th to 9th.

**Official Links**

- [Website](https://eigenlayer.xyz/) (including new [Ecosystem Page](https://www.eigenlayer.xyz/ecosystem-category-avs?category=AVS%2CRollup%2COperator))
- [Documentation](https://docs.eigenlayer.xyz/overview/readme)
- Blog Posts: [EigenLayer](https://www.blog.eigenlayer.xyz/tag/eigenlayer/), [EigenDA](https://www.blog.eigenlayer.xyz/tag/eigenda/), and [AVS Research](https://www.blog.eigenlayer.xyz/tag/avs-research/)
- [X/Twitter](https://twitter.com/eigenlayer)
- [Discord](https://discord.gg/eigenlayer)
- [Discourse](https://forum.eigenlayer.xyz/)

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/update-on-upcoming-lst-additions-and-restaking-unpause/#/portal)

Subscribe_This blog is inspired by David Philipson’s [_series on account abstraction_](https://www.alchemy.com/blog/account-abstraction)._ Special thanks to [Noam Horowitz](https://twitter.com/ProbablyNoam), [Shiva](https://twitter.com/ShivanshuMadan), [NashQ](https://twitter.com/NashQueue), [Mike Neuder](https://twitter.com/mikeneuder), and [Sina](https://twitter.com/sina_eth_) from the community for comments and feedback on the article.

While many people are familiar with the terms restaking and EigenLayer, only a few are aware that our core contracts **consist of thousands of [**lines of code**](https://github.com/Layr-Labs/eigenlayer-contracts/tree/master/src/contracts)** with the following architecture.

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/image.png)_Simplified architecture of EigenLayer_

Where did the complexity of a **seemingly simple** idea come from?

In this blog post, we will take you through the **_evolution_** of the protocol, by covering how EigenLayer's current complex architecture emerged from the initial concept.

The target audience for this post is individuals who have a _basic_ understanding of smart contracts and have heard of EigenLayer or restaking.

💡

Since the purpose of this blog post is to provide a high-level explanation of the EigenLayer's design evolution, there will be instances where the interface, variables, and logic may differ from the current EigenLayer core [contracts](https://github.com/Layr-Labs/eigenlayer-contracts/tree/master/src/contracts).

Now, let's dive in.

## End Goal: Making building infrastructure easy

First, let's preface with the problem EigenLayer is trying to solve. If you are already familiar with this part, move to later sections.

Developers who build decentralized infrastructures on Ethereum face the challenge of establishing their own economic security. While Ethereum provides economic security for smart contract protocols, **infrastructures like bridges or sequencers require their own economic security** to enable a distributed network of nodes to reach consensus.

Consensus mechanisms are essential for facilitating interactions among these nodes, whether it's an L1, an oracle network, or a bridge.

[Proof of work](https://en.wikipedia.org/wiki/Proof_of_work) is energy-intensive, [proof of authority](https://en.wikipedia.org/wiki/Proof_of_authority) is overly centralized, and as a result, [proof of stake](https://en.wikipedia.org/wiki/Proof_of_stake) (PoS) has emerged as the **prevailing** consensus mechanism for most infrastructure projects.

**_However_, _bootstrapping a new PoS network is hard_.**

**Firstly**, it is difficult to identify where the stakers (people who provide stake) are located. There is no single place where developers can find stakers.

**Secondly**, stakers must invest a significant amount of money to obtain a stake in the new network, usually by buying the network's native token, which is generally volatile and hard to get.

**Thirdly**, stakers must forgo other rewards opportunities, such as the 5% rewards offered by Ethereum.

**Lastly**, the current security model is undesirable as the cost to corrupt any dApp is simply the cost needed to compromise its weakest infrastructural dependency.

💡

For now, we will assume that a staker participating in an infrastructure project is also responsible for operating the off-chain software to guarantee its security. However, we will change this assumption later in the article.

**EigenLayer was created to address these issues.**

- It serves as a platform connecting stakers and infrastructure developers.
- Stakers can offer economic security using any token.
- Stakers have the option to restake their stake and contribute to the security of other infrastructures while earning native ETH rewards.
- Through restaking, EigenLayer pools security instead of fragmenting it.

💡

EigenLayer's [whitepaper](https://docs.eigenlayer.xyz/overview/whitepaper) provides an in-depth exploration of these questions.

**_EigenLayer generalizes the concept of providing economic security._**

## Goal: Creating a platform connecting stakers and infrastructure developers

EigenLayer is the platform where stakers can provide stake for any infrastructure project, and infrastructure projects can pitch to potential stakers on EigenLayer. The backbone of this platform is **enabling stakers to make credible commitments for different infrastructures.**

What is this credible commitment?

These commitments are applicable to all PoS systems, not just EigenLayer. L1 stakers generally commit to following the protocol rules and risk losing their stake if they sign conflicting blocks at the same block height.

Infrastructure developers build the infrastructure logic and software, while stakers provide stake to secure the infrastructure. _The **stake** serves as a **commitment** to the users of the infrastructures._ This **commitment** is for the smooth running of the protocol and **against _specific_ misbehaviors.**

**_Conceptually_**, when a project has $100m stake behind it, it means that if it deviates from its _commitment_ and behaves maliciously, _some portion of that $100m will be slashed_. For simplicity, "slashed" can be thought of as burning that money.

The higher this number is, the more security and guarantees it provides to its users.

**If we permit stakers to allocate stakes to various commitments, we can then create a user-friendly interface on top to make a platform.**

We require a trustless and programmable platform to enforce different staker commitments, **_and Ethereum is the most suitable for this._** Additionally, Ethereum holds the largest stake, which aids in bootstrapping the staker-side market.

**The goal here** is that a staker should be able to secure, for example, a bridge protocol through EigenLayer, on Ethereum. If a staker were to maliciously forge a message on Ethereum and transmit it to Gnosis, anyone can submit proof of this and slash the staker.

Since the staker's stake and commitment enforcement occur on Ethereum, the slashing logic is also implemented on Ethereum as smart contracts. If the staker breaches their commitment, anyone can present proof to the slashing contract and forfeit the malicious staker's stake.

**This forms the foundation of EigenLayer - any staker can make credible commitments to any infrastructure protocol.**

### Initial Design for EigenLayer

Now, let's try to implement this. We'll start with the simplest design: a staker stakes tokens into a contract, which includes a function that allows for the slashing of the staker's tokens if a proof is submitted and meets the criteria. The user can then withdraw their balance.

Other stakers can also stake into this contract, adding to the security of the infrastructure. We'll refer to this contract as `TokenPool`.

To provide some clarity, here are some definitions for the terms used in this article:

- **Staker**: anyone who provides tokens into EigenLayer.
- **Token**: any kind of token; for simplicity, think of an ERC20 token for now.
- **TokenPool**: a smart contract that holds the stakers' tokens.
- **Slashing**: removing the staker's access to their staked tokens.

![](https://hackmd.io/_uploads/B1hc4rLk6.png)

The interface of this `TokenPool` can be represented as the following:

```solidity
contract TokenPool {

    mapping(address => uint256) public balance;

    function stake(uint256 amount) public;
    function withdraw() public;
    function slash(address staker, ??? proof) public;
}

```

Does this fulfill the requirement for any staker to provide stake for any infrastructure? Yes!

1. Staker can pledge stake to a specific commitment (defined in the `slash` function)
2. Slashed if found malicious (breaks from the commitment).
3. Withdraw from the system.

✅ It serves as a platform connecting stakers and infrastructure developers.

\- Stakers can offer economic security using any token.

\- Stakers have the option to restake their stake and contribute to the security of other infrastructures while earning native ETH rewards.

\- Through restaking, EigenLayer pools security instead of fragmenting it.

## Goal: Reduce opportunity cost for stakers

When new infrastructure protocols launch, they typically introduce a native token. However, convincing stakers to hold this token is still challenging.

**First**, the native token's value can fluctuate significantly, making it difficult to persuade stakers to acquire it.

**Second**, there is competition from other protocols offering higher rewards rates, which makes it necessary for the protocol to provide compelling incentives to attract stakers.

EigenLayer addresses these challenges by allowing stakers to stake any token and earn multiple rewards simultaneously. Since EigenLayer operates on the Ethereum network, we can leverage existing Ethereum primitives to achieve this.

One approach is to enable stakers to provide stake in a **liquid staking token** (LST). By using LST, stakers can retain their native ETH rewards while securing other infrastructures and potentially earning additional rewards.

💡

What if I do not want to go through a LST provider and instead want to restake my solo validator? We describes in detail how EigenLayer enables this on the bonus section at the end.

✅ It serves as a platform connecting stakers and infrastructure developers.

✅ Stakers can offer economic security using any token.

✅ Stakers have the option to restake their stake and contribute to the security of other infrastructures while earning native ETH rewards.

\- Through restaking, EigenLayer pools security instead of fragmenting it.

## Goal: Pooling security through restaking

EigenLayer's final goal is to address the issue of fragmented security.

Currently, if an application's security relies on multiple infrastructure dependencies, a single economically compromised dependency can threaten the entire application's security.

This problem becomes worse when the economic security of these dependencies is isolated. These isolated points form economic "honey pots" that attract potential attacks.

Therefore, instead of maintaining several separate economic security pools for different infrastructures, it would be beneficial to consolidate these security measures. Doing so could progressively improve the economic security of all dependencies at once.

💡

_**In the EigenLayer context, these infrastructure services are referred to as Actively Validated Services (AVS).**_ _It's important to differentiate them now, as AVS fundamentally differ from traditional infrastructure projects from the benefits provided by EigenLayer._

### Implementing pooled security

Given the model we currently have, how can we incorporate pooled security into it?

As it is, each `TokenPool` has its own unique slashing condition **embedded within** the contract. This implies that for each AVS, a separate `TokenPool` contract needs to be coded. If a staker wishes to participate in two AVSs simultaneously, they would need a new `TokenPool`. This new pool must **incorporate** the `slash` functions from both AVSs.

If two slashing conditions can simultanesouly slash a staker, the staker is making two credible commitments at the same time.

A more **efficient** approach would allow the staker to participate in different AVSs without the need to create new `TokenPool` contracts or modify the `slash` function.

This can be accomplished by relocating the `slash` function into a different contract, termed the " **slasher**". Here is how they would interact:

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/image-1.png)Intermediate EigenLayer design after separating the slashing from the token pools.

The new interface for the `TokenPool` would look like this. ( `slash` is no longer there.)

```solidity
contract TokenPool {
    mapping(address => uint256) public balance;
    mapping(address => address[]) public slasher;

    function stake(uint256 amount) public;
    function withdraw() public;
    function enroll(address slasher) onlyOwner;
}

```

We still maintain the `stake`, `withdraw`, and `balance` functions and variables. In addition to these, we've introduced a new function and a new variable. The variable, `slasher`, monitors each staker's currently enrolled AVSs. The function, `enroll`, allows a staker to participate in AVSs

Enrolling into an AVS therefore is giving a `slasher` the ability to slash your stake.

With this modification, after staking into the `TokenPool`, a staker can join a specific AVS by calling the `enroll` function. This function will incorporate the AVS-specific slasher contract into the `slasher` mapping.

💡

The enroll function is access controlled here because it could potentially enable anyone to enroll into any slasher contract. To ensure that this TokenPool is managed securely, we will assume that it is overseen by a trusted third-party.

During the withdrawal process, the `TokenPool` contract can ask each `slasher` to determine if the staker is eligible to withdraw. This verification is done using the `isSlashed` function in the `slasher` contract. If `isSlashed` for this staker is `TRUE`, the staker cannot withdraw their stakes as they have been slashed.

```solidity
contract slasher {
    mapping(address => bool) public isSlashed;

    function slash(address staker, ??? proof) public;
}

```

The `slash` function contains the slashing logic for each AVS. While the slashing logic might differ across various AVSs, upon successful execution of the `slash` function, the function will set the `isSlashed` variable to true for the questioned staker.

✅ It serves as a platform connecting stakers and infrastructure developers.

✅ Stakers can offer economic security using any token.

✅ Stakers have the option to restake their stake and contribute to the security of other infrastructures while earning native ETH rewards.

✅ Through restaking, EigenLayer pools security instead of fragmenting it.

**With pooled security implemented, we have created the minimum viable EigenLayer!** The minimum viable version acts as a platform connecting stakers and AVS developers. Stakers can now stake any token without compromising other rewards, while AVS developers can utilize the shared security pool to safeguard various types of new infrastructure applications.

In the upcoming section, we will improve the minimum viable version to accommodate additional use cases and make EigenLayer future proof.

## Goal: Staker doesn't have to operate

In the minimum viable design, stakers can stake tokens for various AVSs. However, some stakers may not have the capability or desire to personally handle the operations that secure the AVS. This is similar to **ETH** holders who may prefer **not to** operate their own **validators**.

Our goal is to **distinguish** between these two roles. **Stakers** provide tokens for the economic security of each AVS, while **operators** are individuals who run software to ensure the operational security of each AVS.

We can make a **slight** adjustment to the `TokenPool` contract to include a delegation process. This allows a staker's token balance to be represented by the operator they delegate to. However, if the operator violates the AVS commitment, the staker's tokens will be slashed.

```solidity
contract TokenPool {
    mapping(address => uint256) public stakerBalance;
    mapping(address => uint256) public operatorBalance;
    mapping(address => address) public delegation;
    mapping(address => address[]) public slasher;

    function stake(uint256 amount) public;
    function withdraw() public;
    function delegateTo(address operator) public;
    function enroll(address slasher) operatorOnly;
    function exit(address slasher) operatorOnly;
}

```

We've divided the balance variable into **two distinct parts**: one for the operator and one for the staker. Additionally, we've introduced a `delegation` mapping to record the delegation relationships between stakers and operators.

We've also made a minor modification to the slasher mapping. Now, it gives the slasher contract the authority to slash the operators instead of the stakers.

```solidity
contract slasher {
    mapping(address => bool) public isSlashed;

    function slash(address operator, ??? proof) public;
}

```

In terms of functions, we've incorporated `delegateTo`, enabling stakers to delegate their tokens to operators. The `enroll` function has been redesigned to be operator-specific, allowing the operator to enroll in different AVSs, rather than the stakers.

During the `withdraw` process, the contract initially identifies the operator to whom the staker is delegated. It then checks each `slasher` contract linked to that operator address. If any `slasher` reports that the operator is slashed, the withdrawal process is immediately halted.

### Maintaining staker autonomy

If the operator is the only one who can opt into AVSs, how can stakers decide which AVSs they want their stake to secure? When a staker is also an operator and self-delegates their stake, they maintain full control over the AVS they operate. But what if a staker can't run it on their own and requires another operator to manage these services? Doesn't this give the operator total control over AVS selection?

We can allow the staker to select the AVS through a simple workaround. The operator can maintain different addresses for various AVS combinations. **Each address represents a potential combination** of AVSs supported by the operator.

For instance, if an operator supports two AVSs, they can have three addresses: one for AVS 1, another for AVS 2, and a third for running both.

If a staker trusts the operator and only wants to secure AVS 1, they can delegate to the appropriate operator's address.

Through this method, we enable stakers to fully control their stake. At the same time, they can delegate off-chain responsibilities to operators.

### Modularizing the operator

The `TokenPool` contract is quite bulky right now. We will take the operator-specific functions from the `TokenPool` and put it into a separate contract, `DelegationManager`.

Now, the TokenPool contract would look like the following:

```solidity
contract TokenPool {
    mapping(address => uint256) public stakerBalance;

    function stake(uint256 amount) public;
    function withdraw() public;
}

```

The `TokenPool` contract will only track the balance of each staker. The tracking of AVS and enrolling will be handled by the `DelegationManager`.

```solidity
contract DelegationManager {
    mapping(address => uint256) public operatorBalance;
    mapping(address => address) public delegation;
    mapping(address => address[]) public slasher;

    function delegateTo(address operator) public;
    function enroll(address slasher) operatorOnly;
    function exit(address slasher) operatorOnly;
}

```

Lastly, the slasher contract remains unchanged. It tracks which operator is slashed at the current time for each AVS.

```solidity
contract slasher {
    mapping (address => bool) isSlashed;

    function slash(address operator, ??? proof);

}

```

After cleaning the contract structure and modularizing each component, the architecture would now look like this:

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/image-2.png)Intermediate EigenLayer design after separating the operator role from the staker role.

## Goal: Support more tokens

The design we've developed so far only supports staking **one** token because we only maintain **one** mapping for stakers.

We can address this by adopting a **LP-share based model** of accounting and creating token-specific `TokenPool`.

To do this, we will create a new contract called `TokenManager`. `TokenManager` will be the place where stakers **stake and withdraw** their tokens.

Underneath the `TokenManager`, **each** **token** will have a `TokenPool`. The `TokenManager` will act as the accounting hub for all tokens; it will not store any tokens itself. Each `TokenPool` will hold its own corresponding tokens.

![](https://hackmd.io/_uploads/BJ7iCyMea.png)New component of the design which can now track different tokens from the stakers.

When a user stakes a token, it is processed by the `TokenManager`. The `TokenManager` then invokes the `stake` function of the relevant `TokenPool` associated with that token. The user's tokens are transferred to the `TokenPool`. By the end of the function, `totalShares` and `stakerPoolShares` are updated to reflect the new stake. `totalShares` keeps track of the total number of shares issued by the `TokenPool`, while `stakerPoolShares` records the number of shares held by each individual staker in each `TokenPool`.

The interface for each contract would look like this.

```solidity
contract TokenManager {
    mapping(address => address) tokenPoolRegistry;
    mapping(address => mapping(address => uint256)) stakerPoolShares;

    function stakeToPool(address pool, uint256 amount);
    function withdrawFromPool(address pool);
}

```

```solidity
contract TokenPool {
    uint256 public totalShares;
    function stake(uint256 amount) TokenManagerOnly;
    function withdraw(uint256 shares) TokenManagerOnly;
}

```

A minor change will be made to the DelegationManager as well to reflect this change. Instead of tracking a simple mapping of each operator's balance, the `DelegationManager` will track how much shares does the operator have for each `TokenPool`, similar to `TokenManager`.

```solidity
contract DelegationManager {
    // ...
    mapping(address => mapping(address => uint256)) operatorPoolShares;
    // ...
}

```

Now, under the same core architecture, stakers can stake any token into EigenLayer to secure other AVSs.

## Goal: Expand AVS designs

**Consider the following situation**: a staker withdraws their stakes **right after** engaging in slashable behavior, **but before** others can slash their assets.

_For example_, let's say there's an operator who is also a staker, and they act maliciously in an AVS. Before anyone else can slash onchain, the operator withdraws their stake from the EigenLayer contracts. This is possible because, at the time of withdrawal, the `slasher` has not yet been updated to slash their assets. Consequently, the AVS can no longer slash the malicious operator/staker since there are no more tokens to slash with.

![](https://hackmd.io/_uploads/SyN55Cggp.png)A potential timeline of event for a malicious operator/staker.

Therefore, an AVS that is "safe" would need a slashing contract capable of freezing malicious operators in the **same block** where the incident occurred. This limitation greatly restricts AVS designs, **rendering most of them unsafe.**

One solution is to incorporate an **unbonding period**. Instead of enabling stakers to instantly withdraw their stake, we introduce a **delay** in the withdrawal process known as the unbonding period. After which, the staker can withdraw like before.

When a staker decides to withdraw from the system, their request is placed in a **queue**. This queued withdrawal is only processed after the operator's longest unbonding period has **expired**. This is because an operator may manage multiple AVSs, but a staker's withdrawal can only align with one unbonding period. For security reasons, **the system sets the unbonding period to the longest one.**

For example, if a staker delegates their stake to an operator participating in three AVSs with unbonding periods of six, five, and seven days, they must wait seven days after requesting a withdrawal from EigenLayer to access their stakes. This is because seven days is the longest of the three periods.

After the seven-day period concludes, the staker can **withdraw** their stakes. However, if the operator to whom they delegated becomes **slashed** during this time, the pending withdrawal will be **stopped** as well.

To incorporate this change, the `DelegationManager` would need to track this unbonding period for each operator and update it **whenever the operator enrolls into a new AVS**.

```solidity
contract DelegationManager {
    // ...
    mapping(address => uint256) public unbondingPeriod;
    // ...
}

```

After tracking the unbonding period for each operator, we will also incorporate this value into the staker withdraw process. **The withdraw process consists of two steps**:

1. Stakers queue a withdrawal.
2. After the unbonding period has passed, the staker can complete their withdrawal.

The updated interface would look like the following

```solidity
contract TokenManager {
    mapping(address => address) public tokenPoolRegistry;
    mapping(address => mapping(address => uint256)) public stakerPoolShares;
    mapping(address => uint256) public withdrawalCompleteTime;

    function stakeToPool(address pool, uint256 amount) public;
    function queueWithdrawal(address pool) public;
    function completeWithdrawal(address pool) public;
}

```

When a staker queues a withdrawal, the `TokenManager` will verify with `DelegationManager` whether the staker's delegated operator is slashed. If the operator is not slashed, the `TokenManager` will update the staker's `withdrawalCompleteTime` according to the `unbondingPeriod` from the `DelegationManager` and the current time.

After the unbonding period, the staker can finalize its withdrawal through `completeWithdrawal`. The function will verify if the `withdrawalCompleteTime` has passed. If it has, the staker's token will be transferred out following the same process as before.

💡

The design of this process is complex and has undergone several iterations in our pipeline. We could even write a separate article on this topic! At present, we use a time-tracking system to achieve this unbonding tracking. This part is still a work in progress!

### Modularize the slashers

Since we are making the slashing mechanism safe, let's also try to make it more modular and efficient.

Currently, during the withdrawal process, `TokenManager` would need to check with each individual `slasher` to see if the operator is slashed. **This adds gas overhead to stakers and could significantly lower the rewards for smaller stakers.**

Moreover, as AVS developers typically design slashers, modularizing this particular component could simplify the development process for individual AVSs.

Like the `TokenManager`, we will utilize a two-part design for the slashing mechanism. `SlasherManager` maintains the status of each operator. Individual `slasher` will handle the slashing logic for each AVS.

![](https://hackmd.io/_uploads/rytN0yzeT.png)Modularize the slashing contract even more to reduce staker gas cost.

```solidity
contract SlasherManager {
    mapping(address => bool) public isSlashed;
    mapping(address => mapping(address => bool)) public canSlash;

    function enrollAVS(address slasher) operatorOnly;
    function exitAVS(address slasher) operatorOnly;
}

```

The `SlasherManager` allows an operator to enroll in an AVS by adding the AVS slasher contract as one of the slashers capable of slashing the operator. This permission of "who can slash who" is tracked by the `canSlash` variable.

```solidity
contract slasher {
    function slash(address operator, ??? proof) public;
}

```

The `slasher` will be AVS-specific and most-likely be developed by the AVS developers. It will interact with the `SlasherManager` to update the status of different operators.

## We've Invented EigenLayer!

To recap: **EigenLayer aims to simplify infrastructure building.** We started with four main goals:

1. Connect stakers and infrastructure developers through a platform.
2. Allow stakers to provide economic security using any token.
3. Enable stakers to restake their stake and earn native ETH rewards while contributing to the security of other infrastructures.
4. Pool security through restaking instead of fragmenting it.

After **_several_** iterations, we developed three core components: `TokenManager`, `DelegationManager`, and `SlasherManager`. Each component has a specific function:

![](https://www.blog.eigenlayer.xyz/content/images/2023/10/image-3.png)Simplified architecture of EigenLayer

- `TokenManager`: Handles staking and withdrawals for stakers.
- `DelegationManager`: Allows operators to register and track operator shares.
- `SlasherManager`: Provides AVS developers with the interface to determine the slashing logic.

**These core components also communicate with each other to ensure the safety of the entire system.**

In addition to these core contracts, there are many other functions and contracts that enhance the entire stack. These additional functionalities support various AVS designs, simplify offchain technical complexity, and reduce gas fees for users and operators.

To learn more about these other functions, you can visit our public repository at: [https://github.com/Layr-Labs/eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts)

💡

If you have any question on the EigenLayer core contracts, drop some comments at [https://research.eigenlayer.xyz/](https://research.eigenlayer.xyz/) 😉

## Bonus 1: Who's trusting who?

**When a system is modular, it can be challenging to track the trust assumptions within the protocol.** Therefore, it is essential to explicitly outline the trust assumptions among the participants involved in the protocol.

In EigenLayer, there are three main agents: stakers, operators, and AVS developers.

The operator is relying on the AVS developer to accurately code the client software and the onchain slashing condition. If there are bugs in the AVS softwares, **_at best_**, the operator might miss potential fee payments. **_At worst_**, the operator could be slashed for all their stakes.

Given the significant amount of value at stake, it is important to ensure that the entire system has **training wheels before it is put to the test**.

The veto committee serves as these training wheels. It has the power to reverse slashing resulted from non-malicious behaviors. The veto committee is a **mutually trusted party** among the stakers, operators, and AVS developers.

This way, the trust assumptions placed on the AVS developers can be removed. Even if there's a software bug inside the AVS, the staker and operator won't be penalized.

The staker is trusting the operator they are delegating to. If the operator misbehaves, the staker could miss out on potential fee payments or even lose their entire stake. **This trust assumption is the same for existing validating services such as Binance Staking and other staking services.**

The AVS developers rely on the operators to act honestly. If the operators don't, the AVS service will degrade significantly, leading to customer loss and other consequences.

With the veto committee, among the participants, the trust assumptions are the following:

- **Stakers trust operators** to behave honestly, and misbehavior could lead to slashing.
- **AVS developers trust operators** to operate the AVS software honestly.
- Stakers, operators and AVS developers **trust the veto committee** for reversing slashing.

## Bonus 2: Native Restaking

So far, we've discussed staking LST for restaking. But what if you don't want to stake into EigenLayer through a liquid staking protocol? You can start participate in EigenLayer through native restaking.

Let's define native restaking: it is the process of using ETH within a validator for additional commitments. If the validator deviates from the commitment, they will lose the ETH held within their validator.

The challenge here is that the ETH inside these validators is not represented as ERC20 tokens. Instead, the ETH exists on the beacon chain. If you're not familiar with the execution layer or the consensus layer (beacon chain), [this explainer](https://docs.prylabs.network/docs/concepts/nodes-networks) is a great resource to get you up to speed.

To solve this issue, we can use `EigenPod` to track Ethereum validator balances and slash them if necessary.

`EigenPod` functions as a virtual accounting system. With `EigenPod`, we can monitor the ETH balance for each restaked validator.

At a high level, `EigenPods` handle the **withdrawal process for validators.** When a validator withdraws their stakes from EigenLayer, the ETH **first** passes through `EigenPod` to check if the validator has been slashed. If the validator has been slashed, the tokens will be **frozen** within the `EigenPod` contract, effectively slashing them.

### Implementing EigenPod

Implementing `EigenPod` is tricky because Ethereum validator balances are stored on the beacon chain and we **cannot access** the beacon chain data on the execution layer.

To overcome this issue, we utilize an oracle to relay the beacon chain state root to the execution layer. By obtaining the beacon state root, we can access validator balances by providing the corresponding merkle proof.

With [EIP-4788](https://eips.ethereum.org/EIPS/eip-4788) live, we can remove this oracle and query the beacon root directly from the execution layer.

To encapsulate the accounting system, we will incorporate a model similar to the `TokenPool` and `TokenManager` model, to **modularize** the native restaking system. Each `EigenPod` will handle the withdrawal process for **one validator**. The `EigenPodManager` will coordinate with other core contracts to track the amount of ETH restaked for each operator and staker.

```solidity
contract EigenPodManager{
    mapping(address => uint256) public restakerShares;

    function createEigenPod(address owner) public;
    function stakeToPod(address pod, uint256 amount) public;
    function withdrawFromPod(address pod) public;
}

```

```solidity
contract EigenPod{
    address BEACON_CHAIN_ORACLE;
    address podOwner;
    uint256 restakedAmount;

    function stake(uint256 amount) public;
    function verifyRestakedBalance(uint256 amount, MerkleProof proof) public;
    function withdraw() public;
}

```

The `EigenPodManager` tracks how many shares each staker has. It allows stakers to create `EigenPod`, stake into it, and withdraw from it.

`EigenPod` tracks individual validator balances on the beacon chain through the `restakedBalance` variable. Whenever a balance changes for any restaked validator, anyone can update the balance for this specific validator by calling the `verifyRestakedBalance()` function. The function will check if the updated balance is correct through the beacon state root, which we retrieved from the `BEACON_CHAIN_ORACLE`.

And that is how EigenLayer allows native restaking.

## Sign up for more research, updates, and announcements:

[Enter your email\\
Subscribe](https://www.blog.eigenlayer.xyz/ycie/#/portal)

Subscribe![EigenLayer Blog](https://www.blog.eigenlayer.xyz/content/images/size/w2000/2023/04/EigenLayer_Ghost_Background_01-2.png)

Research, Updates, and Announcements

[![Slashing on Mainnet is Coming Soon - What You Need to Know](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog-Header_slashing_mainnet.jpg)](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[Slashing goes live on EigenLayer mainnet April 17th. Get essential details on what slashing is, the implementation timeline, key reminders for all users, and specific guidance for AVSs, Operators, and Stakers in this comprehensive update.](https://www.blog.eigenlayer.xyz/slashing-on-mainnet-is-coming-soon-what-you-need-to-know/)

[![Intro to Slashing on EigenLayer: AVS Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_AVS-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[EigenLayer's slashing on mainnet arrives April 17th, enabling AVSs to set custom conditions, manage operators through Operator Sets, and implement Unique Stake Allocation—creating stronger security guarantees with targeted accountability.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-avs-edition/)

[![Intro to Slashing on EigenLayer: Stakers' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_stakers-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[Slashing arrives on EigenLayer mainnet April 17th. Learn how it impacts Stakers through opt-in requirements, allocation timelines, and monitoring responsibilities—plus why this feature completion enhances the risk/reward ecosystem for delegators.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-stakers-edition/)

[![Intro to Slashing on EigenLayer: Operators' Edition](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/04/EL_Blog_slashing_operators-edition.jpg)](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[EigenLayer's slashing launches April 17th, giving Operators control through opt-in participation, Operator Sets, and Unique Stake Allocation—isolating risks while creating new reward opportunities aligned with your specific risk profile.](https://www.blog.eigenlayer.xyz/intro-to-slashing-on-eigenlayer-operators-edition/)

[![The Future of EigenLayer Testing: New & Improved Testnets & Tooling Coming Soon](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[Eigen Labs expands EigenLayer protocol testing capabilities with multiple new testnets and developer tools. Explore Hoodi deployment, improved CLI, Self Slasher AVS, and local devnet toolchains—empowering AVS builders with robust, versatile environments for seamless integration.](https://www.blog.eigenlayer.xyz/the-future-of-eigenlayer-testing-new-and-improved-testnets-tooling-coming-soon/)

[![AI Beyond the Black Box: Inference Labs is Making Verifiable, Decentralized AI a Reality with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/AVS_Spotlight_Inference_BlogPost-Header.jpg)](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[Inference Labs is making AI verifiable on-chain with their Zero-Knowledge Verified Inference Network. By leveraging EigenLayer's security, they've created a system that makes dishonest behavior unprofitable while reducing proving times by 76%.](https://www.blog.eigenlayer.xyz/ai-beyond-the-black-box-inference-labs-is-making-verifiable-decentralized-ai-a-reality-with-eigenlayer/)

[![EigenLayer Update: Holesky Network Instability and Upcoming Sepolia Support](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/03/Blog-Post-Header---Gradient--3-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[Due to Holesky testnet instability following Ethereum's Pectra Upgrade, EigenLayer will launch on Sepolia Network (March 10) while maintaining Holesky support. EigenDA targeting Sepolia availability by late April.](https://www.blog.eigenlayer.xyz/eigenlayer-update-holesky-network-instability-and-upcoming-sepolia-support/)

[![The Trust Layer for AI Agents: How Ungate Wukong Leverages EigenLayer for Trust-Minimized Autonomous Agents](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/AVS_Spotlight_Ungate_BlogPost-Header.png)](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[What if you could know with certainty that an AI is acting independently? Ungate and EigenLayer build the trust layer for AI, where autonomous agents don't just claim independence—they prove it cryptographically through verifiable execution.](https://www.blog.eigenlayer.xyz/ungate-wukong-trust-layer-for-ai-agents/)

[![Redefining AVS: From Actively Validated to Autonomous Verifiable Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Header.png)](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[EigenLayer redefines AVS as Autonomous Verifiable Services, reflecting a powerful shift from how these services are validated to what they truly represent: systems that are self-sustaining, verifiable, and built for decentralized ecosystems.](https://www.blog.eigenlayer.xyz/redefining-avs-from-actively-validated-to-autonomous-verifiable-services/)

[![Scaling to $1B in Delegated Assets: How Pier Two Unlocked Growth with EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient--1-.png)](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[From early launch partner to securing over $1B in delegated ETH and EIGEN, Pier Two's success demonstrates how institutional operators can scale efficiently with EigenLayer's restaking framework to build market-leading positions.](https://www.blog.eigenlayer.xyz/pier-two-scaling-institutional-staking-through-eigenlayer-2/)

[![EigenLayer at ETHDenver 2025: Complete Event Guide](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/EigenLayer-at-ETHDenver_Blog-Header.png)](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[Your comprehensive guide to all things Eigen at ETHDenver 2025](https://www.blog.eigenlayer.xyz/eigenlayer-at-ethdenver-2025-complete-event-guide/)

[![Introducing Verifiable Agents on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/02/Blog-Post-Header---Gradient.png)](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[The “Level 1 Agent” is a standardized way for agents to integrate with verifiable tools by using AVSs.](https://www.blog.eigenlayer.xyz/introducing-verifiable-agents-on-eigenlayer/)

[![EigenLayer 2024 Year in Review: Building the Future of Open Innovation](https://www.blog.eigenlayer.xyz/content/images/size/w600/2025/01/EigenLayer-2024-Year-in-Review.png)](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[2024 was a monumental year for EigenLayer.\\
\\
Since launching our first staking contracts in June 2023, and following the full mainnet deployment of the EigenLayer protocol in April 2024, we’ve made great strides toward our vision of building coordination engines for driving open innovation. \\
\\
We launched the EigenLayer protocol,](https://www.blog.eigenlayer.xyz/eigenlayer-2024/)

[![AVS Spotlight: Hyperbolic](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/AVS_Spotlight_Twitter-Thread-Header.jpg)](https://www.blog.eigenlayer.xyz/avs-spotlight-hyperbolic/)

[Proof of Sampling by Hyperbolic: How EigenLayer Leverages PoSP, the Gold Standard for Verification\\
\\
Overview\\
\\
One of the biggest challenges in decentralized AI is ensuring reliable, efficient, and scalable validation. Hyperbolic, a decentralized AI infrastructure platform, has developed Proof of Sampling (PoSP) to address this issue. PoSP, created in collaboration](https://www.blog.eigenlayer.xyz/avs-spotlight-hyperbolic/)

[![Introducing: Slashing](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Slashing---Testnet.png)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[We are excited to introduce Slashing, a proposed protocol upgrade detailed in ELIP-002: Slashing, and the next significant step in the evolution of the EigenLayer protocol. This powerful upgrade introduces slashing, a critical tool for AVSs to enforce cryptoeconomic commitments.\\
\\
This proposed upgrade is the second EigenLayer Improvement Proposal (ELIP)](https://www.blog.eigenlayer.xyz/introducing-slashing/)

[![Introducing: Rewards v2](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/12/Rewards-v2-2.png)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[We are excited to announce the arrival of the Rewards v2 protocol upgrade on Mainnet. It is designed to bring greater flexibility, efficiency, and customization to rewards within the EigenLayer ecosystem. This upgrade is also the first EigenLayer Improvement Proposal (ELIP), ELIP-001: Rewards v2, using the EigenLayer Governance process (EigenGov)](https://www.blog.eigenlayer.xyz/rewards-v2/)

[![Fragmentation to Fusion: Intention is All You Need (Part 2)](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents hold the potential to transform how we interact with Ethereum, yet today, they risk creating fragmented “walled gardens” where each intent-based application exists in](https://www.blog.eigenlayer.xyz/intention-is-all-you-need-2/)

[![Intention is All You Need](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/Screenshot-2024-10-29-at-3.04.46-PM--1-.png)](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[Thanks to Kevin from Khalani, Hart and Nick from Across, Khushi from Predicate, Jon from Hyperlane, Soubhik, and Brandon for reviewing drafts of this piece.\\
\\
Intents are transforming how we think about interactions on Ethereum.\\
\\
Instead of rigid transaction paths, intents open up a world where users declare their desired](https://www.blog.eigenlayer.xyz/intention-is-all-you-need/)

[![EigenLayer x LayerZero: The CryptoEconomic DVN Framework](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/10/LAYER-ZERO.png)](https://www.blog.eigenlayer.xyz/dvn/)

[Eigen Labs, in partnership with LayerZero Labs, is introducing a framework for CryptoEconomic Decentralized Verifier Networks (DVNs). Eigen Labs chose LayerZero because it is one of the most battle-tested protocols in crypto, handling millions of messages and securing billions for apps.\\
\\
As the first step of this integration, we are](https://www.blog.eigenlayer.xyz/dvn/)

[![Celebrating Commit-Boost’s Journey Towards Mainnet](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Commitboost.png)](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[Eigen Labs is excited to announce that Commit-Boost, an open-source public good that we have proudly supported since its inception, is moving towards audit and production on the Ethereum mainnet. This milestone, and the community now behind it, marks a significant step forward in ensuring the healthy growth of the](https://www.blog.eigenlayer.xyz/celebrating-commit-boost/)

[![Intelligent DeFi](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Intelligent-DeFi-B.png)](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[Thanks to Karthik, Ismael from Lagrange, Nikhil from Aethos, Soubhik, Gautham, and Brandon for reviewing drafts of this piece.\\
\\
Ethereum gave birth to DeFi with the launch of Maker in December 2017. Uniswap and Compound launched soon after, forming an economy around ETH and ERC20s. Since then, we’ve witnessed](https://www.blog.eigenlayer.xyz/intelligent-defi/)

[![Introducing Programmatic Incentives v1](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Programmatic-Incentives-v1-1.png)](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

[The Eigen Foundation is excited to announce the upcoming Programmatic Incentives v1 release, a new EigenLayer protocol feature, providing programmatic EIGEN rewards to stakers and operators for their active participation in supporting AVSs.\\
\\
What\\
\\
Programmatic Incentives v1 will enable weekly programmatic rewards of newly-minted EIGEN tokens to qualifying stakers and](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/)

[![Introducing the EigenLayer Security Model: A Novel Approach to Operating and Securing Decentralized Services](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Security-Model-1.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[Eigen Labs is excited to introduce the EigenLayer Security Model, an evolution of our previous model, in preparation for the upcoming ‘Slashing’ protocol feature release.\\
\\
This blog post is a simple description of how EigenLayer empowers AVSs to operate more efficiently and create economically aligned incentives using three foundational concepts:](https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model/)

[![Introducing the EigenPod Upgrade](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/09/Introducing-EigenPod-Upgrade.png)](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[Eigen Labs is pleased to announce the EigenPod upgrade, a major update to the EigenLayer protocol, making native ETH restaking on EigenLayer easier and more rewarding.\\
\\
What\\
\\
The EigenPod upgrade introduces a novel balance checkpointing system for managing Ethereum validator and EigenPod balances.\\
\\
When\\
\\
All EigenPods on mainnet have been](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/)

[![Introducing Updated EigenDA Pricing: Unlocking Greater Value and Accessibility](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Pricing-Update.png)](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[EigenDA’s mission is to scale the decentralized world by making reliable, scalable, and secure data availability (‘DA’) abundant. In pursuit of this goal, and enabled by the scalability of its design, EigenDA strives to be the most price-performant DA solution.\\
\\
Today, we are pleased to announce a 10x reduction](https://www.blog.eigenlayer.xyz/eigenda-updated-pricing/)

[![Community Update: Airdrops and the EigenLayer Ecosystem Network](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/communityupdate.png)](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[CoinDesk published an article on airdrops in our ecosystem and Eigen Labs employee participation in those airdrops. We’ve written a community update on what happened, why it happened, and steps we’ve taken. \\
\\
We want to make clear that we have no knowledge or evidence of any employee at](https://www.blog.eigenlayer.xyz/community-update-eigenlayer-ecosystem-network/)

[![Coming Soon: Permissionless Token Support on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/comingsoonpermissionless.png)](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[Eigen Labs is proud to introduce Permissionless Token Support, an upcoming update, to the EigenLayer protocol. This feature will enable any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic](https://www.blog.eigenlayer.xyz/permissionless-token-support/)

[![Introducing EigenDA Base Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/08/EigenDA-Base-Rewards-5.png)](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[Last week EigenLayer experienced a major upgrade. The Rewards protocol, the onchain system that enables AVSs to distribute rewards to their stakers and operators, went live on mainnet on August 6th. The Rewards protocol upgrade grants AVSs the ability to begin rewarding stakers and operators for their past, present, and](https://www.blog.eigenlayer.xyz/introducing-eigenda-base-rewards/)

[![Principles and Best Practices to Design Solidity Events in Ethereum and EVM](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/mainheader.png)](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[Solidity events are a crucial feature of Ethereum and EVM blockchains, with a vast number of use cases within the ecosystem. Primary use cases, for example, include but not limited to\\
\\
\\* Logging: where events provide a mechanism to log critical actions and state changes inside smart contract, for track contract](https://www.blog.eigenlayer.xyz/principles-and-best-practices-to-design-solidity-events-in-ethereum-and-evm/)

[![Coming Soon: AVS Rewards and EIGEN Programmatic Incentives](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/07/b-1.png)](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[Summary\\
\\
\\* Actively Validated Services (AVSs) are gaining the ability to reward stakers and operators.\\
\\* At least 4% of EIGEN's total supply will be distributed to stakers and operators via upcoming programmatic incentives.\\
\\* These incentives will be distributed via "rewards-boosts" in which stakers and operators will receive](https://www.blog.eigenlayer.xyz/coming-soon-avs-rewards-and-eigen-programmatic-incentives/)

[![Request for AVS: Uniswap v4 Hooks](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/1200x630.png)](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[Occasionally, you hear an idea so exciting that you see the future with utter clarity. At Eigen Labs, we’re excited to work with exceptional builders and hear their special insights about the world on a daily basis. We want to share some of ours with you. We’re starting](https://www.blog.eigenlayer.xyz/univ4-hooks/)

[![Holesky Launch: AVSs Can Now Test Restaker and Operator Rewards](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/06/rewards-claim5-630bdf4627a2efba655108a1f5648581.png)](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[We're proud to announce the launch of the MVP experience for Rewards on the Holesky testnet. This MVP allows AVSs to begin integrating with the rewards system, and test setting up incentives for restakers and node operators. It also allows restakers to test claiming functionality for (worthless) testnet](https://www.blog.eigenlayer.xyz/rewards-mvp-launches-on-holesky/)

[![EigenDA Dual Quorum and Production Traffic Announcement](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/Screenshot-2024-05-20-at-12.53.32-PM.png)](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[We are supporting rollup production traffic on EigenDA mainnet available immediately. Anyone can now deploy their rollup and be whitelisted on our free tier. Interested rollup customers should fill out the EigenDA Contact Form in order to be approved for the initial free-tier usage phase. Please provide the ETH address](https://www.blog.eigenlayer.xyz/eigenda-dual-quorum-and-production-traffic-announcement/)

[![Onboarding Rollups to EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/05/eigenda-Blog-post---1.png)](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[Following the EigenDA mainnet launch announcement on April 9, we are pleased to announce that rollups can now onboard onto EigenDA mainnet for test traffic. We have been working hard in sync with our community of restakers and DA operators to achieve our system performance goals in order to enable](https://www.blog.eigenlayer.xyz/onboarding-rollups-to-eigenda/)

[![EIGEN: The Universal Intersubjective Work Token](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Exploring-EIGEN-Banner.png)](https://www.blog.eigenlayer.xyz/eigen/)

[Over the past few years, we at Eigen Labs have been developing a platform for advancing the concept of open, verifiable digital commons. This blog post summarizes the intersubjective forking protocol enabled by the EIGEN token. We will break down the significance of EIGEN, its core ideas, its high-level implementation,](https://www.blog.eigenlayer.xyz/eigen/)

[![Accelerate Rollup Deployment with EigenDA's RaaS Marketplace](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/b2--1-.png)](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[Teams are launching high performance, low cost rollups and rollup services on EigenDA. In this post we’ll take a look at some of them and learn how you can get started building your own rollup.\\
\\
Rollups as a Service (RaaS) provide everything you need to build, customize, and deploy](https://www.blog.eigenlayer.xyz/accelerate-rollup-deployment-with-eigendas-raas-marketplace/)

[![Unpausing Restaking Deposits on April 16th!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/Ghost_05.png)](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[We're excited to announce the culmination of our phased rollout with the removal of all Liquid Staking Token (LST) caps and unpausing deposits, effective April 16th, 2024, at 9:00 AM PST. This milestone marks the next chapter in EigenLayer's journey, fostering a wide-open and dynamic](https://www.blog.eigenlayer.xyz/unpausing-restaking-caps-on-april-16th/)

[![EigenLayer AVS Mainnet Launch](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/medium-banner_option4.png)](https://www.blog.eigenlayer.xyz/avs-launch/)

[EigenLayer Mainnet Launch Expands Ecosystem with Additional AVSs\\
\\
The EigenLayer ecosystem takes another leap forward with the launch of Stage 3 on mainnet! Following the launch of EigenDA as the first AVS on EigenLayer, this stage introduces a powerful group of AVSs – AltLayer’s MACH restaked rollups with Xterio being](https://www.blog.eigenlayer.xyz/avs-launch/)

[![Mainnet Launch Announcement: EigenLayer ∞ EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/lightbox_launch_0409-3.jpeg)](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[The Start of Infinite Sum Games\\
\\
Not long ago, EigenLayer was just an idea. Restaking launched on Ethereum mainnet in June 2023. In the ensuing months, we’ve been deeply inspired by the crypto community’s enthusiasm for the EigenLayer vision. Today, over 4.1 million ETH are restaked on](https://www.blog.eigenlayer.xyz/mainnet-launch-eigenlayer-eigenda/)

[![Fhenix: FHE Coprocessor on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/04/image1.png)](https://www.blog.eigenlayer.xyz/fhenix/)

[Fhenix and EigenLayer Join Forces to Pioneer FHE Coprocessors, Revolutionizing Onchain Confidentiality on Ethereum\\
\\
We are excited that FHE Coprocessor will be building on EigenLayer and to announce the development of FHE-based coprocessors in collaboration with Fhenix.\\
\\
FHE coprocessors are secured by Fhenix’s optimistic FHE rollup infrastructure and EigenLayer’](https://www.blog.eigenlayer.xyz/fhenix/)

[![EigenLayer Holesky Testnet Launch + Dual Quorum Support for EigenDA](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/Twitter---22.png)](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[The EigenLayer and EigenDA Holesky Testnet is now up and running. This marks an important milestone for the EigenLayer ecosystem, and we're keen for operators, stakers and rollup developers to continue testing on Holesky as we gear up for the upcoming mainnet launch. As a reminder: points are](https://www.blog.eigenlayer.xyz/eigenlayer-holesky-testnet-launch-dual-quorum-support-for-eigenda/)

[![On Liquid Restaking: Risks & Considerations](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/2024-03-21-13.07.06.jpg)](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[In recent months, liquid restaking protocols and liquid restaking tokens (LRTs) have seen tremendous growth, and are beginning to become systemically important in the EigenLayer ecosystem. There are many different flavors of LRT, each with different values, features, and offerings. All of these LRT projects are independent of the underlying](https://www.blog.eigenlayer.xyz/liquid-restaking/)

[![Ethos: Powering the Convergence Era of Blockchains](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/ethosxeigen.png)](https://www.blog.eigenlayer.xyz/ethos/)

[Blockchains are entering an era of convergence, characterized by the dissolution of boundaries between different network architectures. Cosmos started as a network of appchains, with each chain independently establishing its validator set and trust protocol using native tokens. Ethereum, on the other hand, chose a shared security approach where every](https://www.blog.eigenlayer.xyz/ethos/)

[![EigenLayer Mainnet: Preparation for launch!](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/x--2-.png)](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[We're thrilled to announce the next chapter in our journey towards a secure and successful EigenLayer mainnet launch! Our primary focus remains a smooth launch that prioritizes both security and performance. To achieve this, we're introducing a multi-phased approach starting now and ongoing for the next](https://www.blog.eigenlayer.xyz/eigenlayer-mainnet-preparation-for-launch/)

[![Security Bounty Competition on EigenLayer by Cantina](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/03/IMAGE-2024-03-07-12-54-55.jpg)](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[Cantina is a marketplace for web3 security that gives protocols the flexibility to easily book a security review with their desired team, price, and timeline. They have recently launched a competition focused on EigenLayer. This competition presents an exciting opportunity for security researchers and professionals to contribute to the safety](https://www.blog.eigenlayer.xyz/security-bounty-competition-on-eigenlayer-by-cantina/)

[![Ritual ♾️ EigenLayer: AI × Restaking](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/pasted-image-0.png)](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[Summary\\
\\
The worlds of artificial intelligence and onchain protocols are increasingly intersecting as permissionless protocols look to unlock new customer behavior around ownership and markets by utilizing AI models. We’ve witnessed how in offchain settings, AI models could dramatically improve on the status quo of problem solving across various](https://www.blog.eigenlayer.xyz/ritual-eigenlayer-ai-x-restaking/)

[![Accelerating Ethereum Together: Eigen Labs ∞ a16z crypto](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/photo_5096171691515686419_y.jpg)](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[We founded Eigen Labs in 2021 with the mission to empower open innovation across the blockchain infrastructure stack via building the EigenLayer protocol. EigenLayer provides blockchain developers with access to the highly crypto-economically secure and decentralized network that powers Ethereum, which enables them to more easily launch new protocols and](https://www.blog.eigenlayer.xyz/accelerating-ethereum-together-a16z-crypto-x-eigen-labs/)

[![Inco: Building an Universal Confidential Computing L1 on EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/----5-.png)](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[One of the open challenges in the blockchain industry is achieving trustless confidentiality. The inherent transparent nature of public blockchains prevents the development of applications requiring on-chain confidentiality across gaming, decentralized finance (DeFi), governance, and identity without relying on a trusted third party.\\
\\
Current approaches to providing privacy on the](https://www.blog.eigenlayer.xyz/inco-building-an-universal-confidential-computing-l1-on-eigenlayer/)

[![Balancing Neutrality and Decentralization in EigenLayer](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/EigenLayer_Ghost_balancing2_1920x1080_01.png)](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[Today, EigenLayer has unpaused token restaking and removed TVL caps for every token. While the unpause is temporary this time, in the coming months the pause and caps will be lifted permanently. In this post, we reflect on the challenges of balancing neutrality and decentralization in the protocol, and suggest](https://www.blog.eigenlayer.xyz/balancing-neutrality-and-decentralization-in-eigenlayer/)

[![With New Arbitrum Orbit Integration, EigenDA and AltLayer Bring Horizontal Scalability to the Ethereum Ecosystem](https://www.blog.eigenlayer.xyz/content/images/size/w600/2024/02/Untitled.png)](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

[Feb 2, 2024 – EigenLabs, along with Offchain Labs and AltLayer, today announced EigenDA support for Arbitrum Orbit chains, bringing scalability to the Ethereum ecosystem without sacrificing on security. The integration offers developers the ability to build EigenDA-based Orbit rollups that bridge from Arbitrum One, Arbitrum Nova, and Ethereum, and boast](https://www.blog.eigenlayer.xyz/eigenda-altlayer-arbitrum-orbit/)

Subscribe