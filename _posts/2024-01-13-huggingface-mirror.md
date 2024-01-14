---
title: Huggingface Mirror
tags: [Notes, Paper Reading, LLMs]
style: fill
color: success
description: How to quickly download Hugging Face models?
---
# Use the huggingface-cli command-line tool provided by the official Hugging Face

- Install Dependencies
  - ```text
    pip install -U huggingface_hub
    ```
- Set environment variable
  - hf-mirror.com
    - ```bash
        export HF_ENDPOINT=https://hf-mirror.com
        ```
  - hf_transfer
    - ```bash
        export HF_HUB_ENABLE_HF_TRANSFER=1
        ```
- Download Model
  - ```bash
    huggingface-cli download --resume-download bigscience/bloom-560m --local-dir bloom-560m
    ```
- Download Datasets
  - ```bash
    huggingface-cli download --resume-download --repo-type dataset lavita/medical-qa-shared-task-v1-toy
    ```
  - Notes
    - It's worth noting that there is an optional parameter `--local-dir-use-symlinks False` because the default behavior of the Huggingface toolkit is to use symbolic links to store downloaded files.
    - This results in "link files" in the directory specified by `--local-dir`, with the actual models being stored in `~/.cache/huggingface`.
    - If you prefer not to use this symlinking behavior, you can disable it by using `--local-dir-use-symlinks False`.
