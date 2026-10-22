# Awesome-LLM-Training-Frameworks

# Top LLM Training Frameworks Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Distributed Pre-Training, Fine-Tuning Pipelines & Self-Hosted Training Infrastructure*  
**Last updated: October 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **LLM Training Frameworks**. These tools provide the infrastructure for training and fine-tuning large language models—handling distributed parallelism, memory optimization, mixed precision, and experiment orchestration—so teams can train models from scratch or adapt existing ones without rebuilding the training stack.

**Examples** include CoreWeave, Lambda Labs, Together AI, Nebius AI, and the open-source frameworks EleutherAI GPT-NeoX, Megatron-LM, DeepSpeed, and Colossal-AI (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active framework for self-hosting, avoiding GPU cloud vendor lock-in, and controlling the training pipeline—ideal for research labs, enterprises with sensitive data, and teams that want to understand every layer of the training stack. The open-source landscape is anchored by **Megatron-LM** (NVIDIA's peak-performance engine achieving 47% MFU on 6,144 H100 GPUs), **DeepSpeed** (Microsoft's ZeRO optimization standard), **Colossal-AI** (the unified parallelism framework), and **GPT-NeoX** (EleutherAI's research-grade training library), with **LLaMA-Factory** and **Axolotl** serving fine-tuning workflows and **Oumi** emerging as a full-lifecycle open platform.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[CoreWeave](https://www.coreweave.com/)**  
  NVIDIA-only GPU cloud built for large-scale training. **Critical caveat**: Hopper and Blackwell HGX instances rent **eight GPUs at a time**—single-GPU provisioning is not available for H100/B200. An 8x H100 node lists at **$49.24/hour** ($6.16/GPU-hour); 8x B200 at **$68.80/hour** . Reserved multi-year contracts offer up to 60% discounts via negotiation. Best for teams that can commit to full-node training workloads.

- **[Lambda](https://lambda.ai/)**  
  AI cloud with first-party datacenters and consistent instance behavior. **On-demand pricing**: 1x H100 SXM **$4.29/GPU-hour**, 8x H100 SXM **$3.99/GPU-hour**. **1-Click Clusters** with InfiniBand are the differentiated product for multi-node training runs. Best for teams wanting hardware consistency and straightforward per-GPU pricing.

- **[Together AI](https://www.together.ai/)**  
  **GPU cluster pricing** (raw compute, you run the training stack): H100 **$3.99/GPU-hour** on-demand or **$3.09** reserved (181+ days); H200 **$5.99** / **$3.99**; B200 **$8.19** / **$6.79** . Best for teams that want to start with dedicated endpoints and graduate to full clusters as training scale grows.

- **[Nebius AI](https://nebius.com/)**  
  European-headquartered GPU cloud with published pricing. **From October 1, 2026**: H200 NVLink at **$5.40/GPU-hour**, H100 NVLink at **$4.50/GPU-hour** . Best for teams needing EU data residency and predictable published rates without sales negotiation.

- **[Crusoe Cloud](https://www.crusoe.ai/)**  
  GPU cloud with spot, on-demand, and reserved options. Published H200 rates around **$4.29/GPU-hour** and H100 80GB around **$3.90/GPU-hour**. Best for flexible contract terms and a mix of raw compute and managed services.

- **[RunPod](https://www.runpod.io/)**  
  GPU cloud with per-second billing for serverless and pod-based training. Best for short training runs and experimentation where per-second economics matter more than reserved cluster pricing.

## Open-Source GitHub Projects

- **[Megatron-LM](https://github.com/NVIDIA/Megatron-LM)**  
  **NVIDIA's peak-performance training engine.** The reference implementation for large-scale transformer training, with **optimized parallelism strategies** covering Data Parallel (DP), Fully Sharded Data Parallel (FSDP, ~15% faster than PyTorch FSDP2), Tensor Parallel (TP), Pipeline Parallel (PP), Context Parallel (CP), and Expert Parallel (EP) for MoE models . **Benchmark results**: trains models from 2B to 462B parameters across thousands of GPUs, achieving **up to 47% Model FLOP Utilization (MFU)** on H100 clusters (6,144 GPUs for a 462B model) . **Strengths**: production-ready pipeline with checkpointing and fault tolerance; FP8 support on Hopper/Ada/Blackwell; Transformer Engine integration. **Trade-offs**: steep learning curve; tightly coupled to NVIDIA hardware and CUDA ecosystem. Best for teams training frontier-scale models on NVIDIA infrastructure with dedicated engineering capacity.

- **[DeepSpeed](https://github.com/deepspeedai/DeepSpeed)**  
  **Microsoft's ZeRO optimization standard.** 250K+ GitHub stars, Apache 2.0 licensed . **Core contribution**: the **ZeRO (Zero Redundancy Optimizer)** family, which partitions optimizer states, gradients, and parameters across GPUs to eliminate memory redundancy—enabling training of models that would otherwise not fit. **Ecosystem position**: DeepSpeed is the backend for many higher-level frameworks (including Hugging Face Trainer and GPT-NeoX via DeeperSpeed). **Strengths**: mature, widely adopted, and integrates with PyTorch seamlessly. **Trade-offs**: configuration complexity; the ZeRO stages require understanding trade-offs between memory and communication. Best for teams that want proven memory optimization without adopting a full training framework from scratch.

- **[Colossal-AI](https://github.com/hpcaitech/ColossalAI)**  
  **The unified parallelism framework.** 41K+ GitHub stars, Apache 2.0 licensed . **Demonstrated results**: 70B LLaMA2 training accelerated by **195%**; GPT-3 training with **50% GPU resource savings** and **10.7% acceleration**; GPT-2 with **11x lower GPU memory** and superlinear tensor parallelism scaling; 24x larger model size on the same hardware . **Strengths**: broad parallelism support (data, tensor, pipeline, sequence) with a unified API; strong single-GPU optimization (20x larger model on same hardware via NVME offload) . Best for teams that want a single framework covering multiple parallelism strategies with demonstrated speedups across model sizes.

- **[GPT-NeoX](https://github.com/EleutherAI/gpt-neox)**  
  **EleutherAI's research-grade training library.** The training code behind GPT-NeoX-20B and other open models . **Architecture**: built on **DeeperSpeed** (EleutherAI's fork of DeepSpeed), with support for Flash Attention, fused kernels, and the standard training/evaluation/generation loop . **Usage pattern**: launched via `deepy.py` (a wrapper around the DeepSpeed launcher), with `train.py`, `evaluate.py`, and `generate.py` as entry points . **Strengths**: proven at 20B scale; transparent, research-oriented codebase; full training and evaluation pipeline open-sourced alongside model weights . **Limitations**: Python 3.8-era dependencies (some libraries not compatible with Python 3.10+); development pace has slowed relative to newer frameworks. Best for research teams wanting a transparent, proven training stack with full pipeline visibility.

- **[LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory)**  
  **The unified fine-tuning framework.** Supports a wide array of models (LLaMA, LLaVA, Qwen, Mistral) with methods including pre-training, multimodal fine-tuning, reward modeling, PPO, DPO, and ORPO . **Strengths**: web UI (LLaMA Board) for non-expert users; broad model coverage; one-stop workflow from data preparation to export. Best for teams that primarily need fine-tuning and alignment rather than pre-training from scratch.

- **[Axolotl](https://github.com/axolotl-ai-cloud/axolotl)**  
  **The YAML-driven production fine-tuning framework.** Config-as-artifact for reproducibility; supports SFT, DPO, GRPO, and multi-GPU via DeepSpeed/FSDP. Best for production-grade fine-tuning where the config file is the deliverable.

- **[Oumi](https://github.com/oumi-ai/oumi)**  
  **The full-lifecycle open platform.** 9,185+ GitHub stars, Apache 2.0 licensed . **Coverage**: from data preparation and training to evaluation and deployment; 200+ recipes for 100+ models including gpt-oss, Qwen3, and DeepSeek-R1 . Best for teams that want a single platform covering the entire model lifecycle rather than assembling separate tools.

### Additional Strong Open-Source Options

- **VeRL (HybridFlow)** — Open-source implementation of the HybridFlow framework, supporting RL algorithms (PPO, GRPO) with scalability up to 70B models on hundreds of GPUs . Best for teams training reasoning models with RL post-training.
- **TRL** — Hugging Face's library for RLHF and post-training. Best for alignment workflows that follow pre-training.
- **Gym (formerly LLaMA Factory fork)** — Non-profit project under Zoo Labs Foundation with a focus on accessibility and zero vendor lock-in. Apache 2.0 licensed . Best for researchers wanting an education-first training platform.
- **Frameworks for building custom systems**: Use **Megatron-LM** for frontier-scale pre-training on NVIDIA hardware. Use **DeepSpeed** as the memory optimization layer inside your training loop. Use **Colossal-AI** for a unified parallelism API with demonstrated speedups. Use **GPT-NeoX** for research-grade transparency. Use **LLaMA-Factory** or **Axolotl** for fine-tuning. Use **Oumi** when you want a single platform from data to deployment.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- **Training frameworks assume different levels of expertise**: Megatron-LM and Colossal-AI require deep understanding of distributed systems; LLaMA-Factory and Axolotl are accessible to fine-tuning practitioners. Match the tool to your team's skills and your training scope (pre-training vs. fine-tuning).
- **GPU cloud minimum provisioning matters**: CoreWeave rents H100/B200 in units of 8 GPUs—factor this into cost comparisons against providers offering single-GPU provisioning .
- **Framework benchmarks are hardware-specific**: Megatron-LM's 47% MFU is measured on H100 clusters; performance on other hardware will differ. Benchmark against your actual infrastructure before committing.
- **GPT-NeoX has dated dependencies**: Python 3.8-era library compatibility may complicate new deployments; evaluate whether newer frameworks better suit your environment .

---

**Made for ML engineers, research scientists, and organizations building training infrastructure for large language models.**  
Let's make LLM training more open, scalable, and appropriately scoped.
