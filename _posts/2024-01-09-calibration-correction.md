---
title: Some Notes of Calibration
tags: [Notes, Paper Reading, LLMs]
style: fill
color: primary
description: Some notes of calibration (self evaluation, self-correction)
---
# [Language Models (Mostly) Know What They Know](https://arxiv.org/abs/2207.05221)

## Introduction

- study the extent to which Language Models (LMs) possess this ability and how it can be elicited and imparted

  - do the probabilistic predictions from language models match up with frequencies of occurrence?
  - using models to evaluate the accuracy of their own outputs
  - ‘brainstorming other possibilities’ helps large models to evaluate the validity of a given answer option
  - having language models attempt to directly evaluate their own state of knowledge
- Glossary: Observables and Metrics

  - Expected Calibration Error (ECE)
    - first divide the probability interval [0, 1] into multiple *bins*
    - ECE is calculated as a weighted average of the accuracy/prediction error across the bins

      - weighted on the relative number of samples in each bin.
    - $$
      ECE = \sum_{m=1}^{M} \frac{\mathopen| B_m \mathclose |}{n} \mathopen| acc(B_m) - conf(B_m)\mathclose|
      $$
    - [Examples](https://towardsdatascience.com/expected-calibration-error-ece-a-step-by-step-visual-explanation-with-python-code-c3e9aa12937d)

## Larger Models are Calibrated on Diverse Multiple Choice Questions

- Calibrated predictions
  - A model makes calibrated predictions if the probability it assigns to outcomes coincides with the frequency with which these outcomes actually occur.
- Language models can produce well-calibrated probabilities when they are asked to choose the correct answer from among several explicit options.


## From Calibration to Knowing What You Know

### Replacing an Option with ‘None of the Above’ Harms Performance and Calibration

- whether the model actually knows whether each of the answer options is correct, when judged independently
  - modified our multiple choice evaluations by replacing their final option with “none of the above”
  - make questions that do not actually have a unique correct answer ambiguous or impossible
- adding "none of the above" harms calibration

### Models are Well-Calibrated on True/False Tasks

- simply ask models if a given answer is true or false

### RLHF Policy Miscalibration Can Be Remediated with a Temperature Tuning


## Ask the AI: Is your proposed answer True or False?

- apply the True/False approach to the samples models generated when trying to answer questions
  - Zero-shot, P(True) is poorly calibrated, and typically it lies close to 50% for typical samples
  - Showing Many T = 1 Samples Improves Self-Evaluation

# [Just Ask for Calibration: Strategies for Eliciting Calibrated Confidence Scores from Language Models Fine-Tuned with Human Feedback](https://arxiv.org/abs/2305.14975)

## Introduction

- RLHF-LMs may sacrifice well-calibrated predictions for the sake of closer adherence to user instructions in dialogue
- This paper evaluates several methods for extracting confidences about model predictions from RLHF-LMs
- prompts that elicit verbalized probabilities
  - i.e., the model expresses its confidence in token-space
    - as either numerical probabilities or another linguistic expression of uncertainty
- popular RLHF-LMs are able to directly verbalize confidence scores
  - that are better-calibrated than the model’s conditional probabilities (estimated via sampling)

## Evaluating Calibration in RLHF-LMs

- Metrics
  - ECE
    - bin model predictions by their confidence and measure the average accuracy of predictions in each confidence bin
  - ECE-t
    - ECE with temperature scaling
    - Temperature scaling fits a single temperature value β to the model’s confidences to minimize negative log likelihood (NLL) on the data
  - BS-t
    - Brier Score on temperaturescaled confidences
    - the mean squared error between the confidences and the correctness labels
  - AUC
    - the area under the curve of selective accuracy and coverage
- Datasets
- Evaluation protocol
- Methods


## Results


## Discussion
