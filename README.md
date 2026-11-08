# First Assignment: Business Process Prediction, Simulation, and Optimization (WS 2026/2027)

**Author:** Milena Fernanda Rivera Hernández

**Practical Course - Business Process Prediction, Simulation, and Optimization (IN0012, IN40045)**
**Technical University of Munich**

This repository contains my code for the first assignment. 

It analyzes the BPI Challenge 2017 (BPIC-17) event log with process mining: simple event log analysis, process discovery with three algorithms (Alpha Miner, Heuristics Miner, Inductive Miner) including hyperparameter tuning, evaluation with the four quality dimensions (fitness, precision, generalization, simplicity), construction of a final BPMN 2.0 model, and an advanced data science analysis (clustering). The accompanying report is written separately but can be also found in this repo :)

## Environments

The work is split into three notebooks. One runs locally and two run in Google Colab:

| Notebook | Where it runs | What it covers |
|----------|---------------|----------------|
| `notebooks/assignmentI.ipynb` | **Locally** (VS Code + `.venv`) | Sections 3.1 to 3.3: setup, simple event log analysis, process discovery, quality metrics and final model |
| `notebooks/Advanced_Data_Science_AnalysisI.ipynb` | **Google Colab only** | Section 3.4 (Option 2): case-level feature engineering and K-means clustering with cuML |
| `notebooks/Visualization_petri_nets.ipynb` | **Google Colab** (or locally, if Graphviz works on your machine) | Supplementary: draws the Petri nets and the final BPMN model as PDF figures from the exported model files |

> SUPER IMPORTANT: The clustering notebook is **not** meant to run in the local `.venv`: it depends on RAPIDS (cuML) and a CUDA GPU, which are not part of `requirements.txt`.

The visualization notebook exists because the PM4Py drawing functions need the Graphviz system program, which gave me problems on my local machine. Colab has Graphviz installed, so I exported the models locally as `.pnml` / `.ptml` files and draw them there.

## Repository structure

```text
.
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt                # direct dependencies of the local notebook
├── requirements.lock.txt           # exact versions of all installed packages (local)
├── ai-log.md                       # declaration of GenAI usage
├── Assignment_I_instructions.pdf   # instructions for the assignment
├── data_BPIChallenge2017_1_all/    # dataset folder (see note below)
│   ├── BPI Challenge 2017.xes.gz   # BPIC-17 event log (XES, gzip-compressed)
│   └── DATA.xml                    # metadata describing the structure of the log
├── notebooks/
│   ├── assignmentI.ipynb                       # sections 3.1-3.3 (local)
│   ├── Advanced_Data_Science_AnalysisI.ipynb   # section 3.4, clustering (Colab)
│   └── Visualization_petri_nets.ipynb          # Petri net and BPMN figures (Colab)
└── generated_files/                # final outputs, one subfolder per notebook
    ├── generated_files_I/          # created by assignmentI.ipynb
    ├── generated_files_DS/         # created by Advanced_Data_Science_AnalysisI.ipynb
    ├── generated_files_PN/         # created by Visualization_petri_nets.ipynb
    └── generated_files_past_run/   # stores past versions of some generated files in past executions
```

**NOTE:** When a notebook is executed, it writes its outputs directly into `generated_files/`. I chose to keep the final outputs of each notebook sorted into the subfolders above so a "frozen" version of them could be stored and never modified.

## Notebook contents

The notebooks follow the numbering of the assignment instructions:

| Section | Notebook | Content |
|---------|----------|---------|
| 3.1 Technical Setup and Preliminaries | `assignmentI.ipynb` | Libraries, data loading (XES to DataFrame) |
| 3.2 Simple Event Log Analysis | `assignmentI.ipynb` | Basic statistics table, case-level table, additional statistics and visualizations |
| 3.3 Process Model Creation and Validation | `assignmentI.ipynb` | Alpha Miner, Heuristics Miner, Inductive Miner; hyperparameter grid search; PM4Py quality metrics plus custom simplicity metrics; replay diagnostics; final model refinement |
| 3.3 (figures) | `Visualization_petri_nets.ipynb` | Petri net figures of the three miners and of the final model, final BPMN 2.0 model |
| 3.4 Advanced Analysis (Option 2) | `Advanced_Data_Science_AnalysisI.ipynb` | Hypothesis, case-level feature engineering (`create_case_features`), K-means grid search (feature set x number of clusters), stability, cluster interpretation for simulation |

### Files in `generated_files/generated_files_I/` (from `assignmentI.ipynb`)

| File | Content |
|------|---------|
| `Case_duration_and_outcome_distribution.pdf` | Figure: case duration and outcome distribution |
| `Outcome_by_loan_attributes_by_application_type.pdf` | Figure: outcome by loan attributes and application type |
| `alpha_model.pnml` | Petri net discovered with the Alpha Miner |
| `heuristics_grid1.csv` | Hyperparameter grid search results of the Heuristics Miner |
| `heuristics_model1.pnml` | Selected Petri net of the Heuristics Miner |
| `inductive_grid1.csv` | Hyperparameter grid search results of the Inductive Miner |
| `inductive_model1.pnml` | Selected Petri net of the Inductive Miner |
| `baseline_metrics1.csv` | Quality metrics of the three baseline models |
| `Cumulative_coverage_of_trace_variants1.pdf` | Figure: cumulative coverage of trace variants |
| `iterations2.csv` | Refinement iterations of the final model (preprocessing, variant coverage and noise threshold, with all quality metrics) |
| `Tradeoff_replay_precision.pdf` | Figure: trade-off between replayed cases and precision |
| `cross_validation.csv` | 5-fold cross-validation (split by case) of the chosen configuration: out-of-sample metrics per fold |
| `final_model_metrics.csv` | Quality metrics of the final model |
| `final_model.pnml` | Final model as Petri net |
| `final_model.ptml` | Final model as process tree (used to create the BPMN model) |

