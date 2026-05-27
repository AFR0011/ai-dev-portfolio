# Reliable Two-Stage IDS - Support-Audited Trustworthy Intrusion Detection

## Overview

This project is my MSc thesis research: a support-audited trustworthiness evaluation framework for two-stage machine-learning-based intrusion detection.

The work treats intrusion detection as an evaluation and trustworthiness problem, not just a classifier-selection problem. The central idea is that high closed-set performance does not automatically imply deployment readiness. A model can perform well on known classes while failing under unknown attacks, invalid splits, calibration errors, drift, or unstable explanations.

## Research Aim

Design and evaluate a support-audited trustworthiness framework for two-stage ML-based intrusion detection systems.

The framework studies:

- strict closed-set performance,
- support-audited open-set holdouts,
- split validity,
- unknown-family detection,
- sink-family failure modes,
- calibration-aware abstention,
- alternative rejector methods,
- drift monitoring,
- explanation stability,
- external robustness stress tests.

## Problem

Many IDS studies report aggregate metrics such as accuracy, macro-F1, or ROC-AUC. These metrics are useful, but they can hide practical weaknesses.

A deployed IDS may encounter:

- attack families not present during training,
- shifted traffic distributions,
- uncertain samples,
- confidently wrong predictions,
- class imbalance,
- unsupported open-set evaluation scenarios,
- unstable explanations.

The thesis asks a stricter question:

> When is the evaluation scenario valid, and where does the model fail when it is asked to handle unsupported attack families?

## Two-Stage IDS Design

The system uses a two-stage architecture:

1. **Stage 1:** binary benign-vs-attack detection  
2. **Stage 2:** attack-family classification for attack-routed samples

This mirrors an operational IDS workflow: first decide whether traffic is suspicious, then classify the type of attack.

It also exposes a specific risk: an unknown attack may be detected as malicious but absorbed into the wrong known family by Stage 2.

## Protocols

### Protocol A - Closed-Set Anchor

Protocol A is the strict closed-set baseline. It evaluates the two-stage system when all test families are represented during training.

Protocol A is used as the main performance anchor, not as proof of deployment readiness.

### Protocol B - Support-Audited Open-Set Holdouts

Protocol B evaluates unknown-family behavior by holding out attack families.

A holdout is accepted only if the remaining train/validation/test split has enough support for known families and benign samples. This support audit prevents invalid scenario construction from being mistaken for model failure.

## Datasets

Primary datasets:

- CICIDS2017
- CICIoT2023

External robustness stress-test datasets:

- NSL-KDD
- UNSW-NB15

The external datasets are used as stress tests, not as direct transfer-validation evidence. A real transfer study would require a separate feature and label harmonization protocol.

## Models

The thesis uses classical tabular ML models:

- Random Forest
- XGBoost

This is intentional. The contribution is the trustworthiness evaluation framework, not a new neural architecture. Strong tabular baselines make it easier to study protocol validity, calibration, rejection, drift, and explanation behavior without turning the work into an architecture search.

## Main Results

### Closed-Set Protocol A

Random Forest is the practical closed-set anchor on the two primary datasets:

- **CICIDS2017:** system macro-F1 0.7165, system accuracy 0.9863
- **CICIoT2023:** system macro-F1 0.7850, system accuracy 0.9722

XGBoost remains competitive in some metrics, but Random Forest provides the stronger practical baseline across the primary thesis datasets.

### Operational strict_tau Overlay

The strict_tau overlay adds abstention/rejection behavior.

- On CICIDS2017, RF macro-F1 improves from 0.7165 to 0.7441.
- On CICIoT2023, RF macro-F1 remains effectively unchanged.

This makes strict_tau useful as an operational overlay, but not a replacement for the strict closed-set baseline.

### Protocol B Open-Set Behavior

Protocol B shows that unknown-family behavior is strongly family-dependent.

Examples:

- CICIDS2017 DoS is detected strongly as unknown, with unknown-detection rate 0.9911.
- CICIDS2017 DDoS collapses almost completely, with unknown-detection rate 0.0007.

This is the core result: strong closed-set performance does not guarantee reliable unknown-family handling.

### Split Validity

The CICIDS2017 split-validity result is central to the thesis.

The rejected whole-file Protocol B split produced zero eligible holdouts. The accepted recovered contiguous-within-day split produced six valid audited holdouts:

- Botnet
- BruteForce
- DDoS
- DoS
- Scan/Recon
- Web/App

This makes support auditing part of the contribution, not just a preprocessing detail.

### Sink-Collapse Failure Modes

Difficult unknown families often collapse into nearby known families:

- DDoS → DoS
- Web/App → BruteForce
- Botnet → Scan/Recon

This shows that open-set failures are structured. The model is not merely uncertain; it often maps unsupported families into semantically adjacent known families.

### Drift and Explanation Stability

The drift and explanation-stability layers separate the primary datasets:

- CICIDS2017 shows 18/20 feature-drift-only windows and mean explanation top-k Jaccard 0.8012.
- CICIoT2023 shows 10/10 stable windows and mean top-k Jaccard 1.0000.

This supports the thesis argument that trustworthiness should include dataset stability and explanation stability, not only static performance.

## Evaluation Layers

The framework includes:

- closed-set Protocol A anchor,
- support-audited Protocol B holdouts,
- split-validity analysis,
- unknown-detection rate,
- false-unknown rate,
- reject rate,
- sink-family failure analysis,
- calibration-aware abstention,
- tau, margin, entropy, and split-conformal rejectors,
- drift diagnostics,
- explanation-stability diagnostics,
- external robustness stress tests,
- frontier contribution review.

## Planned Extensions

The thesis evidence base supports several future publication tracks.

### 1. Support-Audited Open-Set IDS Benchmark

A paper focused on Protocol A/B, support auditing, split validity, unknown-family behavior, and sink-collapse failures.

### 2. Abstention and Rejection Methods for Open-Set IDS

A method-focused extension comparing tau, margin, entropy, conformal, and sink-aware rejection under operational cost constraints.

### 3. Drift-to-Action Maintenance Policies

A temporal reliability extension studying whether drift diagnostics can support conservative maintenance actions such as calibration refresh.

### 4. Cross-Dataset IDS Robustness and Harmonization

A separate transfer/harmonization study using feature ontology mapping, label alignment, and explicit cross-dataset protocol design.

## What This Demonstrates

This project shows how I approach trustworthy ML systems:

- build a strong baseline,
- audit the protocol before trusting the result,
- evaluate failure modes directly,
- report aggregate and family-level behavior,
- separate core claims from exploratory extensions,
- avoid claiming more than the evidence supports.

The main contribution is not “another IDS model.” The contribution is a stricter evaluation surface for deciding when an IDS result is meaningful.
