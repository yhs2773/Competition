# Court Ruling AI Competition
## Overview
Period: 2023.06.05 ~ 2023.07.03
<br>
<br>
Organized by Dacon, the purpose of this competition was to develop an AI capable of predicting legal case outcomes.

Exploring how an AI can be effectively leveraged in the legal domain, this competition paved the way for innovative application in the field of law.

At the heart of this competition lies the quest to unlock the potential of artificial intelligence in the realm of jurisprudence. A task was to harness the power of data and cutting-edge algorithms to create a model that excels in predicting court rulings.

Data was comprised of the supreme court cases and a dataset had three features and a label:

| Col. name | dtype | feature/<br>label |
|--|--|--|
| First party | (string) | feature |
| Second party | (string) | feature |
| Facts | (string) | feature |
| First party winner | (integer) | label |

First party and second party consisted of the party names, facts consisted of the details of the case, and the first party winner was a label column that indicated whether a first party won the case or not (binary).

## Data Preprocessing
With the given dataset, following steps have been taken:
- Handle imbalanced data
- Cleanse text
- Stem/lemmatize words
- Use TF-IDF vectorizer

#### Handle imbalanced data
As mentioned above, the dataset had 1s and 0s as a label, indicating that it was a binary classifiaction problem and had a ratio of 2:1, which showed some imbalance in a data.

We know that imbalanced classification problems pose a risk of poor predictive modeling as algorithms assume that the ratio of classes are nearly equal. However, the label was not highly imbalanced (i.e. 10:1), so I attempted three different ways of handling the proportion of the label:
1. Not handle imbalanced data
2. Upsample the data
3. Downsample the data

When I didn't change the ratio of each class, both machine learning and deep learning models performed poorly (underfitting). The accuracy of the validation data was around 51%, just above random guessing (50%), so I approached the second option: upsampling.

Upsampling showed great results with the validation data, but overall performed poorly as well (overfitting). The accuracy of the validation data was 80%, but when submitting the prediction with the test data, the results weren't great (similar results with option 1).

So I downsampled the data and the results of both validation and test data were in the middle of option 1 and 2.

#### Cleanse text
Looking into the data, I figured that the text had errors and some unnecessary characters (irrelevant information), so I updated the feature columns with regex and stopwords:
- Transform into lowercase
- Remove parenthesis
- Remove double quotes
- Update contractions (e.g. you'll -> you will)
- Remove possessive term
- Remove special characters
- Remove stopwords (from nltk library)

#### Stem/lemmatize words
Then I used stemming/lemmatization to update words into their root forms. Unfortunately, the results after transforming words using stemming/lemmatization weren't great. Using these transforms lowered the results by 1~2%. Although, I assumed this was the right approach towards NLP data, I decided not to transform words.

#### TF-IDF Vectorizer
In order to input text data into algorithms, we have to change words/characters into a set of integers. In this case, I used TF-IDF vectorizer.

Provided by scikit-learn library, TF-IDF vectorizer converts a collection of raw documents to a matrix of TF-IDF features, which is equalivalent to transforming words with count vectorizer followed by TF-IDF. TF-IDF, which stands for Term Frequency-Inverse Document Frequency, expresses the term weightings of each word in a corpus. And count vectorizer converts a collection of text documents to a matrix of token counts.

## Model
At first, I focused on testing machine learning models as they produce faster results than deep learning models in general, but the output of predictions weren't great. To enhance results, I used AutoML libraries such as pycaret, atuogluon, etc. without any success.

Then I proceeded to utilizing deep learning models to see their performance. I've used XLM-RoBERTa and Bert, since they had pretrained weights and they did score higher than machine learning algorithms.

However, as many of us would be aware, single model itself doesn't produce higher results than an ensembled model. To strenghthen the modeling part, I chose some of the high performing models depending on their accuracy and ensembled them using a stacking classifier and that did produce the best result out of all cases.

## Result
- Ranked 11 out of 506 teams
- Was able to preprocess text data in various ways
- Was able to utilize different models and modeling methods

## Improvements
- Try different preprocessing methods for the given NLP dataset
- Try more deep learning algorithms for NLP problem like DeBERTa
- Ensemble deep learning models
- Ensemble both machine learning and deep learning models