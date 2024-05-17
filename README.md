
<div align="center">

## Stats.stackexchange Response Prediction Project 

<img src="assets/logo.svg" width="450px">

stats.stackexchange.com official logo

</div>

## The Goal

I want to be able to predict whether a question, posted on stats.stackexchange.com, would be answered or not. By 'answered' I mean it will recieve any response at all, not necessarily the one accepted by question owner. I do not count comments as responses. To get a sense of what typical questions/responses/comments look like, consider a couple examples:

1. https://stats.stackexchange.com/questions/105501/understanding-roc-curve - a question with both comments and responses (one of them is marked as accepted)
2. https://stats.stackexchange.com/questions/123026/rademacher-complexity-of-logistic-regression - a question with comments but no responses
3. https://stats.stackexchange.com/questions/641226/should-i-include-a-dummy-variable-for-groups-with-few-observations - no responses, no comments

Ultimately, it would be nice to understand factors, driving questions' appeal. Why some attract so much attention, while others - none at all? Those factors might shed some light on how Q&A communities like SE operate, how to improve questions (for users) and how to tweak recommendation system to everyone's advantage.  

## Navigation

- [`notebooks/report.ipynb`](notebooks/report.ipynb) tells the whole story (go there), but it's a work in progress
- [`notebooks/data_retrieval_utils.ipynb`](notebooks/data_retrieval_utils.ipynb) contains utils for scraping and API access   

- `requirements.txt` refers to https://github.com/forveg/utils.git@dev - those are my own utils
- ignore `requirements_embeddings.txt` unless you're going to create embeddings from scratch (ready embeddings are available, see below )

## Data Sources

1. Raw Data For Feature Construction
    1. [Official data dump at archive.org](https://archive.org/details/stackexchange_20231208) (554 MB .7z archive, unpacked 3GB)

    The rest is available upon request:

    2. Related Questions (`related_full.json`, 8.7 MB)
    3. Reputation History (`reputation_hist_full.json`, 260 MB unpacked)

2. Intermediate inputs (ready for training, to skip lengthy feature construction)

    1. Tabular data (w/t embedding) (`df_no_emb_Feb21.parquet`, ~40 MB)
    2. Two `.npy` embeddings ([OneDrive](https://1drv.ms/f/c/c7230f4150876442/Ep6KHXQ1ipNEs5gP--iFcbYBGvPfbbsc5Y6LGHnXl0H9yg?e=0vcE2W), ~1.1 GB total)
    3. `.npy` response timing, from which the target is obtained via thresholding, e.g. 14-days-threshold (`timing.npy`, 1.5 MB) 
 
I used official stackexchange [data dump](https://archive.org/details/stackexchange_20231208) (release 2023-12-08).

The dump includes Linked questions, but not Related ones. Linked questions are explicitly (manually) referenced by others. Related questions are suggested by recommendation system. There are only ~50k Linked questions in the whole data dump, too few to be useful. I retrieved Related via stackexchange API (plus resorted to scraping because of API request quota).

The dump contains only partail data on user reputation, hence again I accessed reputation history via API.

## Contents

1. [The Goal](notebooks/report.ipynb#The-Goal)
2. [Available Data & Problem Framing](notebooks/report.ipynb#Available-Data-&-Problem-Framing)
    * [2.1 Q&A Total Numbers](notebooks/report.ipynb#Q&A-Total-Numbers)
    * [2.2 Response Timing](notebooks/report.ipynb#Response-Timing)
3. [Feature Design](notebooks/report.ipynb#Feature-Design)
    * [3.1 Overview](notebooks/report.ipynb#1-Overview)
    * [3.2 Individual-Question Features](notebooks/report.ipynb#2-Individual-Question-Features) 
    * [3.3 Contextual Features](notebooks/report.ipynb#3-Contextual-Features)
    * [3.4 Tricky Part](notebooks/report.ipynb#4-Tricky-Part)
4. [Data Loading](notebooks/report.ipynb#Post-History)
5. [Feature Calculation](notebooks/report.ipynb#Feature-Calculation)
    * [5.1 Sliding Window Features](notebooks/report.ipynb#Sliding-Window-Features)
    * [5.2 Non-sliding-window Features](notebooks/report.ipynb#Non-sliding-window-Features)
    * [5.3 Tags](notebooks/report.ipynb#Tags)
    * [5.4 Text Embedding](notebooks/report.ipynb#Text-Embedding)
6. [Training](notebooks/report.ipynb#Training)
    * [6.1 Data Preparation](notebooks/report.ipynb#Data-Preparation)
    * [6.2 Cross Validation Setup](notebooks/report.ipynb#Cross-Validation-Setup)
    * [6.3 Training Loop](notebooks/report.ipynb#Training-Loop)
7. [Results (So Far)](notebooks/report.ipynb#results-so-far)
    * 7.1 [Overview](notebooks/report.ipynb#overview)
    * 7.2 [Feature Importance](notebooks/report.ipynb#feature-importance)
        * 7.2.1 [All Features Top-15](notebooks/report.ipynb#all-features-top-15)
        * 7.2.2 [Non Embedding Features](notebooks/report.ipynb#non-embedding-features)
8. [Future Work](notebooks/report.ipynb#future-work)