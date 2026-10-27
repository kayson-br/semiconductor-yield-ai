PROJECT TITLE:
SECOM Semiconductor Manufacturing Data Analysis

OVERVIEW:
Analyze sensor data from the public SECOM data set (1500+ wafers w/ 590 sensor data points) and use ML agent to investigate patterns associated with manufacturing failures and predict pass/fail probability of manufactured wafers.

OBJECTIVES:
Use ML to interpret and analyze sensor data in order to predict wafer manufacturing pass/fail probability.

TOOLS:
Python - Language
Pandas - Python Library/ Data extrapolation 
NumPy - Python Library/ Math
Matplotlib - Python Library/ Data visualization
Scikit-learn - Python Library/ ML



DESIGN PHASES:

Phase 1 -- Data
Goal: Understand semiconductor data
Dataset -> Load -> Explore -> Clean -> Visualize

Phase 2 -- First ML model
Goal: super simple model
Sensor measurements -> Model -> Pass / Fail

Phase 3 -- Improve model
Goal: update model
Prioritize specific sensors and patterns

Phase 4 -- Anomaly detection

Phase 5 -- AI agent interface

DESIGN PHASES (AI explanation):

Phase 1: Data          →  clean dataset + what you learned about it
Phase 2: First model   →  a baseline score to beat
Phase 3: Better model  →  a stronger model + the sensors that matter most
Phase 4: Anomalies     →  flags unusual wafers, even without labels
Phase 5: AI agent      →  a plain-English interface on top of all of it

Phase 1: Data (done)
Question: what does this data look like, and what's wrong with it?
You can't build a good model on data you don't understand. This phase produces a cleaned dataset and a list of things to watch out for.

Phase 2: First ML model
Question: can sensor readings predict pass/fail at all?
The plan is a deliberately simple model, most likely logistic regression. Its score becomes the baseline, the number every later model has to beat. Without a baseline you can't tell whether a fancier model is actually better.
It relies on Phase 1 in two ways:
* It starts from data/secom_clean.csv.
* It follows the Phase 1 findings: fill in the missing values inside the model pipeline, scale the sensors, and measure fail recall instead of accuracy

Phase 3: Improve the model
Question: which sensors really matter, and can a better model catch more failures?
This is where you trim the redundant sensors that Phase 1 found (272 nearly duplicate pairs), choose the most useful ones, try stronger models such as random forests or gradient boosting, and deal with the class imbalance. Every result gets compared with the Phase 2 baseline.

Phase 4: Anomaly detection
Question: which wafers look unusual, even if we don't know whether they failed?
Phases 2 and 3 learn from labeled examples ("this wafer failed"). Anomaly detection instead learns what normal looks like and flags anything that doesn't fit. That matters because:
In a real factory you often don't know a wafer failed until much later.
Failures are rare (6.6% here), so there are few examples to learn from.
Phase 1 found something useful here: the fail rate dropped sharply over time, which suggests the process changed. Spotting that kind of drift is what anomaly detection is for.

Phase 5: AI agent interface
Question: can someone ask about the results in plain English?
Your README only names this phase, so here's how I'd read it: an AI assistant that can use your models as tools. An engineer could ask "why was wafer 812 flagged?" or "which sensors predict failure best?", and the agent would run the Phase 3 model or the Phase 4 detector and explain the answer. It's last because it needs working models to call.
