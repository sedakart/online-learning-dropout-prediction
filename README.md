# Online Learning Dropout Prediction

MSc research project predicting student dropout in an online course, using survival analysis to find when students leave, classification to find who is at risk, and clustering to group students by behaviour. Bayes Business School, City St George's, University of London, 2026.

## Research problem

Most students who enrol on an online course do not finish it. The project works from a single idea: one dropout label can hide two different kinds of exit. An academic dropout disengages during the course and leaves early. An administrative dropout finishes all the coursework and tests but never takes the final step of certifying. The two look opposite in the data and need different responses, so the project asks three questions: when students leave, which behaviours signal it, and whether students group into types that drop out differently.

## Data

A behavioural dataset of 18,724 students, provided by an industry partner. The columns cover session viewing, test and coursework grades, forum activity, office-hour visits, and a dropout flag. The raw data is confidential and is not included in this repository.

The missing values are structured, not random. Once a student's record stops it never resumes, so the pattern of what is missing records the exit itself. That rules out filling the gaps and instead sets up the cohort design below.

## Approach

Three notebooks, meant to be read in order.

**`01_data_preparation.ipynb`**: Builds four cohorts, one at each checkpoint after a test. Each cohort keeps only the students still present at that point, so no cohort has a missing value. This turns one prediction problem into four, each asking whether a student present at a checkpoint survives to the next.

**`02_classification.ipynb`**: Fits five classifiers to each cohort (logistic regression, decision tree, random forest, gradient boosting, SVM), handles class imbalance, tunes by cross-validated F1 on the dropout class, and compares each cohort with and without engagement features.

**`03_clustering.ipynb`**: Places every student on two axes, an engagement score and their first test grade, then groups them two ways, a median split into four quadrants and a k-means solution. The groups are fixed at the start and tracked across the cohorts, so any shift shows who left rather than who changed.

## Key findings

Dropout is not one thing. Academic and administrative dropouts show up differently across all three methods, and they call for different responses.

Risk is highest at the end. The dropout rate rises across the course and peaks at the certification stage. Across the whole course, about 40% of students drop out.

Behaviour predicts academic dropout well but not administrative dropout. The models separate early dropouts from stayers almost perfectly in the first three cohorts, but do much worse at the certification stage, where the two groups both completed the course and differ only in a step the behaviour does not capture.

Engagement matters more than early grades. Among the four student types, the two most engaged groups completed at the highest rates, while students who scored well early but engaged little looked safe and left late. A warning system built on grades alone would miss them.

## Repository structure

```
01_data_preparation.ipynb   Cohort construction and feature engineering
02_classification.ipynb     Per-cohort dropout classification
03_clustering.ipynb         Student-type clustering and how the mix shifts
requirements.txt            Python dependencies
```

## Running it

```
pip install -r requirements.txt
```

The notebooks expect the raw dataset locally, which is not distributed, and run in numbered order.
