# Predicting Citation Scores of Astrophysics Publications

This program is meant to predict citation counts of publications found in the SAO/NASA Astrophysics Data System (ADS).

The final or near-final citation count may not be realized for multiple years. Therefore it is of significant interest to predict the future citation count of an article. There is also the interesting question of how much predictive power a model receives from the underlying paper metadata it works with. The current project focuses on this last point, attempting to quantify the relative importance of author and affiliation metadata versus title and abstract metadata on eventual citation count.

The goals of the project are the following:
1. Predict the citation count of publications from the ADS using three different models.
(a) Model-A with features related to the authors and affiliations.
(b) Model-P with features related to the title and abstract of the paper.
(c) Model-AP with features related to the both the authors and affiliations, as well as the title and abstract of the paper.

2. Maximize model performance while focusing on a relatively small set of features derived from title, abstract, authors, and affiliations.

3. Evaluate the performance of the three models to determine which model produces the best results.

4. Determine the relative importance of the sets of features to quantify the impact of an author or affiliation versus the contents of the abstract.

5. Add to a growing body of research related to citation count prediction from various article features.

## Data
Papers included in the project were restricted to the following five years: 2001, 2002, 2003, 2004, and 2005. The time frame was chosen because the citation rates of these papers should be relatively static at this point. I made the assumption that any new changes in the citation rates are negligible. Analyzing a five year period of allowed me to draw appropriate conclusions about citations 15-20 years in the future.

There are 1,438,932 paper instances between 2001 and 2005, so papers for the training and test set were randomly extracted. For each year, 2,000 papers were randomly selected. The papers fell into groups of 500 papers: low citation count (0-10), low/medium citation count (40-50), medium citation count (80-100), and high citation count (400-6000) Figure 1d. The binning of papers into various citation counts groups ensures that there is enough spread in the data for the regression task.

## Models
There are two baseline models. The first baseline model incorporates random citation counts between 0 and the maximum citation count. In the second baseline model, the citation count is always equal to the most common entry across all citation counts.

For the baseline model, I performed tokenization on author affiliations using NLTK1 and fit an SVR model with X and y using scikit-learn2, where X is the vocabulary matrix, and y is the citation counts. The SVR also included both a linear and rbf kernel. The fit baseline model can then be used predict citation counts. For each model, the pre-processed text is split into appropriately sized training (80% of data) and test sets (20% data).

In order to create the three trained models (A, P, AP), I also use an SVR regression model. After an SVR model is fitted to the training data, it can be used to predict the citation numbers for the test set. The relevant vocabularies for each model were strung together in rows of the input matrix as 0s and 1s to represent which features were absent and present in each entry.

## Acquiring Keys from ADS API
ADS API keys can be acquired from https://ui.adsabs.harvard edu/. Log into the site, and generate an API token under account settings. Information about the ads package can be found here: 

Docs: https://ads.readthedocs.io/
Repo: https://github.com/andycasey/ads
PyPI: https://pypi.python.org/pypi/ads

## Secrets
Store the API Key in a file called 'secrets.py'. The secrets file will be imported in the main program file.

Use the following format and enter your own key.
```
ADS_DEV_KEY = ""
```

## Requirements
Use the package manager pip to install packages.

```
pip install ads, numpy, pandas, ast, matplotlib, seaborn, iteration_utilities, sklearn, nltk
```

## Plots
The model will generate the following plots:

1. Citation Count distributions
2. SVR linear model results for Model A, P & AP
3. RBF SVR results for Model A, P & AP
4. Baseline models

## Support
If you have any issues, comments, or questions please contact wernerck@umich.edu.
