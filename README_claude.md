# First Assignment: Business Process Prediction, Simulation, and Optimization (WS 2026/2027)

**Author:** Milena Fernanda Rivera Hernández

**Practical Course - Business Process Prediction, Simulation, and Optimization (IN0012, IN40045)**
**Technical University of Munich**

This repository contains my code for the first assignment.

It analyzes the BPI Challenge 2017 (BPIC-17) event log with process mining: simple event log analysis, process discovery with three algorithms (Alpha Miner, Heuristics Miner, Inductive Miner) including hyperparameter tuning, evaluation with the four quality dimensions (fitness, precision, generalization, simplicity), construction of a final BPMN 2.0 model, and an advanced data science analysis (Option 2: clustering of cases). The accompanying report is submitted separately.

## Three notebooks, two environments

The work is split into three notebooks. One runs locally and two run in Google Colab:

| Notebook | Where it runs | What it covers |
|----------|---------------|----------------|
| `notebooks/assignmentI.ipynb` | **Locally** (VS Code + `.venv`) | Sections 3.1 to 3.3: setup, simple event log analysis, process discovery, quality metrics and final model |
| `notebooks/Advanced_Data_Science_AnalysisI.ipynb` | **Google Colab only** (GPU runtime) | Section 3.4 (Option 2): case-level feature engineering and K-means clustering with GPU acceleration (cuML) |
| `notebooks/Visualization_petri_nets.ipynb` | **Google Colab** (or locally, if Graphviz works on your machine) | Supplementary: draws the Petri nets and the final BPMN model as PDF figures from the exported model files |

The clustering workflow is the most computationally demanding part of the assignment: it builds a feature matrix for all 31,509 cases and trains K-means repeatedly for every candidate number of clusters and feature set, including extra runs to measure stability. That is why it runs in Colab, where a GPU is available. Keeping it in a separate notebook also leaves the process-mining pipeline of the main notebook untouched.

> The clustering notebook is **not** meant to run in the local `.venv`: it depends on RAPIDS (cuML) and a CUDA GPU, which are not part of `requirements.txt`.

The visualization notebook exists because the PM4Py drawing functions need the Graphviz system program, which gave me problems on my local machine. Colab has Graphviz installed, so I export the models locally as `.pnml` / `.ptml` files and draw them there.

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
│   ├── Advanced_Data_Science_AnalysisI.ipynb   # section 3.4, clustering (Colab, GPU)
│   └── Visualization_petri_nets.ipynb          # Petri net and BPMN figures (Colab)
└── generated_files/                # final outputs, one subfolder per notebook
    ├── generated_files_I/          # created by assignmentI.ipynb
    ├── generated_files_DS/         # created by Advanced_Data_Science_AnalysisI.ipynb
    └── generated_files_PN/         # created by Visualization_petri_nets.ipynb
