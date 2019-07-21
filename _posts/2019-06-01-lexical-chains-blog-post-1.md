---
title: 'Lexical Chains'
date: 2019-06-01
excerpt: "Traditional NLP for text! <br/> <img src='https://i.imgur.com/KZbfa3w.png'>"
permalink: /posts/lexical-chains-blog-post-1/
tags:
  - Topic Modelling
  - Natural Language Processing
  - Semantic Relations
---


Lexical chains are connections between semantically related words & phrases in the text, separated over short or long distances. These words or phrases are called lexical items. 

*  The Lexical items will carry their own meaning.
* Lexical items relating to the same topic will form a dense chain.
* Each Lexical item must solely exist in exactly only one chain.
* Chains with only one item may exist.
* Dense chains may be linked together to generate broader topics.

![blog1](https://i.imgur.com/KZbfa3w.png)


### Candidate Lexical Items

Should be terms with some value or meaning. One should prefer shorter phrases over longer ones. 

* Nouns & Compounds
* Adjective-noun combinations



### Dense Lexical Chains

Every item of a dense lexical chain must belong to the same topic. So, a Dense lexical chain will represent just a single topic. 

* **Collocational Terms** :	Semantically related terms which often co-occur
* **Super/Sub-ordinate** :     Semantic heirarchy
* **Part-whole-relations** :   Defined for nouns



### Links between Dense Chains

Dense chains can be linked by lexical items. A link is basically a semantic relationship between two lexical items form different dense chains. 

* Two chains can be connected by a single best link only
* Multiple links from a single lexical item to different dense chains is allowed



### Pitfalls

* Homonyms & polysemy
* Two items might share some strong topic, but the current common topic in context might be a different one
* Compounds can be deceptive



### Annotation Pipeline

* **READ** the document, identify major topics
* **PICK** the next unchained lexical item
* **SEARCH** for a lexical chain or another lexical item to merge with
* **CONTINUE** the process till all the lexical items are merged
* **REVISIT** the dense chains and check for possible merging or splitting of chains to maintain a desirable average chain length
* **LINK** the dense chains if possible
* In the **END** , you'll have a few major chains for major topics and side-chains for subtopics









