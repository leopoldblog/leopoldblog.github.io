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
  - TriviaQA
    - contains 650k question-answer pairs gathered by trivia enthusiasts
  - SciQ
    - contains approximately 14k crowdsourced science exam questionanswer pairs
  - TruthfulQA
    - contains 817 questions designed to test language models’ tendency to mimic human falsehoods
- Evaluation protocol
  - use either GPT-4 or GPT-3.5 to evaluate whether a response is essentially equivalent to
    the ground truth answer
- Methods
  - Sampling Estimation
    - Label prob.
    - ‘Is True’ prob.
  - Verbalization: the model expresses its confidence in token space
    - Verb. 1S top-k
      - produce k guesses and a probability that each is correct all in a single response
      - take the highest-probability prediction and its associated probability as the model’s output and confidence
    - Verb. 2S top-k
      - the model is first asked to provide only its answers
      - and afterwards, in a second round of dialogue, asked to assign probabilities of correctness to each answer.
    - Verb. 2S CoT
      - uses a chain-of-thought prompt before giving a single answer
  - Ling. 1S-human
    - prompted to assign confidences to its guesses by choosing from a set of linguistic expressions of uncertainty
      - {Almost certain, Likely, . . . , Almost no chance}
  - Ling. 1S-opt.
    - uses a held out set of calibration questions and answers to compute the average accuracy for each likelihood expression, using these ‘optimized’ values instead.


## Results

- Large RLHF-LMs can often directly verbalize better-calibrated confidences (either a numerical confidence probability or an expression such as ‘highly likely’) than the models’ conditional probabilities.
- Language models can express their uncertainty with numerical probabilities as well or better than with words, which is surprising in light of long-standing difficulties in representing numbers in language models
- Chainof-thought prompting does not improve verbalized calibration
