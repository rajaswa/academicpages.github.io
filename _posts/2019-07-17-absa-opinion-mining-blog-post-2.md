---
title: 'Developing Data-domain Independent Opinion Mining and Aspect Based Sentiment Analysis Model in NLP'
date: 2019-07-17
excerpt: "Say goodbye to text-data domain bias !<br/> <img src='https://i.imgur.com/VVSxhpM.png'>"
permalink: /posts/absa-opinion-mining-blog-post-2/
tags:
  - Natural Language Processing
  - Sentiment Analysis
  - Bias
---

![blog2](https://i.imgur.com/VVSxhpM.png)

The issue of bias in Machine Learning is well known. The inability of the learning based models to perform on domains different than that of the training data can be really frustrating. Opinion-mining is one of the actively explored tasks in NLP, both in industry and academia. It has endless uses in social media analysis, user-reviews analysis, large scale sentiment analysis, aspect based sentiment analysis etc. The problem of training data-domain dependence is also observed in opinion mining. Let's see how we can eliminate this problem with respect to this particular task.



## Lack of generalized datasets

There is a lack of generliazed datasets for tasks like opinion-mining and aspect based sentiment analysis. Most of the adequately sized datasets for these tasks are heavily dependent on the domain of the data. Ex: [SemEval-2014 Restaurants & Laptops reviews data](http://alt.qcri.org/semeval2014/task4/index.php?id=data-and-tools), [FiQA-2018 Financial Microblogs Data](https://sites.google.com/view/fiqa/home) etc Any learning based models for these tasks trained on these kind of datasets won't deliver on data from sources other than restaurant reviews, laptop reviews etc 

## Dividing the task into data-domain independent sub-tasks

The task of opinion-mining or aspect-based sentiment analysis can be divided into four sub-tasks which are interlinked by basics of linguistics and are easily achievable. Moreover these tasks will be almost training data-domain independent. This approach is based on the one proposed in the following [paper](http://ceur-ws.org/Vol-1874/paper_6.pdf) by [Marco Federici](https://scholar.google.com/citations?user=TfInmkIAAAAJ&hl=en) &  [Mauro Dragoni](https://scholar.google.com/citations?user=88gjGJ0AAAAJ&hl=en) for Multi-domain sentiment analysis. The four sub-tasks can be listed as follows:

*	POS-tagging 
*	Dependency parsing
*	Co-reference resolution
*	Rule based entity-opinion pair generation

## The Approach

Once you've obtained the POS tags and Parsing tree for the text content, the only thing left to do is extracting the entity-opinion pair by using the following simple rules:

*	If the dependency between two words is Noun-subject and the governing word is an adjective, then that is an entity-opinion pair.
*	If the dependency is Adjective Modifier and the governing word is an adjective, then that is an entity-opinion pair.
*	If the dependency is Direct object and the governing word is an adjective, then that is an entity-opinion pair.
*	Once you have all the pairs, you simply need to replace all the pronouns with the actual entity in context using Co-reference resolution.

This pipeline will provide very good results for opinion-mining or aspect based sentiment analysis for any kind of text data domain.
Using the [POS Tagger](https://nlp.stanford.edu/software/tagger.html), [Dependency Parser](https://nlp.stanford.edu/software/nndep.html) and [Co-reference resolution tool](https://stanfordnlp.github.io/CoreNLP/coref.html) from the [Stanford CORENLP-toolkit](https://stanfordnlp.github.io/CoreNLP/), these types of results can be obtained :

| Text  | Entity | Opinion-word |
| ------------- | ------------- | -------------|
| 'IPL 2019: MS Dhoni fined after fierce on-field argument with umpires in tense CSK chase'  | 'argument'  | 'fierce', 'on-field' |
| 'IPL 2019: MS Dhoni fined after fierce on-field argument with umpires in tense CSK chase' | 'csk-chase'  | 'tense' |

## The Python code

```python
#Imports

from stanfordcorenlp import StanfordCoreNLP
nlp = StanfordCoreNLP('./stanford-corenlp-full-2018-10-05')

import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer
nltk.download('vader_lexicon')
sid = SentimentIntensityAnalyzer()
```

```python
#Concatenating two consecutive noun entities

def pre_process(doc):
  noun_tags = ['NN', 'NNS', 'NNP', 'NNPS']
  noun_pairs = []
  
  new_doc = ''
  doc_lower = doc.lower() 
     
  pos_tags = list(nlp.pos_tag(doc_lower))
  
  for i in range(len(pos_tags)-1):
    if (pos_tags[i][1] in noun_tags) & (pos_tags[i+1][1] in noun_tags):
      noun_pairs.append(i)
     
  for i in range(len(pos_tags)):
    if i in noun_pairs:
      new_doc += str(pos_tags[i][0]) + '-'
    else:
      new_doc += str(pos_tags[i][0]) + ' '
      
  return new_doc
```

```python
#co-reference resolution 

def coref(doc):
  coref_list = nlp.coref(doc)
  coref_set = []
  for i in range(len(coref_list)):
    similar_tokens = []
    cluster = coref_list[i]
    for j in range(len(cluster)):
      similar_tokens.append(cluster[j][-1])
    
    coref_set.append(similar_tokens)
  
  return coref_set

```

```python
#Getting opinion pairs

def get_opinion_pairs(doc, polar_threshold):
  doc = pre_process(doc)
  coref_list = coref(doc)
  parses = nlp.dependency_parse(doc)
  
  targets = []
  opinion_pairs = []
  final_opinion_pairs = {}
  id2token = {}
  
  tokens = nlp.word_tokenize(doc)
  
  for i in range(len(tokens)):
    id2token.update({ i+1 : tokens[i]})
  
  for triplet in parses:
    relation  = triplet[0] 
    if relation == 'ROOT':
      continue
      
    governor, dependent = id2token[(triplet[1])], id2token[(triplet[2])]
    
    gov_polarity = (sid.polarity_scores(governor))['compound']
    dep_polarity = (sid.polarity_scores(dependent))['compound']
    
    if  (relation == 'nsubj') & (abs(gov_polarity) >= polar_threshold):
      opinion_pairs.append((dependent, governor))
      
    elif (relation in ['amod']) & (bool(nlp.pos_tag(dependent)[0][1] in ['JJ', 'JJR', 'JJS'])):
      opinion_pairs.append((governor, dependent))
      
    elif (relation in ['dobj']) & (bool(nlp.pos_tag(governor)[0][1] in ['JJ', 'JJR', 'JJS'])):
      opinion_pairs.append((dependent, governor))
      
  opinion_pairs = list(opinion_pairs)
  
  for i in range(len(opinion_pairs)):
    opinion_pairs[i] = list(opinion_pairs[i])
    for cluster in coref_list:
      if opinion_pairs[i][0] in cluster:
        for item in cluster:
          if bool(nlp.pos_tag(item)[0][1] not in ['PRP', 'PRP$', 'WP', 'WP$']):
            opinion_pairs[i][0] = item
  
  for i in range(len(opinion_pairs)):
    if opinion_pairs[i][0] not in targets:
      targets.append(opinion_pairs[i][0])      
      final_opinion_pairs.update({opinion_pairs[i][0] : (opinion_pairs[i][1]).split()})
    else:   
      if opinion_pairs[i][1] not in final_opinion_pairs[opinion_pairs[i][0]]:
        final_opinion_pairs[opinion_pairs[i][0]].append(opinion_pairs[i][1])
  
  
  return final_opinion_pairs
```

```python
opinion_pairs = get_opinion_pairs("Sample Text")
```











