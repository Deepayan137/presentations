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

#### The above sentences are taken from Twitter Sentiment dataset. 

Note:
As we can see the sentences don't make whole lot of sense to us, since the context is not present. Our immediate response would be to open up a search engine and and put the short text into the search box because we beleive that among billion of web pages there will exist some web page that will help us make sense of the short text. We sometimes also, update the query and perform another round of search.

---
# How do humans expand short text?

### flow chart

---

# Proposed aproach

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

The proposed architecture 




