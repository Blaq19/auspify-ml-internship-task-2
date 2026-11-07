# Netflix Content Type Prediction Model: Task 2 Documentation

Auspify Technologies Machine Learning Internship | Task 2 (Easy) | Author: Elliot Agyare



1. Project Overview

---

Objective: develop a machine learning model that predicts whether a Netflix title is a Movie or a TV Show based on its available features.

Approach in one paragraph: the title type is converted to a 0/1 target (1 = TV Show). Features such as rating, country, release year, year added and number of directors are encoded as numbers, and three classification models (Logistic Regression, Decision Tree and Random Forest) are trained and compared on unseen test data. Two extra experiments show how data leakage from the duration and genre columns inflates the scores. 



# 2\. Dataset

• File: Dataset.csv, 8,790 rows and 10 columns, reduced to 8,787 rows after removing 3 duplicates.

• Target: the type column, which is 69.7% Movies and 30.3% TV Shows.

• Baseline: a model that always answers "Movie" is already right 69.7% of the time, so any useful model must beat that number.

• Data quality: missing values are stored as the text "Not Given" and were replaced with empty values.



3. Tools and Libraries
===

• Python in Google Colab

• pandas and numpy for data handling

• scikit-learn for encoding, models and metrics

• matplotlib for charts



4\. Methodology

===

#### Step 1: Select relevant dataset features

• Target: is\_tv\_show (1 for TV Show, 0 for Movie).

• Numeric features: release\_year, year\_added (from date\_added) and num\_directors (how many directors are listed).

• Categorical features: rating and country.

• Experiment-only features: duration (split into minutes and seasons) and listed\_in (genres). These were kept out of the main model on purpose, see Section 5.



#### Step 2: Encode categorical variables

• Rating and country were one-hot encoded, so each category becomes its own 0/1 column.

• Genres were split into individual genre names and converted to 0/1 columns with CountVectorizer.

• Numeric columns were passed through unchanged.



#### Step 3: Train classification models

• Data was split 80/20 into 7,029 training rows and 1,758 test rows, stratified so both parts keep the same Movie/TV Show ratio, with random\_state=42.

• Three models were trained: Logistic Regression, Decision Tree and Random Forest (200 trees).

• class\_weight was set to "balanced" so the models do not favour Movies just because there are more of them.


Step 4: Evaluate prediction performance
---

• Predictions were made on the unseen test set.

• Accuracy and the F1 score for the TV Show class were recorded. F1 is shown because accuracy alone can look good on imbalanced data.

• A confusion matrix and classification report were produced for the Random Forest.


Step 5: Compare model accuracy
---

• The three models were compared in a table and in a bar chart against the 0.697 baseline.

• The same models were also run on three feature sets to demonstrate data leakage.



## 5\. Results

Main comparison (no duration, no genres)

Model

Accuracy

F1 (TV Show)

Logistic Regression


Confusion matrix (Random Forest)

• Movies predicted correctly: 3

• TV Shows predicted correctly: 2

• Movies mistaken for TV Shows: 5

• TV Shows mistaken for Movies: 3


What this shows: data leakage means a feature contains the answer itself. A duration of "1 Season" can only be a TV Show, and "90 min" can only be a Movie. Genres such as "TV Dramas" exist only for TV Shows. Sets A and B therefore look almost perfect, while set C is the honest result because it predicts from attributes that do not state the answer.



6. Challenges and How They Were Solved

• Duration text: the duration column mixes minutes and seasons in one text field. It was split into two numeric columns with a regular expression.

• Imbalanced classes: Movies outnumber TV Shows more than two to one, so class\_weight="balanced" and F1 were used alongside accuracy.

• Suspiciously perfect scores: accuracy of 1.000 revealed data leakage rather than a perfect model.





7. Limitations
---

• The honest feature set uses only weak signals, so accuracy stops in the low 90s.

• The model was tested on one random split. Cross-validation would give a more reliable estimate.

• Predictions rely on how this dataset was collected. Fewer directors being listed for TV Shows is a pattern of the data, not a rule.



8. How to Run the Projects
---

1\. Open Google Colab and create a new notebook.

2\. Upload Dataset.csv using the folder panel on the left.

3\. Run the cells from top to bottom with Shift + Enter.



