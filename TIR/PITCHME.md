# Short Text Expansion

### By Avijit Dasgupta and Deepayan Das (CVIT)

---

## Introduction

<br>

What is short text expansion?
---

## Examples of short text

* "Watching that movie makes me ROFL!"

* "I bend backwards"

* "RIP, David Eddings."

The above sentences are taken from [Twitter Sentiment dataset](http://thinknook.com/twitter-sentiment-analysis-training-corpus-dataset-2012-09-22/). 

Note:
As we can see the sentences don't make whole lot of sense to us, since the context is not present. Our immediate response would be to open up a search engine and and put the short text into the search box because we beleive that among billion of web pages there will exist some web page that will help us make sense of the short text. We sometimes also, update the query and perform another round of search.

---
### How do humans expand short text?
 
<a><img src="flowchart" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

---

### Proposed aproach

1. We retrieving a list of documents which are relevant to the short text.

2. Selectively pay attention to each retrieved document and then rank the documents.

3. Do iterative expansion of the short text and conduct multiple rounds of search.

---

# Pipeline

### diagram from paper

---

<br>

An end to end trainable deep memory network is proposed which integrates useful information from relevant documents and integrates it with the orignal short text.

**Problem Defination**: 

> Given a collection of long documents C, we aim to learn a function f that expands a short text into richer representation q' i.e. q'=f(q,C). Based on richer representation 
q', we can accurately classify short text into one of the predefined categories y.

---

The proposed architecture consists of five different modules:

1. Retrieval Module
2. Short text Representation Module
3. Long Document Representation Module
4. Expansion Module
5. Classification Module

---

### Retrieval Module

* We first use orignal short text q as a query to search for a set of potentially relvant long documents Cq from an external large collection C.

* We have made use of a python search engine library called woosh for obtainig the set of relevant long documents.

``` python
with idx.searcher() as searcher:
try:
    results = searcher.search(query, limit = top_k)
except TermNotFound:
    results = []
for hit in results:
  file_names.append(hit['text_filename'].encode('utf-8'))
```
@[3]
@[5-6]
---
### Short Text Module

Each short text *q = w1,....,wn* is represented as a *d*-dimensional vector q' in a continuos space. Each word is represented as a *d*-dimensional vector and then we take the average of all the word vectors in order to represent the short text. We have used GloVe for obtaining the word embeding.

**insert math equation**

<!-- $$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$ -->

---

### Long Document Representation Module

Each long document is also representated as a d-dimensional vector space. Similary,
each document is *di = w1,....,wn* are represented as the average vector of the words 
in the documents, i.e,

**insert equation**

---

### Expansion Module

This module is made up of two components:

1. given query representation *q'* , what information should we read from the memory

2. how to integrate the information in the orignal query representaion *q'*?

*Memory Reading*: We try to identify relevance of each document with respect to the query. This is achieved by calculating the inner product between the query *q'* and each document *di* and later a soft max function is used to calculate the attention probaility of each document *i* in the memory.

**insert softmax equations**

---

*Short Text Expansion*: We try to reformulte the orignal short text with the information from the memory reading component. For this we used a Gated Recurrent Unit , which is automtically able to determine the weight of the two sources of information.

**insert equation**

We repeat the above procedure several times using multiple hops in the *short text expansion*. More specifically, when an expanded representation *q''* is output by the *short text expansion* module, *q''* is treated as an initial query to the module. After repeating the above procedure for several times, the final output represenatation is used as the representation of the orignal query *q'*.

---
### Classification module

In this module, we keep the orignal short text representation *q'* and represent the final short text representation as a concatenation of *q'* and *q''* i.e 
*q_final = [q',q'']*, which is then used to predict the category of the short text.

*insert equation 12*

A fully connected layer is first applied to the short text representation followed by a 
softmax transformation, which gives us a distribution over the categories.

---

### Experiments

#### Data Sets

We have used Twitter Data set, a large corpus for positive and negative sentiment classification. For each short text we associated top 20 relevant document as its memory.

---
### Results

**insert table containing results**


---
### Conclusion


---

