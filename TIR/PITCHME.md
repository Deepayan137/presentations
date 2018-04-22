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
 
![flowchart](./TIR/flowchart.jpg)

---

### Proposed aproach

1. We retrieve a list of documents which are relevant to the short text.

2. Selectively pay attention to each retrieved document and then rank the documents.

3. Do iterative expansion of the short text and conduct multiple rounds of search.

---

### Pipeline
![flowchart](./TIR/model.png)

---

<br>

**Problem Definition**: 

An end to end trainable deep memory network is proposed which extracts useful information from relevant documents and integrates it with the orignal short text.


* Given a collection of long documents $C$ and a query $q$, we first retrieve a set of potential relevant documents $C_q$.

* We aim to learn a function $f$ that expands a short text into richer representation $q'$ i.e. $q'=f(q,C_q)$. 

* Based on richer representation $q'$, we can accurately classify short text into one of the predefined categories $y$.

---

The proposed architecture consists of five different modules:

1. Retrieval Module
2. Short Text Representation Module
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
### Short Text Representation Module
* Each word in a short text query is represented as a 100-dimension vector ($w_1, w_2, ..., w_n$).

* To represent a whole query ($\hat{q}$), we simply take the average of all the word representations.
$$\hat{q} = \frac{\sum_{i=1}^{n} w_i}{n}$$
* We use GloVe library for word representation.
---

### Long Document Representation Module

* Each word in a long document $d_i\in C_q$, is represented as a 100-dimensional vector ($w_1, w_2, ..., w_n$).

* To present a whole document ($d_i$), we again take average of all the words.

$$ d = \frac{\sum_{i=1}^{n} w_i}{n}$$

* We use GloVe library for word representation.

---

### Expansion Module

1. Given query representation $\hat{q}$ , what information should we read from the context memory?

2. How to integrate the information in the orignal query representaion $\hat{q}$ with the information extracted from memory $d_i$?

*Soft Attention:*

$$a_i = softmax(\hat{q}^T d_i)$$

Information read from the documents are defined as:

$$\hat{o} = \sum_{i=1}^{K}a_id_i$$

---

### Short Text Expansion 

* How to integrate information from original representation $\hat{q}$, and information from retrieved memory $\hat{o}$?

* We can make use of an GRU unit where the hidden states are initialized as $\hat{o}$ and input is $\hat{q}$. 

* GRU should be able to automatically weight the importance of query and long documents.

* We repeat the above procedure several times using multiple hops in the *short text expansion* to get the expanded representation $\hat{q'}$.

---
### Classification module

* In this module, we keep the orignal short text representation $\hat{q}$ and represent the final short text representation as $q_{final} = [\hat{q},\hat{q'}]$, which is then used to predict the category of the short text.

* A fully connected layer is first applied to the short text representation followed by a 
softmax transformation, which gives us a distribution over the categories.

$$p(y|q_{final}) = Softmax(W^y q_final)$$

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

