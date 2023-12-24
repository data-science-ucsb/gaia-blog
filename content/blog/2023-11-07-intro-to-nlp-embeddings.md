+++
title = "Intro to NLP (notes on embeddings)"
date = 2023-11-07
description = "🌱"
+++

These are the notes I wrote for the [UCSB Data Science Club](https://datascienceucsb.org/) Intro to NLP workshop.
They are heavily adapted from Vicki Boykis's [Embeddings](https://raw.githubusercontent.com/veekaybee/what_are_embeddings/main/embeddings.pdf) book [^1] and other sources linked throughout this page.

This is more of a roadmap, less of a tutorial/lecture. It's intended to be a starting point for learning more about embeddings and how NLP works!

The target audience for this workshop was students with some background in ML and a good familiarity with probability and linear algebra.

## Embeddings

Say we have a set of all words (or parts of words) our model knows, called a vocabulary:

$$
V = \\{ \text{I}, \text{am}, \text{a}, \text{set}, \text{and}, \text{have}, \text{only}, \text{twelve}, \text{words}, \text{in}, \text{my}, \text{vocabulary} \\}
$$

**Motivation:** How do we represent words/phrases/sentences/etc. in computer-friendly format (i.e. numbers)? Convert it into a vector space!

### One Hot Encoding
If the size of our vocabulary is $|V|$, then we can represent each word as a $|V|$-dimensional vector with a 1 in one location and a 0 everywhere else. This is the one-"hot" approach.

$$
\vec{v_\text{twelve}} = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0
\end{bmatrix}^T
$$

Although it converts words/sentences into a computer friendly format, there are at least two big issues. Can you guess what they are?

1. Large vocabularies will result in unnecessarily *massive* and sparse vectors
2. Because the vectors are so sparse, we don't get any inherent notion of semantics or relationships between words (all words are orthogonal to each other in the vector space)

So the one-hot approach is *bad*. Let's try something else!

### TF-IDF
Stands for "Term Frequency, Inverse Document Frequency." This approach gives us an idea of how important a word is in any document, using a numerical weight.

**Term frequency** is calculated by finding the proportion of times the term (word/token) shows up in the document. If $t$ is the term, $d$ is the document, and $f$ is the frequency, then we calculate it with:

$$
\text{tf}(t, d) = \frac{f_{t, d}}{\sum_{t^\prime \in d} f_{t^\prime, d}}
$$

**Inverse document frequency** is calculated by dividing the total number of documents by the number of documents that contain the term $t$. We throw a logarithm in there, too. Here, $N$ is the number of documents and $D$ is the set of all documents. We add a $1$ on the numerator and denominator to avoid dividing by zero.

$$
\text{idf}(t, d) = \log \frac{N + 1}{|\\{ d \in D \vert t \in d \\}| + 1}$$

We can then multiply the two values together to get a single TF-IDF score:

$$
\text{tf-idf}(t, d, D) = \text{tf}(t, d) \cdot \text{idf}(t, d)
$$

We can find how "important" certain words are in any given document by seeing how high their TF-IDF scores are. Intuitively, a high TF-IDF score means that a word shows up in few documents, but in a particular document $d$ it shows up frequently. Thus, it is likely to carry important information.

Words like `a`, `an`, `the` are likely to be filtered out because they are likely to show up within many documents (even though they may show up often inside these documents). [^2]

**Creating Vectors:** How do we create a vector from this information? For each term, we can just create a $|D|$-dimensional vector, where the $d$th index is the TF-IDF score $\text{tf-idf}(t, d, D)$ for the word $t$ in document $d$.

TF-IDF only does an OK job at capturing the semantic meanings and relationships between words. With a lot of documents, even the TF-IDF vectors can be sparse. However, this approach does better than one hot encoding, and it also captures how important each word is! Luckily, we can do better.

### Word2Vec (CBOW & Skip-Gram)
Of course, people have thought of using neural networks to solve the embeddings problem. Word2Vec is a family of models that use does exactly this.

Word2Vec is an unsupervised classification algorithm. We want to find the probabilities of other surrounding words in the vocabulary. (Therefore, we use a softmax activation in the final layer and cross-entropy loss while training.)

Our goal is to perform **Maximum Likelihood Estimation**. We're training our Word2Vec model on a *corpus* of data. We want to choose our neural network's parameters such that the model maximizes the probability of related words showing up together.

Let's clarify what that means. There are two approaches, CBOW and Skip-Gram.
- (The below is also taken from Boykis's Embeddings book)
- ([Semantle](https://semantle.com/) is a fun Wordle spin-off based on Word2Vec)

#### Continuous Bag of Words (CBOW)
We have a context window of fixed size, and we want to find the probabilities of the word in the blank.

We can use a simple MLP-style architecture (a fully connected layer with a linear classifier on top), and we can just grab the vectors from the hidden layer. We "create one-hot encodings of each word to a numerical position, and each position back to a word, so that we can easily reference both our words and vectors. The goal is to be able to map back and forth when we do lookups and retrieval." (Boykis)

#### Skip-Gram
In [skip-gram](http://www.realworldnlpbook.com/blog/gentle-introduction-to-skipgram-word2vec-model-allennlp-ver.html), we predict all the words surrounding our input within the context window.

We assume that words that show up together have similar meanings ([distributional hypothesis](https://en.wikipedia.org/wiki/Distributional_semantics)). Our goal is to push these similar words (vectors) closer together in the vector space.

In our skip-gram implementation, we use a one-hot encoding as the input to our neural network [^3].

If you're interested in learning more, read about [negative sampling](http://www.realworldnlpbook.com/blog/gentle-introduction-to-skipgram-word2vec-model-allennlp-ver.html), which helps train more efficiently on larger vocabularies.

## Linear Algebra, A Detour
### Curse of Dimensionality, Reduction via PCA
When we have high-dimensional vectors in machine learning, we often want to reduce their dimensionality while still preserving as much important information as we can.

> Once we start generating a large number of features (columns), we start
running into the *curse of dimensionality*....
> 
> The more features we accumulate, the more data we need in order to accurately statistically confidently say anything about them, which results in models that may not accurately represent our data
> 
> (Boykis)

#### Principal Component Analysis
You might want to review [What is an eigenvalue/eigenvector?](https://textbooks.math.gatech.edu/ila/chap-eigenvalues.html) ([3Blue1Brown](https://www.youtube.com/watch?v=PFDu9oVAE-g))

1. Sort eigenvectors of covariance matrix by eigenvalue (large $\rightarrow$ small)
2. Pick top $k$ eigenvectors as principal components
3. Project vectors onto principal components

The idea here is that we're translating our data into a new basis determined by the eigenvectors of the covariance matrix. By doing so, we pick the directions that best capture the variance in the data.

**In-depth notes for the curious:**

The covariance matrix $\Sigma$ is a special type of matrix called a Hermitian matrix—meaning it's equal to its own (conjugate) transpose. In other words, $\text{cov}(X, Y) = \text{cov}(Y, X)$

A property of such matrices is that the eigenvectors form a basis for their domain/range. Thus we can write any vector in the domain of the covariance matrix as a linear combination of the eigenvectors. The eigenvectors will also be [orthogonal](https://math.stackexchange.com/questions/142645/are-all-eigenvectors-of-any-matrix-always-orthogonal) because the covariance matrix is Hermitian.

When we perform PCA, we are changing the basis of our data into a new basis defined by the eigenvectors of $\Sigma$.

> What’s special about this basis is it redefines your components to have zero linear dependence (in the variance of your data). That is, each eigenvector controls one and only one dimension of linear randomness. And the amount of randomness is given by your eigenvalue, which is the variance along that axis.
> 
> \- Daniel Naylor

### Embedding Similarity Metrics
Since we've embed our natural language into a vector space, we can now perform all sorts of vector math on them!

One of the most useful things to do is compute distances between various embeddings (which represent words/sentences/etc.).

There are all sorts of metrics between a pair of two-vectors $\vec{x}, \vec{y}$:

1. Manhattan Distance (L1): $\text{dist}(\vec{x}, \vec{y}) = |x_1 - y_1| + |x_2 - y_2|$
2. Euclidean Distance (L2): $\text{dist}(\vec{x}, \vec{y}) = ((x_1 - y_1)^2 + (x_2 - y_2)^2)^\frac{1}{2}$
3. Cosine Similarity: $\cos(\theta) = \frac{\vec{x} \cdot \vec{y}}{||\vec{x}||||\vec{y}||}$

We like to use cosine similarity in NLP to compare embeddings, because our vectors might be high-dimensional and have high Euclidean distances from each other. It's useful to call two vectors similar when they are close to parallel with each other.

NOTE: Cosine similarity is not a true distance metric (violates the triangle inequality), but it's still a useful metric.

### Vector Databases: The Power of Embeddings
Semantic search allows you to search for content based on its meaning, rather than just matching keywords or doing a regular-expression search.
- Example [Project](https://github.com/TanayB11/cosine) (shameless self-promotion)


## Language Models & Conditional Probability
> This section adapted from [Harvard CS197 Lecture Notes](https://www.cs197.seas.harvard.edu/)

Language models are just models that estimate a probability distribution of words, conditioned on a sequence of previous words (technically, tokens, which can be words but also other things like contractions or punctuation).

In this very simple example, our model outputs these probabilities:
$$P(\text{sleep} | \text{I'm really tired, I'm going to}) = 0.8$$
$$P(\text{study} | \text{I'm really tired, I'm going to}) = 0.2$$

We have a similar process for sequence-to-sequence generation. If we pick the most likely word from above:
$$P(\text{soon} | \text{I'm really tired, I'm going to sleep}) = 0.75$$
$$P(\text{today} | \text{I'm really tired, I'm going to sleep}) = 0.1$$
$$P(\text{tonight} | \text{I'm really tired, I'm going to sleep}) = 0.15$$

So then by simple probability rules,

$$
P(\text{I'm really tired, I'm going to sleep soon} | \text{I'm really tired, I'm going to}) = 0.8 \times 0.75 = 0.6
$$


This is the end of the notes for what I covered in the workshop—the next workshop is planned to be an introduction to transformers and fine-tuning a model with HuggingFace!

---

[^1]: https://raw.githubusercontent.com/veekaybee/what_are_embeddings/main/embeddings.pdf) 

[^2]: https://en.wikipedia.org/wiki/Tf-idf

[^3]: https://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/