### Files in `generated_files/generated_files_DS/` (from `Advanced_Data_Science_AnalysisI.ipynb`)

| File | Content |
|------|---------|
| `clustering_grid_results.csv` | Grid search results (one row per feature set and number of clusters, with validation metrics, stability and smallest cluster share) |
| `clustering_grid_search.pdf` | Figure: validation metrics, stability and inertia against the number of clusters |
| `cluster_distribution.pdf` | Figure: distribution of test cases across the selected clusters |
| `cluster_simulation_summary.csv` | Per-cluster summary (share of cases, median events, duration, resources, handovers) for simulation parameters |
| `cluster_differences.pdf` | Figure: duration, events, resources and handovers per cluster |
| `cluster_pca.pdf` | Figure: 2D PCA projection of the clusters |

### Files in `generated_files/generated_files_PN/` (from `Visualization_petri_nets.ipynb`)

| File | Content |
|------|---------|
| `alpha_model1.pdf` | Figure: Petri net of the Alpha Miner |
| `heuristics_model1.pdf` | Figure: Petri net of the Heuristics Miner |
| `inductive_model1.pdf` | Figure: Petri net of the Inductive Miner |
| `final_model_petri1.pdf` | Figure: Petri net of the final model |
| `final_model1.bpmn` | Final process model in BPMN 2.0 |
| `final_model_bpmn1.pdf` | Figure of the final BPMN model used in the report |

## Note on the dataset

The BPI Challenge 2017 event log (B.F. van Dongen, Eindhoven University of Technology, 2017, doi:10.4121/uuid:5f3067df-f10b-45da-b98b-86ae4c7a310b) is **included in this repository** (`data_BPIChallenge2017_1_all/BPI Challenge 2017.xes.gz`) because the official website hosting the dataset was unreachable at the time of the assignment. No download is needed. Its metadata (`DATA.xml`) marks the rights as public.

The local notebook loads the log with a relative path, so the folder name and file name must not be changed.

## Running the main notebook locally (`assignmentI.ipynb`)

The project uses [uv](https://docs.astral.sh/uv/) to manage the virtual environment. The repository contains `requirements.txt` and `requirements.lock.txt` (there is no `pyproject.toml` or `uv.lock`).




## Setting up the environment

The project uses [uv](https://docs.astral.sh/uv/) to manage the virtual environment and dependencies.

### 1. Prerequisites

- **uv**. On macOS: `brew install uv`. Other systems: see the [uv installation guide](https://docs.astral.sh/uv/getting-started/installation/).
- **Python 3.12**. uv can install it for you if it is missing.
- **Graphviz (system program)**, required by PM4Py to draw Petri nets and process trees. The Python package alone is not enough.
  - macOS: `brew install graphviz`
  - Debian/Ubuntu: `sudo apt install graphviz`
  - Windows: install from the [Graphviz website](https://graphviz.org/download/) and add it to the `PATH`.

### 2. Create the virtual environment

From the project root:

```bash
uv sync
```

This creates `.venv/` and installs the exact versions recorded in `uv.lock`.

### 3. Run the notebook

**VS Code (recommended):**

I used VS code, so I recommend it, however you may use the IDE of your preference.

1. Open the project folder and then `assignmentI.ipynb`.
2. Click **Select Kernel** (top right), choose **Python Environments**, and select the `.venv` interpreter.
3. Run all cells in order (**Run All**).

**Terminal (optional):** activate the environment with `source .venv/bin/activate` (Windows: `.venv\Scripts\activate`) and open the notebook with your preferred Jupyter-compatible editor.

> IMPORTANT NOTE: Run the notebook with the project root as the working directory. Otherwise the relative path to the dataset will not resolve.

### Setting up from scratch without the lock file

If `pyproject.toml` and `uv.lock` are not available:

```bash
uv init --bare
uv python pin 3.12
uv add pm4py==2.7.23.11 pandas==3.0.6 numpy==2.5.4 matplotlib==3.11.2 ipykernel==7.4.0
```
All analyses and quality metrics are computed in Python with the open-source tools above; no commercial software is used.

## Troubleshooting

This were some problems I went through so maybe this can help you!

- **`ExecutableNotFound: failed to execute 'dot'`**: Graphviz is not installed or not on the `PATH` (see prerequisites). Restart the kernel after installing it.
- **`FileNotFoundError` when loading the log**: the notebook is not being run from the project root, or the dataset folder or file was renamed.
- **`IProgress not found` warning**: harmless, it only affects how progress bars look. Optionally run `uv add ipywidgets` and restart the kernel.
- **Loading the log takes about a minute or two**: Note that the log has 31,509 cases.