# CoADAM: Reproducibility and execution notes

This document records the current repository-level requirements and limitations for reproducing the CoADAM experiments. It describes the existing codebase; it does not change the algorithms, parameter values, data splits, seeds or result files.

## 1. Software requirements

The project README specifies:

- MATLAB R2025b.
- Deep Learning Toolbox.
- Statistics and Machine Learning Toolbox.

The repository does not currently provide a tested cross-platform installation script or a complete dependency lock.

## 2. Data requirements

The preprocessing script identifies three external datasets:

- CIC-IIoT-2025.
- WUSTL-IIoT-2021.
- Edge-IIoTset.

Obtain the data from the respective dataset owners. The repository contains small configuration/threat-profile MAT files under `data/processed/`, but it does not contain all original raw datasets or all derived dataset files required to regenerate the experiments.

## 3. Local path configuration

The current MATLAB scripts contain absolute paths from the original development environment. For example, `Phase1_Preprocessing.m` specifies local input locations and an output directory under a user-specific macOS path. `Phase2A.m` and `Phase_3and4.m` also define a fixed project root.

Before running the workflow on another machine, inspect and update the path variables in a **local working copy**:

- Dataset input paths and output paths in `Phase1_Preprocessing.m`.
- The project-root and derived-directory paths in `Phase2A.m`.
- The project-root and phase-directory paths in `Phase_3and4.m` and the subsequent phase scripts.

The scripts have not been made portable as part of this documentation update. No experiment code or configuration was changed.

## 4. Main workflow

The repository's documented main sequence is:

1. `Phase1_Preprocessing.m` — preprocess source datasets and derive threat profiles.
2. `Phase2A.m` — build the environment configuration.
3. `Phase_3and4.m` — run the main DQN defender / tabular Q-learning attacker training workflow.
4. `Phase5_Baselines.m` — baseline comparison.
5. `Phase6_CrossDataset.m` — cross-dataset evaluation.
6. `Phase7_Ablation.m` — ablation analysis.
7. `Phase8_Scalability.m` — scalability analysis.
8. `Phase9_GTAnalysis.m` — game-theoretic analysis.

Run scripts from a MATLAB session whose current working directory is the repository root, after checking the local paths and required input files.

### Distinct training implementation

`coevolutionary_trainer.m` contains a separate class-based tabular Q-learning trainer. It should not be conflated with the DQN training loop in `Phase_3and4.m`. The two implementations have different structures and should be described separately when reporting which code produced a particular result.

## 5. Existing results and testbed-summary script

The `results/` directory contains MATLAB result files from previous experiments. The README's reported metrics are historical project results; they were not regenerated as part of this documentation update.

`FinalTestBed_Validation.m` loads `results/baseline_results_final.mat` and calculates a comparison using aggregate Cowrie, Dionaea and Suricata counts specified directly in the script. It does not parse raw honeypot or IDS logs, control the physical testbed, or independently verify the origin of those counts. Its reported AMR is an arithmetic result derived from the embedded counts. The underlying raw logs and measurement procedure are not supplied in the repository.

For that reason, distinguish the script's calculated testbed summary from independently reproducible physical deployment evidence.

## 6. Reproduction and reporting checklist

Before reporting a reproduced result:

- Record the MATLAB release and installed toolbox versions.
- Record the exact dataset versions and source locations.
- Record any local path adjustments.
- Preserve the experiment's original parameter settings, data splits and random seeds.
- Identify the exact script/training implementation used.
- Keep newly generated outputs separate from the existing result files.
- Report whether a result comes from simulation, a post-hoc calculation, or an independently instrumented physical experiment.
- Do not describe the testbed summary as a live deployment measurement unless the underlying raw records and measurement process are available.

## 7. Licence status

No `LICENSE` file is currently present in the repository. The repository therefore does not state a licence or grant explicit reuse and redistribution permissions. Contact the author for permission before reusing, modifying or redistributing the code.
