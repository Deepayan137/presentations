# Short Text Expansion

### By Avijit.d and Deepayan (CVIT)

---

## Introduction

<br>

What is short text expansion ?
---

## Examples of short text

* "Watching that movie makes me ROFL!"

* "I bend backwards"

* "RIP, David Eddings."

The above sentences are taken from Twitter Sentiment dataset. 

Note:
As we can see the sentences don't make whole lot of sense to us, since the context is not present. Our immediate response would be to open up a search engine and and put the short text into the search box because we beleive that among billion of web pages there will exist some web page that will help us make sense of the short text. We sometimes also, update the query and perform another round of search.

---
### How do humans expand short text?
 
flow chart

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
### Short Text and Long Document Representation Mode

$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$

