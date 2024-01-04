---
title: How to undo your git failure?
tags: [External Post, Git]
style: fill
color: warning
description: Using `git reflog` and `git reset` to save your code.
external_url: https://blog.usejournal.com/how-to-undo-your-git-failure-b76e31ecac74

---
# Data Modalities

- Audio to Image: mel spectrograms
- Video to Image
- Text to Video: take a photo of text
- Data to Image: Chart
- Image to Text: Image to vector to tokens of text
- Audio to Text

# Multimodal Tasks

- Vision Language Tasks
  - Generation
    - Image Generation (text-to-image synthesis)
      - Model: Dall-E, Stable Diffusion, and Midjourney
    - Text Generation
      - Visual Question Answering (VQA)
  - Vision-Language Understanding (VLU)
    - Classification
    - Text-Based Image Retireval (TBIR) (image search)
      - Possible Approaches
        - Generate caption and metadata for image
          - given a text query, find images whose captions/metadata are closest to this text query
        - Train a joint embedding space for both images and text
          - Given a text query, generate an embedding for this query,
          - and find all images whose embeddings are closest to this embedding.

# Fundamentals of Multimodal Trainging

- Three Components

  - Encoder for each data modality
  - A way to align embeddings of different modality
    - into the same multimodal embedding space
  - (generative model only) A language model to generate text responses.
- CLIP: map data of different modalities, text and images, into a shared embedding space.![img](https://huyenchip.com/assets/pics/multimodal/4-CLIP-architecture.png)
- Flamingo: CLIP + a language model![img](https://huyenchip.com/assets/pics/multimodal/13-flamingo-architecture.png)

  - **Vision encoder**: a CLIP-like model
  - **Language model**: finetuned Chinchilla
    - predicts the next text token based on both the preceding text and visual tokens
  - Perceiver Resampler![img](https://huyenchip.com/assets/pics/multimodal/16-flamingo-perceiver-resampler.png)
  - GATED XATTN-DENSE layers![img](https://huyenchip.com/assets/pics/multimodal/17-gated%20xattn-dense.png)

# Reading List

* Incorporating more data modalities
  * [ULIP: Learning a Unified Representation of Language, Images, and Point Clouds for 3D Understanding](https://arxiv.org/abs/2212.05171) (Xue et al., Dec 2022)
  * [ImageBind: One Embedding Space To Bind Them All](https://browse.arxiv.org/abs/2305.05665) (Girdhar et al., May 2023)
  * [NExT-GPT: Any-to-Any Multimodal Large Language Model](https://next-gpt.github.io/) (Wu et al., Sep 2023)
* Multimodal systems for instruction-following
  * [MultiInstruct: Improving Multi-Modal Zero-Shot Learning via Instruction Tuning](https://arxiv.org/abs/2212.10773) (Xu et al., Dec 2022)
  * [LLaVA: Visual Instruction Tuning](https://arxiv.org/abs/2304.08485) (Liu et al., Apr 28, 2023)
  * [InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning](https://arxiv.org/abs/2305.06500) (Salesforce, May 11, 2023)
  * LaVIN: [Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models](https://arxiv.org/abs/2305.15023) (Luo et al., May 24, 2023)
* Adapters for more efficient multimodal training
  * [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597)
  * [LAVIN] [Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models](https://arxiv.org/abs/2305.15023)
  * [LLaMA-Adapter V2: Parameter-Efficient Visual Instruction Model](https://arxiv.org/abs/2304.15010)
