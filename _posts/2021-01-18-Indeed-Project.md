---
layout: post
title: You're going to need a bigger scrape - with apologies to Jaws.
---

Predicting Data Science Salaries from the Indeed website.

"You're going to need a bigger scrape" - with apologies to Jaws.

1) What was I trying to do?

Predict if an annual salary was going to be higher or lower than median for a data science role for a sample set scraped from the Indeed website - based on location, title and summary description.

1) Scraping the Indeed website.

A long and complicated process. Tuning required for number of search results available per search (1000), changing job title and location, variations in the html structure of the individual job postings (as well as the sleep time between page grabs to avoid overloading the web page). The eventual result was 1186 unique descriptions (with preprocessed annual salaries) from an initial raw list of nearly 8000 results. Earlier attempts harvested 5-800 results from lists of over 20,000 (many duplicates!). The preprocessing included cleaning the salary data (extracting annual salaries, removing text and averaging ranges), cleaning location data (removing postcodes, making the remote category usable) and creating a binary High/Low salary variable based on the median (useful for reducing the effect of outliers) of the annual salaries.

2) Initial modelling with location data:

Firstly a baseline was established - the actual split in the initial dataset between high and low salaries.

The location data was dummified (without dropping any columns - there could well be future additional locations to consider). The cleaned remote marker column was also considered as a location predictor. After a suitable test/train split, different classification models were tried:
A) Logistic Regression (tuned with higher max iterations) - OK results. Useful information from the model coefficients in terms of better/worse locations.
B) K nearest neighbours with default nearest neighbours value of 5 - not good results.
C) DecisionTreeClassifier (no tuning) - better results than knn, feature_importances consistent with Logistic Regression coefficients.

3) Preparing Title and Description columns and modelling (basic, tuned and ensemble.
The job titles and summary descriptions have useful modelling prediction possibilities (due to key words in the title - eg Senior or skills in the summary - eg R, SQL) which we can use after the right processing. In order to maximise the usefulness, the columns were treated with NLP count vectorising (to extract the most useful predictive words without assumptions) in order to provide modelling material to combine with the location.
The text columns were cleaned of punctuation and other characters that might confuse the count vectoriser. Count vectorisation was applied to the two text columns as well as one_hot to the undummified location column and remote column (to mimic the dummification process used earlier for the location only model) and then modelling with the 2 better modelling approaches from above wer applied.

A) Logistic Regression which provided much higher accuracy in the cross validated scores than location only. A gridsearch tuning provided slightly improved scores with an alpha strength of 0.04 (but not a big improvement). The summary and title data is useful in the modelling alongside the location data.

B) Decision Tree with also provided higher accuracy (although not as high as the Logistic regression). A gridsearch tuning exercise with this model was not successful in improving the scores.

Another tuning approach was tried for improving the models - ensemble methods.

C) Bagging with DecisionTreeClassifier as the base model (tuned with higher number of estimators) - an improvement in the results (cross validated training score and test score improvements).

D)  Random Forest (tuned with higher number of estimators) which gave the best cross-validated training score and a slightly worse test score - but would appear to be the best model to use for this exercise.

It is useful to note at this point that the task brief to minimise false positives (wrongly predicting a high salary) is best served by the last model as well - with the best AUC (0.92) and AP (0.92) scores from the ROC and Precision/Recall curves. 

Gridsearch tuning on these last 2 models to improve precision without sacrificng other performance was not helpful, so another method was tried: varying the probability threshold for declaring a True result for both the models (ie making it less likely that any data point would be predicted as true). This has the effect of reducing false positives as well as increasing false negatives.

Remembering that recall = tp / (tp + fn), precision = tp / (tp + fp). High recall means low false negatives. High precision means low false positives - so we're looking for high  precision (subject to sufficient accuracy!). The varying threshold method gives us a good view of what to vary to achieve the most useful results.

The sequence of confusion matrices for each model as the threshold is varied gives a usseful discussion tool for the end-user - how many false positives can you live with, given the implications shown in the matrices? Model D gave the best results for this exercise (quickest reducing false positives without the accuracy declining too fast).
