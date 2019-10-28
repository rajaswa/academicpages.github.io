---
title: 'Gender Bias in Natural Language'
date: 2019-10-15
excerpt: "How has our language evolved over time with respect to gender binarism?<br/><img src='https://i.imgur.com/FuOViKS.png'>"
permalink: /posts/gender-bias-in-nlp-blog-post-3/
tags:
  - Bias
  - Natural Language Processing
  - Gender
  - Interpretability
---

Language & Vision, the two major driving domains in the latest AI trends. Vision had its moment during the ImageNet period, while Language is currently catching up with the advances in Vision with the latest developments in transformer based language models. Even though both Language & Vision have more or less equally advanced in the realm of AI, the development of Natural Language itself has been way more different than that of vision.

Unlike vision, language was not naturally found in the civilisation. It has developed over thousands of years and has been heavily influenced by multiple factors like the location, race, ethnicity, religion, migration, industrialisation and of course the evolution of civilisation itself. Every small aspect of language has the influence of human society in it, unlike the visual aspects, which are mostly naturally found in the universe.



<img src="https://i.imgur.com/Yx3FtYN.jpg" alt="Imgur" style="zoom:150%;" />



This brings up a very important challenge in Natural Language: ***The Reflection of Our Biases in Natural Language***. Our languages are often the reflection of our own society, the biases, the inferences, the beliefs and the traditions, its all there, incorporated in our language. 

Now, a fairly educated individual might get over these barriers, but how do you make a statistical model overcome them?

Bias, specifically gender bias in natural language has been a huge challenge. All the civilisations have used the gender binarism as metaphors for the *strong vs gentle (Gods from Greek civilisation), for the good vs bad (Yin-Yang in Confucianism), for power vs knowledge (Shakti & Vidya in Hinduism)* . Centuries later, we are facing the aftermath of this strong inclusion of gender bias into our language as one of the most prominent issues.

<img src="https://i.imgur.com/hnYqjRl.png" alt="yin" style="zoom:33%;" />



This has resulted in ***"Diversity & Bias"*** being one of the ever-present topics in all the top NLP conferences. NAACL 2019 had *Diversity & Inclusion* and *Bias* as two of its topics. I came across this paper from NAACL 2019 from researchers at Allen Institute and Bar-Ilan University: *Lipstick on a Pig: Debiasing Methods Cover up Systematic Gender Biases in Word Embeddings But do not Remove Them* ([H Gonen &  Y Goldberg](https://www.aclweb.org/anthology/N19-1061.pdf)) . This short paper was an eye-opener for the current state of NLP when it comes to Gender Bias. And it certainly inspired me to write about it for the general public.

Before going into the issues discussed in this particular paper, it is important to visualise the current methods of language modelling as a functionality mapping to high-dimensional vector space. Be it the traditional bag-of-words or skip-gram based static word embeddings like Glove & word2vec or the contextual word embeddings from language models like ELMo and BERT, all these techniques simply map the entities of the language (character sequences or words) into a high-dimensional space to capture the meaning of that entity. Now, almost all your Natural Language tasks are performed on these embeddings, if they are mapped wrongly in the space, the very same wrong mappings will be used as knowledge source by your statistical models.

Now, the problem arises when the biases present in the human society start reflecting their presence in this mapping. Gender bias is one of the most prominent example of this. So, before talking about Gender bias in NLP, lets discuss the gender bias in our society. 

From a Psycholinguistics perspective, what's the first mental image formed in your mind, when you hear the word *Soldier*. For most of you, the mental image would have been something like this:

<img src="https://i.imgur.com/7Jwzoi1.jpg" alt="army" style="zoom:13%;" />

A ***MAN***. A ***MALE*** personality.

Why not a *female* soldier? 

Now, go preform an image search on google for *Soldier*, the first picture with female soldier presence is at the 33<sup>rd</sup>  position in the search results. That is, no female solider result in first 32 entries, coincidence?

According to Wikipedia: *There have been women in the United States Army since the Revolutionary War, and women continue to serve in it today. As of fiscal year 2014, women are approximately 14 percent of the active duty Army, 23 percent of the Army Reserve, and 16 percent of the Army National Guard.* 

A considerable ratio, still it has been incorporated into our language (and our mindsets) that a female cannot be a symbol of strength or violence.

Now, why does this matter anyways? Suppose, the US army automates its recruitment process by involving statistical natural language based models. Now, if this very same bias is incorporated in the mapped embeddings, its highly likely that the model will act against the female recruits and favour the male recruits, further deteriorating the sex ratio. Now that we know what the problem is, lets move on to how to tackle it.

To broadly classify the solutions available to tackle gender bias in natural language with respect to the embeddings, there are two categories: 

1. Training the word-embedding model with extra caution against gender bias
2. Debiasing the already trained word-embeddings with existing gender bias

Now, most of the techniques belong to category 2 and the techniques from category 1 are not that feasible or effective. To measure how gender biased a particular embedding is, we find its component along the bias vector. (remember the embeddings are nothing but a vector in the high dimensional space?)  

```
vector(bias) = vector("he") - vector("she") = vector("male") - vector("female")
```

Most of the literature follows the "he-she" convention to map the bias vector. This bias vector gives us the direction of gender bias in the mapping space. A word with absolutely zero bias (ie. gender-neutral) will be orthogonal to the bias vector in this space. The higher the magnitude of the component along the bias vector, lower the gender neutrality of that word in the embedding space.

We have been blindly following techniques from category 2 without much Interpretability of these deep learning based word embeddings. These techniques de-bias the word embeddings by eliminating their component along the bias vector. But as shown by [H Gonen &  Y Goldberg](https://www.aclweb.org/anthology/N19-1061.pdf) , that is not enough. These techniques only cover-up the gender bias with respect to the direction along the bias vector, but the spatial arrangement of these gender biased entities in the embedding space still holds the biased knowledge which we are yet to deal with. 

As, we can clearly see, they have easily clustered together the biased embeddings with respect to some professions, providing evidence of a certain gender-based *groupism* in these professions.

<img src="https://i.imgur.com/FuOViKS.png" alt="cluster"  />

Now this once again opens up a huge paradigm of solving the issue of gender bias in NLP with respect to the directional as well as the spatial properties of the embeddings. The whole discussion here was based on gender binarism, but as more and more fractions of world population is accepting the other genders, this problem can be expanded to other genders as well. 

We do want smart AI entities in the future, but we do need them to be better at certain things than the human civilisation itself, especially with respect to issues as critical as this one. I am currently exploring this issue and am open up for discussions on the same. Let's push forward for a better future in Natural Language Processing!









