# AI Log for my First Assignment
------------------


## Tools
- Gemini
- Claude
- ChatGPT

---------------------
## Usage philosophy


-----------------
## Usage entries

- **Gemini - 26/10/26**

**Promt:** "hey gemini, I am writing an introduction for my project, and I need to Present an overview in the report - state of the art and limitations about this 4 papers, I share with you what I have and I would really appreciate if you could help me keep my tone (first person narrator) and ideas but just check correctness of them and the concepts used, and if you want to add more information about tools used in the additional paper feel free. give me the polished version as a .txt document"

**Output:** \section{Introduction}

\subsection{Challenge and Data Context}
The Business Process Intelligence Challenge (BPIC) 2017 provided an event log from a Dutch Financial Institution containing data from 2016 to early 2017 \cite{dadashnia2017exploring, rodrigues2017stairway}. The log consists of 1,202,267 events belonging to 31,509 loan applications, for which 42,995 offers were created \cite{rodrigues2017stairway}. The process data is highly complex because it merges three distinct event types: Application state changes, Offer state changes, and Workflow events \cite{povalyaeva2017bpic}. The challenge asked participants to analyze throughput times, the influence of incomplete files on final outcomes, and the impact of single versus multiple offers on conversion rates \cite{blevi2017process}. The competition was divided into three categories: Students, Academics, and Professionals. Notably, the company had switched systems since a prior 2012 log and now supports multiple offers per application \cite{dadashnia2017exploring}.

\subsection{State of the Art}
To compare the approaches used during the challenge, I analyzed four papers: the three category winners and an additional paper of personal interest.

\subsubsection*{Students - BPIC 2017: Density Analysis of the Interaction With Clients}
Going into this challenge, my team and I shared a similar background with the student winners: zero previous experience with process mining \cite{povalyaeva2017bpic}. They grounded their work in the Process Mining Manifesto \cite{ieee2011process}, specifically relying on Guiding Principle 5 and Guiding Principle 6. Using tools like Celonis, Disco, ProM, and Python-based machine learning, they delivered outstanding results \cite{povalyaeva2017bpic}. Their key recommendations included implementing retention mechanisms for unresponsive clients and improving the initial data-checking process. They also identified 0-second duration events indicating logging issues, used a Concept Drift plug-in to confirm process stability, and defined three multi-offer scenarios: Create-Create, Create-Call-Create, and Incomplete-Create. However, I noted that some of their optimization recommendations prioritize broad process mechanics while slightly overlooking the bank's core profitability motives, such as the direct financial impact of minimizing cancellations.

\subsubsection*{Professionals - Process mining on the loan application process of a Dutch Financial Institute}
The professional winners framed their analysis around the "Time \& Effort" pillar of the Six Pillars customer experience methodology \cite{kpmg2016us}, focusing on minimizing customer effort and using time as a competitive advantage \cite{blevi2017process}. They found that customers often do not understand required documentation, making clear communication and delay management critical. To visualize the dataset, they built a SQL database connected to Power BI and combined this with Disco and ProM. For predictive analytics, they utilized R and Azure Machine Learning Studio to run Logistic Regression, Random Forest, and Neural Network models. While their architectural setup is highly accessible for non-technical business profiles, I observed potential data leakage in their predictive models. They used predictors like Duration\_days and FrequencyOfIncompleteness, which are only fully known after the case concludes or the customer returns documents, potentially allowing the model to learn "long duration means no sale" without offering actionable customer insights. Additionally, they did not report fitness or precision metrics for their fuzzy-miner maps.

\subsubsection*{Academics - Stairway to value: mining a loan application process}
The academic winners distinguished themselves through exhaustive conformance checking \cite{rodrigues2017stairway}. They built 19 Petri nets using Yasper and evaluated them in ProM, ultimately achieving a final AS-IS model with 97.28\% replay fitness and 98.72\% Align-ETConformance precision. They divided the process into three milestones: Receiving applications, Negotiating offers, and Validating documents. They recommended closer initial customer contact, found that increasing the number of offers improved success rates, and noted that validation calls merely shifted the proportions between Denied and Cancelled applications. Like other participants, they suggested the bank implement a clear checklist for required documentation.

