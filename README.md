# First Assignment: Business Process Prediction, Simulation, and Optimization (WS 2026/2027)

**Author:** Milena Fernanda Rivera Hernández

**Practical Course - Business Process Prediction, Simulation, and Optimization (IN0012, IN40045)**
**Technical University of Munich**

This repository contains my code for the first assignment. 

It analyzes the BPI Challenge 2017 (BPIC-17) event log with process mining: simple event log analysis, process discovery with three algorithms (Alpha Miner, Heuristics Miner, Inductive Miner) including hyperparameter tuning, evaluation with the four quality dimensions (fitness, precision, generalization, simplicity), construction of a final BPMN 2.0 model, and an advanced data science analysis. The accompanying report is written separately but can be also found in this repo :)

## Repository structure

```text
.
├── README.md
├── LICENSE
├── .gitignore                      # excludes .venv/
├── requirements.txt                # direct dependencies (what the project uses)
├── requirements.lock.txt           # exact versions of all installed packages
├── assignmentI.ipynb               # main notebook (run this one)
├── ai-log.md                       # declaration of GenAI usage
├── Assignment_I_instructions.pdf   # instructions for the assignment
├── data_BPIChallenge2017_1_all/    # dataset folder (see note below)
│   ├── BPI Challenge 2017.xes.gz   # BPIC-17 event log (XES, gzip-compressed)
│   └── DATA.xml                    # metadata describing the structure of the log
└── generated_files/                # outputs created by running the notebook
    ├── alpha_model.pnml
    ├── heuristics_model.pnml
    ├── inductive_model.pnml
    ├── grid_results.csv
    ├── final_model.bpmn
    ├── final_model.pdf
    ├── Case_duration_and_outcome_distribution.pdf
    └── Outcome_by_loan_attributes_by_application_type.pdf
```

## Notebook contents

The notebook follows the numbering of the assignment instructions:

| Section | Content |
| --- | --- |
| 3.1 Technical Setup and Preliminaries | Libraries, data loading (XES to DataFrame) |
| 3.2 Simple Event Log Analysis | Basic statistics table, case-level table, additional statistics and visualizations |
| 3.3 Process Model Creation and Validation | Alpha Miner+, Heuristics Miner, Inductive Miner; hyperparameter grid search; PM4Py quality metrics plus custom simplicity metrics; final model refinement and BPMN 2.0 export |
| 3.4 Advanced Analysis (Option 2) | Advanced data science analysis |

Files generated when the notebook is executed (written to the project root):

| File | Content |
| --- | --- |
| `alpha_model.pnml` | Petri net discovered with the Alpha Miner |
| `heuristics_model.pnml` | Petri net discovered with the Heuristics Miner |
| `inductive_model.pnml` | Petri net discovered with the Inductive Miner |
| `grid_results.csv` | Hyperparameter grid search results (one row per configuration, with all quality metrics) |
| `final_model.bpmn` | Final process model in BPMN 2.0 |
| `final_model.pdf` | Figure of the final BPMN model used in the report |
| `Case_duration_and_outcome_distribution.pdf` | Figure: case duration and outcome distribution |
| `Outcome_by_loan_attributes_by_application_type.pdf` | Figure: outcome by loan attributes and application type |

## Note on the dataset

The BPI Challenge 2017 event log is **included in this repository** (`data_BPIChallenge2017_1_all/BPI Challenge 2017.xes.gz`) because the official website hosting the dataset was unreachable at the time of the assignment. No download is needed. The notebook loads it with a relative path, so the folder name and file name must not be changed.

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
uv add pm4py==2.7.23.11 pandas==3.0.6 numpy==2.5.4 matplotlib==3.11.2 ipykernel==7.4.0 r4pm==0.6.2
```
All analyses and quality metrics are computed in Python with the open-source tools above; no commercial software is used.

## Troubleshooting

This were some problems I went through so maybe this can help you!

- **`ExecutableNotFound: failed to execute 'dot'`**: Graphviz is not installed or not on the `PATH` (see prerequisites). Restart the kernel after installing it.
- **`FileNotFoundError` when loading the log**: the notebook is not being run from the project root, or the dataset folder or file was renamed.
- **`IProgress not found` warning**: harmless, it only affects how progress bars look. Optionally run `uv add ipywidgets` and restart the kernel.
- **Loading the log takes about a minute or two**: Note that the log has 31,509 cases.