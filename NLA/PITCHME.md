## Sentence Generation 

### By Avijit.d and Deepayan

---

### Introduction
uld be added to the state. In the next step, we’ll combine these two to create an update to the state.
Genertaing sentences is an important problem in unsupervised learning. Sentence generation finds its application in language modeling. One of the most popular 
approach towards learning sentence representation is by encoder decoder network 
via RNNs. RNNs do this by maximizing the log predictive likelihood of each token 
in the training sequence given the previous observed tokens.
---
### Problems associated with RNN language model

1. *Exposure Bias*: While trying to generate sentences from certain laten codes, the error will acumulate exponentially with the length of the sentence. The first few words can be reasonable, however the quality of sentence detoriates quickly.

2. The length of sentence generated from random latent distributiion is difficult to control.

---
### Proposed approach
 
In this project we propose to generate realistic sentences using Generative Adversarial
Networks. We use LSTMs as generators as well as discriminators. The discriminator learns to discriminate between generated and real sentences while the generator tries to fool the discriminator by generating high quality text.

---

### Problem with GANs generating sequences

GANs have difficulty in generating sequences of discrete tokens, such as texts.

*The gradient of the output of the discriminator network with respect to the synthetic data tells you how to slightly change the synthetic data to make it more realistic.
You can make slight changes to the synthetic data only if it is based on continuous numbers. If it is based on discrete numbers, there is no way to make a slight change.
For example, if you output an image with a pixel value of 1.0, you can change that pixel value to 1.0001 on the next step.
If you output the word "penguin", you can't change that to "penguin + .001" on the next step, because there is no such word as "penguin + .001". You have to go all the way from "penguin" to "ostrich".*
                   -*Ian Goodfellow*
---

### Proposed Approach

We try to learn intermediate representations that are discrete. We can use stochastic neural networks, where each layer computes the parameters of some (discrete) distribution, and its forward pass consists of taking a sample from that parametric distribution. However, the difficulty is that we can’t backpropagate through samples.

---

### Gumbel Softmax distribution

We use Gumbel distribution which can be smoothly deformed into the categorical distribution, and provides an efficient way to draw samples z. As such it allows 
gradients to flow back during back propagation.

sample = softmax((logits+gumbel noise)/temperature)

As temerature τ→0, the softmax becomes an argmax and the Gumbel-Softmax distribution becomes the categorical distribution. During training, we let τ > 0 to allow gradients past the sample, then gradually anneal the temperature .

---

### Model Architecture

* We give the generator random noise, z ∼ *N(0, I). Sample z is of shape *nxd* where n is the length of sequence and d is the fixed length noise vector at each time step.

* The generator then transforms z into a sequence of probability distributions over the vocabulary of size *nxk* where k is the size of our true data distribution’s vocabulary.

* The discriminator network is provided with both, fake samples from the generator as well as samples from the true distribution. The samples from the true distribution are one hot encoded vectors.

--- 

### Experiments and Data

We train our model on all sentences in the treebank. The vocaburay size of the data set is $10000$ words. Models are trained using the back-propagation algorithm updating our   parameters using the Adam optimization method. A learning rate of $2x10^-3$ is used for generator and discriminator.

---

### Results

The model has learned to generate short phrases which are meaningful but the sentence as a whole does not make any sense. 

1. for nov call historical grades *gary anticipates* abortions *microsoft withdrawal compound compound nov expected has population expected other marketplace reversal* prosecutions transplants appear letters institute artist appetite appetite appetite soul <pad> directs <pad> analysis classical disrupted weakest contracted contracted salary dealt affordable urge spooked degrees million finnish bribery pcs heard wary expectations has formerly edge contributing carr facsimile helpful consist
n placed

2. Byale shed modify assessed challenging township husband degrees preferences <pad> urge carson claiming covering alleviate similarity conditional camp weakest russell plo detergent contributing gary anticipates acknowledging compound has totaled mixed race pickens deregulation turkey nov compound folks deregulation compound doors drinks broaden eliminated redeemed banned confused exchange ralph gently potentially altered re-election cooled dead leaped backdrop mass

The above two paragraphs address somewhat market related and political sentiments respectively. However there is more scope for imporovement.

---

### Conclusion

In conclusion, this work presents a straightforward  method to train GANs for Sentence Generation. In future work, we would like to explore techniques like proffesor forcing
so that the hidden state representations of fake and real samples could be as close as possible. This will let the generator generate sentences which are more more semantically meaningful.