\subsubsection*{Additional - Exploring the Potentials of Artificial Intelligence Techniques for Business Process Analysis}
This paper stood out by specifically testing the limits of AI in business process analysis and directly comparing the 2017 dataset to the 2012 system \cite{dadashnia2017exploring}. Alongside standard software like Disco and ProM 5.2, they utilized Aris for modeling, Python and R for data manipulation, Weka for machine learning, and Tensorflow for Deep Learning. They used the RefMod-Miner prototype to cluster process data into smaller, structured subprocess fragments. Furthermore, they implemented a Convolutional Neural Network (CNN) to predict next process steps, comparing its efficiency against LSTM networks. Using Chi-square statistical tests, they concluded that incompleteness is strongly associated with a Pending outcome, and multiple offers are similarly associated with Pending.

\subsection{Synthesis}
Ultimately, each group covered blind spots left by the others. The students addressed concept drift and conformance thresholds \cite{povalyaeva2017bpic}, while professionals focused on lifecycle behaviors, loan-goal segmentation, and a strong customer experience framework \cite{blevi2017process}. The academics provided a rigorous resource analysis that both other groups skipped \cite{rodrigues2017stairway}. Finally, all four papers reached a unified consensus on the bank's core hypothesis: asking for more completion requests does not lead to higher rejection rates \cite{dadashnia2017exploring, povalyaeva2017bpic, blevi2017process, rodrigues2017stairway}.

- **Claude - 26/10/26**

**Promt:** "For the left one is it possible to use seaborn to level up the graph? or is it better to keep it using matplotlib?"(for the second additional statistic)

**Output:** Seaborn can draw this chart, but let me test whether it improves on yours before I recommend either.

Keep matplotlib for that panel. Seaborn is built on top of matplotlib, so it wouldn’t make the chart any better.

I prototyped it with seaborn on your data to check. sns.histplot(..., y="goal", hue="outcome", multiple="fill") can draw 100 % stacked horizontal bars straight from the case table, and it looked almost the same as yours. It cost more than it gained:

Seaborn stacks the outcomes in the opposite order, so accepted ends up on the right and the dotted “overall accepted” line no longer lines up with the accepted boundary. Fixing that needs extra code.
The row order, the n= in the labels, the percentage labels, the black gaps between segments and the legend position still need matplotlib code.
The legend came out with the title outcome_cat and covered part of the bars.

Where seaborn does help is the look of the whole figure. Two lines, applied to both panels, remove the top and right frame lines and tidy the ticks:

python
import seaborn as sns
sns.despine(ax=ax1, left=True); sns.despine(ax=ax2, left=True)

It also has sns.set_context("talk") if you want larger text for a presentation.

If the aim is to improve how the left panel reads, these changes would help more than a library switch. I haven’t made any of them yet:

Baseline row: add an “All decided cases” bar at the top, with a thin separator under it, so each goal is compared against the overall mix of 54.8 % accepted, 33.1 % cancelled and 12.1 % denied.
Small segments: label the denied segments below 8 %, which are blank now, in a smaller font. Several of them, such as 5.5 % for “Boat” and “Remaining debt home”, are interesting.
Goal families: group related goals with a thin separator and a bold row label, for example vehicles (Car, Motorcycle, Boat, Caravan / Camper) and debt-related goals. That only helps if you want to tell a story about goal types.
Highlights: colour one or two goals’ tick labels, for example “Not speficied” with the lowest acceptance and “Remaining debt home” with the highest.

I’d start with 1 and 2, which are quick and don’t change your design. Tell me which ones you want and I’ll update the cell.


- **Claude - 26/10/26**

**Promt:** (Attached my notebook) I have this, help me answer in code lines this questions: How does a case typically look like? How do long cases look like? How do short ones look like?  pretty sure I only need to show certain logs from the data frame.

**Output:** (Case example python code)I wrote case_examples.py for your notebook. Paste each # %% block into its own cell, after the cell that builds cases. I ran it on your log, and it uses your variable names (df, case, act, timestamp, resource, lifec and the cases table).

You’re right that you only need to show a few cases, but I’d put numbers next to them so the examples aren’t anecdotal. The code has three parts:

Groups: short means the bottom 10 % of cases by number of events (≤ 21), long means the top 10 % (≥ 60), and the rest are typical.
Profile table: one row per group with mean events, median duration, mean offers, outcome mix and the activities that are most frequent per case.
Examples: one representative case per group, shown as a one-line summary and as an event table. I picked each as the case closest to its group’s median length and duration, among cases with the group’s most common outcome. A last optional cell shows the extremes: most and fewest events, longest and shortest duration.

