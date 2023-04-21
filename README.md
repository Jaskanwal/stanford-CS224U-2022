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

## Assignment 3: [Few-shot OpenQA with ColBERT retrieval](hw_openqa.ipynb)
The goal of this homework is to explore few-shot (or, prompt-based) learning in the context of open-domain question 
answering.

Our core task is **open-domain question answering (OpenQA)**. In this task, all that is given by the dataset is a 
question text, and the task is to answer that question. By contrast, in modern QA tasks, the dataset provides a text 
and a gold passage, with a guarantee that the answer will be a substring of the passage.

OpenQA is substantially harder than standard QA. The usual strategy is to use a retriever to find passages in a 
large collection of texts and train a reader to find answers in those passages. This means we have no 
guarantee that the retrieved passage will contain the answer we need. If we don't retrieve a passage containing 
the answer, our reader has no hope of succeeding. Although this is challenging, it is much more realistic and widely 
applicable than standard QA. After all, with the right retriever, an OpenQA system could be deployed over the entire Web.

The task posed by this homework is harder even than OpenQA. We are calling this task few-shot OpenQA. 
The defining feature of this task is that the reader is simply a general purpose autoregressive language model. 
It accepts string inputs (prompts) and produces text in response. It is not trained to answer questions per se, and 
nothing about its structure ensures that it will respond with a substring of the prompt corresponding to anything 
like an answer.

Few-shot QA (but not OpenQA!) is explored in the famous GPT-3 paper 
([Brown et al. 2020](https://arxiv.org/abs/2005.14165)). 
The authors are able to get traction on the problem using GPT-3, an incredible finding. Our task 
here – few-shot OpenQA – pushes this even further by retrieving passages to use in the prompt rather than assuming 
that the gold passage can be used in the prompt. If we can make this work, then it should be a major step towards 
flexibly and easily deploying QA technologies in new domains.

In summary:

| Task                 | Passage given | Task-specific reader training |Task-specific retriever training  | 
|---------------------:|:-------------:|:-----------------------------:|:--------------------------------:|
| QA                   | yes           | yes                           | n/a                              |
| OpenQA               | no            | yes                           | maybe                            |
| Few-shot QA          | yes           | no                            | n/a                              |
| **Few-shot OpenQA**  | no            | no                            | maybe                            |

The goal of this assignment is to explore the final line in the above table and develop an original system to
perform open Few-shot OpenQA using ColBERT as a retriever. The following are the key details of the developed system:
1. Use the pre-trained ColBERT retriever for identifying relevant passages for the given question.
2. For every passage/question pair, use the SQUAD train data set to find relevant passage/question/answer pairs for 
    doing few-shot learning.
3. Concatenate the passage/question/answer pairs (obtained in step 2), the passage (obtained in step 1), and the
    question of interest to generate a prompt for language model and obtain the output sequence as a candidate
    answer.
4. Score the answers (one answer per passage obtained in step 1) to find the best answer for the asked question.
5. Gridsearch on the key  model parameters to find best combinations.
More details on the model can be found in the [linked notebook](hw_openqa.ipynb).