# CoADAM

**A Co-Evolutionary Signaling Game Framework for Equilibrium Cyber Deception in Industrial IoT**

[![Zenodo DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19556576.svg)](https://doi.org/10.5281/zenodo.19556576)

## Overview

CoADAM is a MATLAB research framework for studying adaptive cyber deception in Industrial Internet of Things (IIoT) environments. It models the interaction between an attacker and a defender as a constrained, sequential decision problem with signaling-game and game-theoretic analysis.

The main training workflow in `Phase_3and4.m` uses a Deep Q-Network (DQN) defender and a tabular Q-learning attacker. It includes replay-based DQN updates, a target network, adaptive exploration and a pool of historical attacker policies. The repository also contains a separate class-based tabular Q-learning implementation in `coevolutionary_trainer.m`; it is a distinct training implementation, not a dependency that the README assumes is automatically run by the DQN script.

CoADAM is a simulation and experimental-analysis codebase. The presence of a testbed summary script should not be interpreted as evidence that the repository itself deploys or operates a physical honeypot.

## Research components

- **IIoT environment:** attacker and defender actions, state transitions, reward signals and resource constraints.
- **DQN defender:** a neural-network policy trained using experience replay and a target network.
- **Adaptive attacker:** tabular Q-learning with an attacker-policy pool in the main DQN training workflow.
- **Alternative trainer:** a separate class-based Q-learning co-evolutionary training implementation.
- **Evaluation:** baseline comparisons, cross-dataset transfer, ablation, scalability and game-theoretic analysis.
- **Data preparation:** preprocessing and MITRE ATT&CK-aligned threat-profile construction for the selected IIoT datasets.

## Repository structure

The current repository stores most MATLAB scripts at the **repository root**; the `core/`, `phases/`, `figures/` and `testbed/` folders shown in earlier project descriptions are not the current on-disk layout.

| Path | Purpose |
|---|---|
| `Phase1_Preprocessing.m` | Dataset preprocessing and threat-profile extraction |
| `Phase2A.m` | Environment configuration builder |
| `iiot_environment.m` | IIoT simulation environment |
| `Phase_3and4.m` | Main DQN-defender / Q-learning-attacker training workflow |
| `coevolutionary_trainer.m` | Separate class-based tabular Q-learning trainer |
| `Phase5_Baselines.m` | Baseline comparison |
| `Phase6_CrossDataset.m` | Cross-dataset evaluation |
| `Phase7_Ablation.m` | Ablation analysis |
| `Phase8_Scalability.m` | Scalability analysis |
| `Phase9_GTAnalysis.m` | Game-theoretic analysis |
| `FinalTestBed_Validation.m` | Post-hoc calculation comparing stored simulation metrics with manually supplied aggregate testbed counts |
| `Fig_*.m`, `Figure_Batch*.m` | Figure-generation scripts |
| `data/processed/` | Includes the environment configuration and threat-profile files; the original source datasets and all derived dataset files are not bundled |
| `results/` | Selected MATLAB result files from existing runs |
| `.gitignore` | Excludes large derived datasets, checkpoints, figures and generated reports |

## Reported results

The project README has reported the following results from its existing experiments:

- Attack Mitigation Rate (AMR): **0.8363** against a random attacker and **0.8033** against an adaptive attacker.
- Reported ε-Nash exploitability: **ε = 2.4523** (4.43% normalized).
- Cross-domain transfer retention: **99.61%** across the three listed IIoT datasets.
- Training convergence: reported at episode **1,377** in a 5,000-episode training run.

These are reported project results, not results independently rerun during this documentation update. Refer to the corresponding result files and analysis scripts when interpreting or reproducing them.

### Important note on the testbed summary

The existing `FinalTestBed_Validation.m` script loads stored simulation results and calculates a comparison using aggregate counts entered directly in the script (including Cowrie, Dionaea and Suricata counts). It does not ingest the underlying raw honeypot/IDS logs or run a physical testbed. The script's AMR value of approximately **0.9975** is calculated from those supplied counts. The raw records and independent measurement procedure are not included in this repository, so this figure should be described as a script-derived testbed summary rather than independently verifiable physical-deployment evidence.

## Requirements

The original project specifies:

- MATLAB R2025b.
- Deep Learning Toolbox.
- Statistics and Machine Learning Toolbox.

Compatibility with other MATLAB releases and toolbox versions has not been established in this repository documentation.

## Datasets

The preprocessing workflow identifies the following public IIoT datasets:

1. [CIC-IIoT-2025](https://www.unb.ca/cic/datasets/iiot-dataset-2025.html)
2. [WUSTL-IIoT-2021](https://www.cse.wustl.edu/~jain/iiot2/index.html)
3. [Edge-IIoTset](https://ieee-dataport.org/documents/edge-iiotset-new-comprehensive-realistic-cyber-security-dataset-iot-and-iiot-applications)

The datasets are not redistributed here. Obtain them from their respective owners and check their terms of use.

## Running the workflows

Run the scripts from a MATLAB session with the repository root as the current working directory. **The current scripts contain machine-specific absolute paths.** Before running them on another computer, update the path variables in the scripts to match the local dataset locations and project directory. This documentation update does not change those scripts or their experiment settings.

The intended high-level workflow is:

1. **Preprocess the datasets**
   ```matlab
   run('Phase1_Preprocessing.m')
   ```

2. **Build the environment configuration**
   ```matlab
   run('Phase2A.m')
   ```

3. **Run the main DQN co-training workflow**
   ```matlab
   run('Phase_3and4.m')
   ```

4. **Run the evaluation workflows as required**
   ```matlab
   run('Phase5_Baselines.m')
   run('Phase6_CrossDataset.m')
   run('Phase7_Ablation.m')
   run('Phase8_Scalability.m')
   run('Phase9_GTAnalysis.m')
   ```

5. **Run the post-hoc testbed summary calculation, if needed**
   ```matlab
   run('FinalTestBed_Validation.m')
   ```

The final step requires the expected stored simulation-results file and uses aggregate testbed counts embedded in the script. It is not a command to deploy or operate the honeypot.

The separate `coevolutionary_trainer.m` class-based trainer should be inspected and invoked according to its own class interface; it is not listed as a mandatory step in the main DQN workflow.

## Reproducibility notes

- The source dataset files are not included. The small configuration and threat-profile MAT files in `data/processed/` do not replace the original datasets required for preprocessing.
- Several scripts use fixed local paths. Update those paths in a local working copy before execution; the current scripts have intentionally not been modified as part of this documentation cleanup.
- Existing `results/` files are outputs from prior runs. This README update does not regenerate or validate those outputs.
- Preserve the original experiment settings, random seeds, data splits and result files when attempting to reproduce a reported result.
- See [docs/REPRODUCIBILITY.md](docs/REPRODUCIBILITY.md) for the current execution constraints and evidence limitations.

## Citation

The repository includes a Zenodo software DOI badge at the top of this page. Cite the software release using that DOI and cite the associated research article using its final bibliographic details.

## Licence

No `LICENSE` file is currently present in this repository. Reuse, modification and redistribution permissions are therefore not specified here. Contact the author before reusing or redistributing the code; do not assume that the project is released under the MIT License.

## Contact

**Usman Wushishi**  
School of Computer Science and Technology  
Chongqing University of Posts and Telecommunications  
Email: wushi09@outlook.com