What the log shows

Short	Typical	Long
Cases	3,544	24,695	3,270
Mean events	19.2	36.1	73.8
Median duration	30.8 days	16.7 days	25.7 days
Accepted / cancelled	0.2 % / 97.0 %	58.5 % / 27.3 %	84.5 % / 7.4 %
A typical case goes: create application, make an offer and send it, call the customer after the offer, validate the application, maybe request missing documents, and the customer accepts (A_Pending). That takes about two weeks and 35 or so events.
Short cases are applications where the offer is sent and the customer never responds. There is no validation and no returned offer, and the bank cancels after about 31 days. This matches the duration pattern we saw earlier.
Long cases are applications that loop through “validate application” and “call incomplete files”, about 19 and 21 times per case against 6 and 4 for typical ones. They get more offers (1.9 on average) and still end up accepted most of the time.

Things to mention in the report

Length vs duration: case length and case duration are almost unrelated (rank correlation of −0.04). Short cases by events are actually the slowest, because they wait for a time-out. That is why I defined short and long by events and show the duration extremes separately.
Lifecycle events: the lengths count every lifecycle event (schedule, start, suspend, resume, complete), so a case’s length depends on how many times its work items were suspended. If section 3.3 works on the complete-only log, lengths will be shorter. State which log you use here.
Display: the example tables show timestamps with the time since the first event. For long cases I limited the table to 25 rows, with the full sequence in the one-line summary.

- **Gemini - 27/10/26**

**Promt:** I want to visualise this as a stair. return " -> \n".join(f"{a} (X{n})" if n > 1 else a for a, n in runs)

**Output:** def compact_trace(case_id):
    """Activity sequence as a visual staircase; consecutive repeats are collapsed."""
    seq = df.loc[df[case] == case_id, act].tolist()
    runs = [(a, sum(1 for _ in g)) for a, g in itertools.groupby(seq)]
    
    # Use enumerate to multiply the indent spaces (e.g., 4 spaces per step)
    return "\n".join(f"{'    ' * i}↳ {a} (x{n})" if n > 1 else f"{'    ' * i}↳ {a}" 
                     for i, (a, n) in enumerate(runs))

- **Claude - 30/10/26**

**Prompt:** Asked for help writing the `README.md` of the repository: instructions for setting up the virtual environment (later adapted to `uv`, a `requirements.txt` plus a lock file, and VS Code), a note that the BPIC-17 dataset is included because the official website was unreachable, and the repository structure with the dataset folder and its files. After uploading the assignment instructions, asked to update the README to match them, and later to add the new `generated_files/` folder, `LICENSE`, `ai-log.md`, and the reproducibility instructions for VS Code.

