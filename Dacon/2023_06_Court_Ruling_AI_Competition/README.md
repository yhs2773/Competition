# Court Ruling AI Competition
## Overview
Period: 2023.06.05 ~ 2023.07.03
<br>
<br>
Organized by Dacon, the purpose of this competition was to develop an AI capable of predicting legal case outcomes.

Exploring how an AI can be effectively leveraged in the legal domain, this competition paved the way for innovative application in the field of law.

At the heart of this competition lies the quest to unlock the potential of artificial intelligence in the realm of jurisprudence. A task was to harness the power of data and cutting-edge algorithms to create a model that excels in predicting court rulings.

## Data Analysis
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

#### Handle imbalanced data
As mentioned in data analysis, the dataset had 1s and 0s, with 1 meaning first party winner and 0 meaning second party winner and had a ratio of 2:1, which showed some imbalance in a data.

