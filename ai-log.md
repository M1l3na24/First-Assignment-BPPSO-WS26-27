# AI Log for my First Assignment
------------------


## Tools
- Gemini
- Claude

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