```

When a notebook is executed, it writes its outputs directly into `generated_files/`. The repository keeps the final outputs of each notebook sorted into the subfolders above.

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

### 1. Prerequisites

- **uv**. On macOS: `brew install uv`. Other systems: see the [uv installation guide](https://docs.astral.sh/uv/getting-started/installation/).
- **Python 3.12**. uv can install it for you if it is missing.
- **Graphviz (system program)**, needed by PM4Py to draw Petri nets and process trees. The Python package alone is not enough. If you cannot install it, use `Visualization_petri_nets.ipynb` in Colab to draw the models.

  - macOS: `brew install graphviz`
  - Debian/Ubuntu: `sudo apt install graphviz`
  - Windows: install from the [Graphviz website](https://graphviz.org/download/) and add it to the `PATH`.

### 2. Create the virtual environment

From the project root:

```bash
uv venv --python 3.12
uv pip install -r requirements.lock.txt
```

This creates `.venv/` and installs the exact versions I used. If the lock file fails on your system (it was generated on macOS and contains macOS-only packages such as `appnope`), install the direct dependencies instead:

```bash
uv pip install -r requirements.txt
```

### 3. Run the notebook

**VS Code (recommended):**

I used VS Code, so I recommend it, however you may use the IDE of your preference.

1. Open the project folder and then `notebooks/assignmentI.ipynb`.
2. Click **Select Kernel** (top right), choose **Python Environments**, and select the `.venv` interpreter.
3. Run all cells in order (**Run All**).

**Terminal (optional):** activate the environment with `source .venv/bin/activate` (Windows: `.venv\Scripts\activate`) and open the notebook with your preferred Jupyter-compatible editor.

> IMPORTANT NOTE: The notebook reads the dataset from `../data_BPIChallenge2017_1_all/` and writes its outputs to `../generated_files/`, so its working directory must be the `notebooks/` folder (the default in VS Code). Do not move the notebook, the dataset folder or `generated_files/`, otherwise the paths will not resolve.

All analyses and quality metrics are computed in Python with open-source tools; no commercial software is used.

## Running the clustering analysis in Google Colab (`Advanced_Data_Science_AnalysisI.ipynb`)

1. **Open the notebook in Colab.** Go to [colab.research.google.com](https://colab.research.google.com), choose the **GitHub** tab and paste the repository URL, or upload `notebooks/Advanced_Data_Science_AnalysisI.ipynb` directly.
2. **Select a GPU runtime:** *Runtime → Change runtime type → T4 GPU* (or better).
3. **Upload the dataset.** Put `BPI Challenge 2017.xes.gz` (from `data_BPIChallenge2017_1_all/`) in the Colab session storage, in the same folder as the notebook (the default `/content`), since the notebook reads it as `"BPI Challenge 2017.xes.gz"`.
4. **Run all cells in order** (*Runtime → Run all*). The first cell installs `pm4py`.
5. **Download the outputs.** The notebook writes to `../generated_files`, which in Colab is `/generated_files`, one level above `/content` (go up one folder in the file browser). It is a temporary folder outside this repository: download the CSV and PDF files and copy them into `generated_files/generated_files_DS/`.

Notes for reproducibility:

- Every random seed is fixed (`random_state = 22`), and the notebook applies the same event filter (`lifecycle:transition == "complete"`) as the main notebook.
- The cases are split by case ID into 60% training, 20% validation and 20% test, so no events of one case are shared between splits. The feature vocabulary, the scaling and the model are fitted on the training cases only.
- Library versions in Colab can differ from the local lock file, so numbers can differ slightly from a local run. For the purpose of the assignment these small differences are not relevant.
- **Do not install `cuml-cu12`.** Current Colab GPU runtimes already ship with CUDA 13 and the matching RAPIDS packages (cuDF and cuML), so installing the CUDA 12 build creates a mixed environment. If you need to install RAPIDS, use the `cu13` build and restart the session before importing.

## Drawing the models in Google Colab (`Visualization_petri_nets.ipynb`)

1. Open the notebook in Colab (no GPU needed).
2. Upload these files from `generated_files/generated_files_I/` to the session storage, next to the notebook: `alpha_model.pnml`, `heuristics_model1.pnml`, `inductive_model1.pnml`, `final_model.pnml` and `final_model.ptml`.
3. Create an empty folder called `generated_files` in the session storage (the notebook saves its figures there).
4. Run all cells (the first one installs `pm4py`), then download the PDF and BPMN files from `generated_files/` and copy them into `generated_files/generated_files_PN/`.

## Troubleshooting

These were some problems I went through, so maybe this can help you!

**Local notebook**

- **`ExecutableNotFound: failed to execute 'dot'`**: Graphviz is not installed or not on the `PATH` (see prerequisites). Restart the kernel after installing it, or draw the models in Colab.
- **`FileNotFoundError` when loading the log**: the notebook is not being run from the `notebooks/` folder, or the notebook or dataset folder was moved or renamed.
- **`IProgress not found` warning**: harmless, it only affects how progress bars look. `ipywidgets` is already in `requirements.txt`; restart the kernel after installing it.
- **Loading the log takes about a minute or two**: the log has 31,509 cases and about 1.2 million events.

**Colab notebooks**

- **`ImportError: libcudf.so: cannot open shared object file`**: the environment has a broken or mixed RAPIDS installation (for example after installing the `cu12` packages on top of a CUDA 13 image). Use *Runtime → Disconnect and delete runtime*, reconnect with a GPU and run the notebook again without installing cuML.
- **pip "dependency resolver" conflict messages after an install**: they are only warnings about packages that were already installed. The real failure is the `ImportError` that comes afterwards.
- **`FileNotFoundError` for `BPI Challenge 2017.xes.gz` or for a `.pnml` / `.ptml` file**: the file was not uploaded to the Colab session, or the session was reset and the file is gone. Upload it again.
- **Error when saving a figure in the visualization notebook**: the `generated_files` folder does not exist in the session. Create it first.
- **The clustering takes a long time**: check that the runtime really has a GPU (`!nvidia-smi`). The case-level feature building loops over all cases in Python, so a part of the runtime does not depend on the GPU.

## Use of generative AI and license

The use of generative AI tools during this project is declared in [`ai-log.md`](ai-log.md).

The code is released under the GNU General Public License v3.0 (see [`LICENSE`](LICENSE)). The dataset belongs to its authors (see the note on the dataset); please cite it if you reuse it.
