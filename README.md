# Rule-based Interpretations of Trace Variants

This repository contains a **Python pipeline** for analyzing event logs and deriving interpretable explanations of trace variants using clustering, decision trees, and rule extraction.

---

## Overview

This notebook implements a complete pipeline for analyzing an event log and generating human-readable explanations of trace variants.

The workflow includes:

1. Data Loading  
2. Trace Extraction  
3. Variants Analysis  
4. Data Encoding  
5. Trace Clustering  
6. Decision Tree Realization  
7. Decision Table Construction  
8. Rules Extraction  
9. Rules Translation  

---

### Prerequisites

The pipeline was tested using **Python 3.13.2** and the packages listed in `requirements.txt`. You can install them using:

```bash
pip install -r requirements.txt
```

### Virtual Environment

If you are using a virtual environment, which is advised, make sure to activate it before installing dependencies.

---

### Datasets

For testing, three datasets are used:

- `order.xes`
- `sepsis_10.xes`
- `hospital_12.xes`

You can replace `nameOfTheDataset.xes` in the notebook with any of the provided datasets to run the analysis.

---

### Pipeline Steps

1. **Data Loading**: load the event log and inspect its structure. Each case is identified by a unique case_id, and each event contains attributes such as:

    - concept:name (activity name)

    - time:timestamp (event timestamp)

2. **Trace Extraction**: each trace corresponds to a single case and is represented as an ordered sequence of activities.

3. **Variants Analysis**: unique sequences of activities are identified as trace variants. Each trace is assigned a Variant ID.

4. **Data Encoding**: activities and transitions are transformed into frequency-based features:

    - n if the activity/transition occurs n times in the trace

    - 0 otherwise

    This enables numerical analysis of trace behavior.

5. **Trace Clustering**: traces are grouped using K-Means clustering:

    - Silhouette score is used to select the number of clusters

    - Each cluster should correspond to one variant

6. **Decision Tree Realization**: a decision tree classifier models the relationship between trace features and cluster membership. Feature importances are extracted to identify the most influential activities or transitions.

7. **Decision Table Construction**: the decision tree is converted into a decision table, where:

    - Rows represent conditions (features) or actions (variants)

    - Columns represent distinct rules extracted from paths in the tree

8. **Rules Extraction**: rules are extracted from the decision table as IF–THEN statements, specifying:

    - Conditions on features

    - Resulting variant assignment

9. **Rules Translation**: rules are translated into human-readable explanations:

    - Activity and transition names are cleaned and capitalized

    - Numerical thresholds are interpreted

    - Self-loops and repeated activities are handled

    - Condition frequency and polarity are visualized using a color scale: intensity reflects how discriminative a condition is (rarer conditions appear darker, common ones lighter), while color indicates polarity (green for presence and red for absence)
