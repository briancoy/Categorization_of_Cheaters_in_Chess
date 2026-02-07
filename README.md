# Categorization of Chess Cheaters: A Machine Learning Approach

## Project Overview
This project investigates whether publicly available game data from **Chess.com** can be used to build a reliable automated detection system for cheaters. The goal was to determine if a machine learning framework could distinguish between honest players and those using chess engines by analyzing the relationship between **ELO rating** and **move accuracy**.

### The Problem
Cheating in chess is often subtle. Dishonest players may only use engines during critical moments, allowing them to mimic the statistical profile of average honest players. This makes them incredibly difficult to isolate using simple summary statistics alone.

## Data Engineering & Methodology
The scale of the data required significant management and a pivot from the original plan of local engine analysis to a more scalable approach using API-provided metrics.

* **Data Source:** Chess.com Public API.
* **Scale:** A final dataset comprising **22,087 players** and **12,220,862 games**.
* **Storage:** Managed using a local **SQLite** database to handle the high volume of records efficiently.
* **Feature Selection:** Analysis focused exclusively on **10-minute "Rapid" games** to provide a cleaner signal for correlating rating with accuracy.

## Machine Learning Approaches
I implemented three distinct modeling techniques to evaluate their effectiveness in separating honest players from cheaters.

### 1. K-Means Clustering (Unsupervised)
* **Goal:** To see if cheaters naturally clustered into a distinct group away from honest players.
* **Result:** Even with $k=3$ (representing Beginner, Expert, and Suspicious groups), fair play violation accounts were spread evenly across all clusters.
* **Insight:** Unsupervised grouping based on rating and accuracy does not isolate dishonest behavior.

### 2. One-Class Support Vector Machine (Anomaly Detection)
* **Goal:** To learn the "shape" of honest play and flag mathematical outliers as anomalies.
* **Result:** While the model correctly identified statistical outliers at the fringes of the distribution, the vast majority of actual cheaters were clustered tightly within the "normal" zone.
* **Performance:** The model achieved a **Recall of 0.01**, as cheating does not always manifest as a statistical anomaly in summary data.

### 3. Random Forest Classifier (Supervised)
* **Goal:** To find non-linear decision boundaries within a heavily imbalanced dataset.
* **Result:** The model performed no better than random guessing, with an **ROC AUC Score of 0.506**.
* **Insight:** Because cheaters represent less than 1% of the population, the model reached high "accuracy" simply by classifying every player as honest.

## Key Conclusions
The results of this analysis conclusively show that **summary statistics (ELO and average accuracy) are insufficient** for automated cheat detection.

* **Statistical Mimicry:** Cheaters do not consistently produce high-accuracy outliers; they often play with average accuracy or only cheat when necessary to win.
* **Future Directions:** A functional detector likely requires **move-by-move analysis (PGN data)** rather than per-game summaries to identify specific moments where a player's behavior diverges from human norms.

## Technologies Used
* **Python:** Data scraping, processing, and machine learning.
* **SQLite:** Large-scale data storage and management.
* **Scikit-learn:** Implementation of K-Means, SVM, and Random Forest.
* **Matplotlib/Seaborn:** Data visualization and exploratory analysis.
