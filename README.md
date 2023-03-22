# CS 224U: Natural Language Understanding
This repo documents my solutions for  assignments of 
[CS 224U (Natural Language Understanding - 2022)](http://web.stanford.edu/class/cs224u/). The complete code for this
class can be found [here](https://github.com/cgpotts/cs224u). In order to run the code in the solution notebooks, the
environment can be setup by running the [setup notebook](setup.ipynb).

A brief summary of key concepts covered in different assignments is summarized below.

## Assignment 1: [Word Relatedness](hw_wordrelatedness.ipynb)
Word similarity and relatedness datasets have long been used to evaluate distributed representations. 
This assignment provides code for conducting such analyses with a new word relatedness datasets. 
It consists of word pairs, each with an associated human-annotated relatedness score.

The evaluation metric for each dataset is the 
[Spearman correlation coefficient  𝜌](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient) between 
the annotated scores and your distances, as is standard in the literature.

This homework asks you to write code that uses the count matrices in `data/vsmdata` to create and 
evaluate some baseline models. The final question asks you to create your own original system for this task, 
using any data you wish. In the assignment solution, this system is designed by using an Auto-encoder (AE) to 
arrive at low-dimensional embeddings of the word count vectors, in conjunction with performing 
[Positive Pointwise Mutual Information (PPMI)](https://en.wikipedia.org/wiki/Pointwise_mutual_information) re-weighting 
and LSA on raw co-existence count vectors before generating AE embeddings. 

## Assignment 2: [Sentiment Analysis](hw_sentiment.ipynb)
This homework and associated bakeoff are devoted to supervised sentiment analysis using the ternary 
(positive/negative/neutral) version of the Stanford Sentiment Treebank (SST-3) as well as a new 
dev/test dataset drawn from restaurant reviews. The homework questions ask you to implement some 
baseline system, and the bakeoff challenge is to define a system that does well at both the SST-3 test 
set and the new restaurant test set. Both are ternary tasks, and our central bakeoff score is the mean of 
the macro-FI scores for the two datasets.

As a part of this assignment a system analysis system is developed using BERT base for tokenization and getting
the token embeddings. Then a bi-direction LSTM model with a classification head on its last hidden state is 
trained for the sentiment analysis task. Optimal hyper-parameters of the model considering the LSTM hidden dimension,
number of layers, bi-directional/uni-directional, and the classification head's activation function are identified
by performing a grid search. More details on the model can be found in the [linked notebook](hw_sentiment.ipynb).
