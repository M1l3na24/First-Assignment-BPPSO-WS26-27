# AI Log for my First Assignment
------------------


## Tools
- Gemini
- Claude

---------------------
## Usage philosophy


-----------------
## Usage entries

Gemini - 26/10/26

hey gemini, I am writing an introduction for my project, and I need to Present an overview in the report - state of the art and limitations about this 4 papers, I share with you what I have and I would really appreciate if you could help me keep my tone (first person narrator) and ideas but just check correctness of them and the concepts used, and if you want to add more information about tools used in the additional paper feel free. give me the polished version as a .txt document

output: \section{Introduction}

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