**Output:** A draft `README.md` with the project description, the repository tree, the dataset note, step-by-step environment setup with `uv` and VS Code (including a check that the notebook uses the project's `.venv`), a table of tools and libraries, a reproducibility section, and a troubleshooting section, plus an updated directory tree. The author adapted the text to the final project structure.

- **Claude - 31/10/26**

**Prompt:** Uploaded my notebook and the task text for the final process model (replay about 80% of the cases, precision, generalization, simplicity, BPMN 2.0, metric trade-offs for a simulation base model) and asked for help. Later uploaded the results table and the metrics of the model that did not reach 80%, and asked whether to keep changing the grids.

**Output:** Diagnosed that my models had high log fitness but almost no fitting traces, and that my F-score selection criterion favored models that missed the target. Proposed filtering to the most frequent variants, discovering with the Inductive Miner, and picking by a rule: first reach about 80% fitting traces, then maximize precision, then simplicity. Argued the metric priority from the simulation purpose (precision first, generalization last). Delivered final_process_model.py (iteration grid, selection, alignment-based metrics, PNML and BPMN export), tested only on a synthetic log. After my results, explained that noise thresholds of 0.45–0.55 were discarding behavior and recommended rerunning with noise between 0.0 and 0.1.

- **Claude - 31/10/26**

**Prompt:** Asked for help with Task 3.3 (process discovery and quality metrics) on BPIC-17: reviewing my notebook, using Alpha Miner, Heuristics Miner and Inductive Miner, designing a grid search with justified hyperparameter values, computing alignment-based and token-based metrics, and implementing additional simplicity metrics based on a literature review paper on process model complexity. Also asked about lines of code I did not understand (stable sorting, accessing the first trace and event of the dataframe, the meaning of TBR) and about an "easy sound" exception raised by PM4Py.

**Output:** A review of the notebook that identified errors (Alpha Miner used instead of Alpha Miner+, undefined variable names, a silently failing simplicity call), a grid-search design with the reasoning for each parameter range, and code cells for alignment-based and token-based evaluation of each algorithm. Claude explained that alignments cannot be applied to a model that is not easy sound, and proposed the CFC and CNC metrics (from the reviewed paper) together with documented implementations of the custom metrics (NOA, AGD, CFC, CNC). The code was tested by Claude only on synthetic logs; results on BPIC-17 were obtained and interpreted by me.

- **Claude - 31/10/26**

**Prompt:** Asked whether the event log should be split into training and test sets before discovery and evaluation (after another tool suggested it), and for help writing an email to my professor about it. Then asked how to construct the final process model (hand-drawn Petri net or another method), how to reach about 80% replayed cases, how to experiment with iterations, and how to fix an error when running my iteration function.

**Output:** A discussion of when a train/test split is useful and when it is not required by the assignment, plus a draft email in English to the professor. A proposed workflow to build the final model iteratively using process trees (editing the model as text, measuring each version with token-based replay, recording every change in a design log) and exporting to BPMN 2.0 and PDF. Claude also explained and fixed the error caused by passing a Petri net where a process tree was expected, and suggested example iteration cells (variant-coverage filtering, noise threshold, manual edits). The suggestions were tested by Claude only on a synthetic log.

- **Gemini - 31/10/26**

**Prompt:** Asked for help polishing a report note on metric constraints and generating Python code to build a comparative metrics table (combining PM4Py metrics with custom NOA and AGD simplicity metrics) for Alpha, Inductive, and Heuristics miners. Requested clarification on log splitting, process tree behavior, and custom docstrings for filtering, discovery, and evaluation helper functions. Asked for an interpretation of the grid search results to select the optimal model. Finally, asked for a workaround to export and visualize Petri nets and BPMN models as PDFs in Google Colab, bypassing local Intel Mac Graphviz dependency issues.

**Output:** Refined the metric constraint note and provided standalone evaluation cells and Pandas DataFrame code for the final report. Clarified the chronological necessity of train/test splitting to ensure valid generalization scores. Formatted the requested docstrings. Analyzed the grid search table, recommending the 0.75 coverage configuration to avoid structural entanglement (spaghetti models). Provided a workflow to export the model locally as a `.ptml` (Process Tree) file, upload it to Colab, and use PM4Py to convert, visualize, and save the final BPMN and Petri net models as PDFs.

- **ChatGPT - 31/10/26**

**Prompt:** Asked for help continuing the implementation of an unsupervised K-means clustering analysis in Google Colab using the BPI Challenge 2017 event log in XES format. Uploaded the notebook for review and requested step-by-step guidance, preserving the previously established methodology. Implemented a grid search over K-means configurations, comparing Silhouette, Davies–Bouldin, Calinski–Harabasz, and inertia, with training, validation, and test sets to prevent data leakage. After sharing the results, asked for help evaluating the selected two-cluster model and interpreting cluster distributions, process complexity, performance, activity patterns, transitions, and resource collaboration.

**Output:** Reviewed the notebook and provided the next implementation cells for grid search, final test-set evaluation, and cluster interpretation. Identified K=2 as the strongest initial configuration based on validation metrics, with a test Silhouette score of 0.2554 and two clusters containing 69.17% and 30.83% of test cases. Provided code to compare case duration, event counts, resource involvement, handovers, most frequent activities and transitions, and visualizations. Also highlighted methodological considerations, including avoiding indiscriminate duplicate removal, interpreting inter-event times carefully, and checking the influence of different feature groups on clustering.

- **ChatGPT - 31/10/26**

**Prompt:** Uploaded updated versions of the Google Colab notebook at different implementation stages and requested continued assistance based on the actual notebook outputs, rather than restarting the analysis.

**Output:** Reviewed the successive notebook versions and guided the analysis incrementally, explaining the purpose of each implementation stage and requesting the relevant outputs before proceeding.