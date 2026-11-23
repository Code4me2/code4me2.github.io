---
title: "Homelab"
date: 2026-11-23
draft: false
summary: "A quick post about my homelab, how I use it, and why I have it."
tags: ["tech"]
---

<div class="post-subtitle">

*A quick post about my homelab, how I use it, and why I have it*

</div>

![My homelab cluster: four GX10-class units and a DGX Spark, stacked on books](/images/homelab.jpg)

Early in 2024 I liberated my brother's old PC to use its GPU for testing local LLMs and ML models. At the time, the PC had 8GB of VRAM and 16GB of RAM. It was fine for gaming, but as I got into the ML space I realized what a constraint those numbers would turn out to be. I began investing most of my earnings from work into hardware to modify his PC, and eventually to build others.

I have now built ~3 PCs for ML workloads, and the GPUs (and now RAM) have consistently been the most expensive components. To be able to run models with ~400 GB of weights would require a 4x cluster of RTX 6000 pro blackwell chips (and the supporting CPU and ram), which at current prices totals somewhere around $85k. So, I went to the DGX spark line from Nvidia (with a GB10 architecture) to solve some of my memory needs. While ARM unified memory performs slower parallel computation than a discrete GPU, I can cluster them with a high bandwidth interconnect (200 Gb/s) to get a decent throughput for larger models. Given that the rtx 6000s would also be bottlenecked by the PCIE without nvlink (which isn't supported), it made sense to get machines that are slightly slower, with a better capacity for high speed networking. That way I can scale up if needed without committing further overhead to rack scale solutions.

The following are some of the **use cases** for the cluster pictured above:

* Fine tuning and quantizing LLMs models
* Running continual personal agent inference
* Kaggle competition compute for training ML models
* Inference engine testing and optimization
* Enable peer access for robotics model fine tuning, deep learning or NLP class projects
* High-bandwidth network fiber testing for simulated environments and larger scale projects.
* Financial data analysis
* Inference for analyzing and aggregating Arxiv papers (see chiya-library)

For a multitude of reasons, mixture of expert models (MoEs) have become increasingly popular as the cluster has come together. Dense model architectures (where every parameter activates for each token produced) are computationally expensive, and slow down significantly when operating on unified memory. A sparser model (MoEs where there are less active parameters per decode step) on the other hand, get significant gains in speed and throughput on unified memory when compared to dense models. As sparsity has become more prominent, so has distillation and intelligence per parameter. The following is a visualization of **intelligence density** over time, accounting for sparsity:

{{< intelligence-density >}}

The result of these trends in compute and memory efficiency mean that the value of hardware capable of running these models has also increased over time. So, I have been able to better leverage each individual machine to support more capable agents, and thus find more uses for the same cluster. Many peers of mine, both in college and already in industry, want to learn more and experiment with different models, inference engines, model fine tuning, but balk at the cost of renting cloud nodes, especially over extended periods of time. So, I have been connecting them into the cluster via tailscale to allow them to experiment whenever the machines are idle. This has the added effect of growing a community around local experimentation and hacking. Further updates will be included on the same post.
