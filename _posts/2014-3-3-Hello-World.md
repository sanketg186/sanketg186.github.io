---
layout: post
title: Transformers
---
## Introduction
In recent years, transformers have emerged as a powerful architecture in the field of machine learning, revolutionizing natural language processing (NLP), computer vision, and other domains. With their ability to capture long-range dependencies and context, transformers have become the backbone of many state-of-the-art models. In this blog, we'll explore what transformers are, how they work, and their applications in machine learning.

## What are transformers?
Transformers are a type of deep learning model architecture introduced in the seminal paper "Attention is All You Need" by Vaswani et al. in 2017. Unlike traditional sequence models such as recurrent neural networks (RNNs) and long short-term memory networks (LSTMs), transformers relying on attention mechanisms to draw global dependencies between input and output data.

## Model Architecture 
In the paper, Transformers are a Encoder-Decoder Network of 6 stacked layers. There is a 6-layer encoder stack on the left and a 6-layer decoder stack on the right.

<img width="700" height="700" alt="image" src="https://github.com/sanketg186/sanketg186.github.io/assets/17880735/78c6778b-3dcf-46a6-94e1-c10b0cc1f9be">

In Encoder, each layer has two sub layers: 1. multi headed self attention mechanism 2. Fully connected feed forward network.
In Decoder, each layer has three sub layers: 1. multi headed self attention mechanism 2. Fully connected feed forward network 3. Multi headed attention (over Encoder output).

### Attention
Attention mechanism is a key component of transformer models, enabling them to capture dependencies between different parts of the input sequence more effectively than traditional recurrent or convolutional architectures.
In the context of transformers, attention can be broadly categorized into self-attention and multi-headed attention.
### Self Attention
Self-attention, also known as intra-attention, allows the model to weigh the importance of different words in a sequence with respect to each other. It computes attention scores between all pairs of words in the input sequence and uses these scores to create weighted representations of each word. The attention scores are calculated based on the similarity between the embeddings of the words.

Example:

Consider the sentence: "The cat sat on the mat." In self-attention, each word's embedding is compared to the embeddings of all other words in the sentence. For instance, when calculating the representation of the word "cat," the model assigns higher weights to words like "the" and "sat," which are closely related to "cat" in the context of the sentence, while assigning lower weights to less relevant words like "on" and "mat."

### Multi Headed Attention
Multi-headed attention extends the concept of self-attention by allowing the model to focus on different aspects of the input sequence simultaneously. It achieves this by computing multiple sets of attention weights, each corresponding to a different "head" or subspace of the embedding space. These attention weights are then concatenated and linearly transformed to produce the final output.
The vectors responsible for tokens are broken up into multiple parts called heads (8 in the original paper) which go through the same attention computing process as before.The results of the process are concatenated together to form an output of the same type.
This process can be parallelized, which enables training the model faster. It also allows the model to learn the context of the words better.

Example:

Continuing with the example sentence, in multi-headed attention, the model may learn to attend to different aspects of the sentence concurrently. For instance, one head might focus on the syntactic structure of the sentence, while another head might focus on the semantic relationships between words. By incorporating multiple heads, the model can capture a richer and more diverse set of dependencies within the input sequence.

## Deeper into the Transformer

### Encoder: 
The encoder layer structure in the transformer model remains consistent across all six layers. Each layer comprises two primary sublayers: a multi-headed attention mechanism and a fully connected position-wise feedforward network, depicted in the accompanying figure.

<img width="366" alt="image" src="https://github.com/sanketg186/sanketg186.github.io/assets/17880735/b9874f2a-f0c4-4ba9-8a23-4f021fad5a7c">

A crucial aspect of the transformer model is the inclusion of residual connections around each main sublayer, ensuring that the original input x to a sublayer is preserved and passed through a layer normalization function. This preservation of input ensures that essential information, such as positional encoding, is retained throughout the layer processing.Consequently, the normalized output of each layer is given by:
```math 
LayerNorm(x + Sublayer(x))
```
Sublayer(x) is the function implemented by the sub-layer itself.
To facilitate these residual connections, all sub-layers in the model, as well as the embedding layers, produce outputs of dimension dmodel = 512

Although the structural composition of each layer within the six-layer encoder is identical, the content within each layer may differ. For instance, the embedding sublayer is exclusively present in the initial layer of the stack. The subsequent five layers lack an embedding layer, ensuring stability in the encoded input across all layers.

Additionally, while the multi-head attention mechanisms in layers 1 through 6 serve the same overarching function, they are not tasked with identical operations. Instead, each layer builds upon the knowledge gleaned from its predecessor, exploring distinct methods of associating tokens within the sequence.

### Input Embedding
In a transformer model, the input embedding layer plays a fundamental role in converting discrete tokenized inputs, such as words or subwords, into continuous-valued embeddings.
The input embedding sublayer within the transformer model converts input tokens into vectors of dimension.

The input embedding sublayer within the transformer model converts input tokens into vectors of dimension $` d_{model} `$ using learned embeddings. In case of original transformaer $` d_{model} = 512 `$.

Tokenization: A tokenizer is a crucial component in natural language processing (NLP) pipelines responsible for breaking down raw text into smaller, meaningful units called tokens. These tokens could be words, subwords, or characters, depending on the granularity of tokenization required for the task at hand. 

Function of Tokenizer:

Segmentation: Tokenizers segment input text into individual tokens based on predefined rules or patterns. This segmentation process ensures that the text is broken down into manageable units for further processing.

Normalization: Tokenizers may also perform text normalization tasks, such as lowercasing, removing punctuation, or handling special characters, to ensure consistency and uniformity in the tokenized output.

Vocabulary Generation: In the context of machine learning models, tokenizers often generate a vocabulary or dictionary mapping each unique token to a numerical identifier. This v

Example:
Consider the sentence: "The quick brown fox jumps over the lazy dog."

Word Tokenization:

Input: "The quick brown fox jumps over the lazy dog."

Output Tokens: ["The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog."]
tokenized text(integer mapping) = [1996, 4937, 7771, 2006, 1996, 6411, 1012, 2009, 2001]

Subword Tokenization (e.g., Byte-Pair Encoding - BPE):

Input: "The quick brown fox jumps over the lazy dog."

Output Tokens: ["The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog", "."]
tokenized text(integer mapping) = [1996, 4937, 7771, 2006, 1996, 6411, 1012, 2009, 2001, "3435"]

Character Tokenization:

Input: "The quick brown fox jumps over the lazy dog."

Output Tokens: ["T", "h", "e", " ", "q", "u", "i", "c", "k", " ", "b", "r", "o", "w", "n", " ", "f", "o", "x", " ", "j", "u", "m", "p", "s", " ", "o", "v", "e", "r", " ", "t", "h", "e", " ", "l", "a", "z", "y", " ", "d", "o", "g", "."]

tokenized text(integer mapping) = [2, 4, 5, 1, 5, 6, 25, 56,......]
The transformer initially used Byte-Pair Encoding (BPE) tokenization, but other models use other methods.

Embeddings: These tokens are converted to embeddings. These embeddings are low dimensional vectors that capture semantic and syntactic information about the entities they represent.

Positional Encoding: As our model lacks recurrence and convolution, preserving the sequence's order requires injecting information about the relative or absolute position of tokens. Thus, we introduce "positional encodings" into the input embeddings at the base of both the encoder and decoder stacks. These encodings share the same dimension $`d_{model} `$ as the embeddings, allowing for their summation.

In their work, they used sine and cosine functions of different frequencies:
``` math
PE_{(pos,2i)} = \sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})
```
``` math
PE_{(pos,2i+1)} = \sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})
```
### Multi-Headed Attention
Multi-head attention is a mechanism used in transformer models to enhance the model's ability to focus on different parts of the input sequence simultaneously. It allows the model to jointly attend to information from different representation subspaces at different positions, enabling more expressive and effective attention computations.

### Layer Normalization
Layer Normalization includes both an addition operation and a layer normalization process. The addition operation handles residual connections originating from the input of the sublayer. The primary objective of these residual connections is to prevent the loss of essential information. 
```math 
LayerNorm(x + Sublayer(x))
```
### Feed-Forward Network(FFN)
The feed forward network is a fully connected layer, input of FFN is the output of previous normalization layer. The output of FFN again goes to normalization layer, as shown in the figure.

### Decoder:
Each Layer of decoder looks like this:

<img width="348" alt="image" src="https://github.com/sanketg186/sanketg186.github.io/assets/17880735/a1179902-9853-4b49-88f3-f4ab4a47117f">

The architecture of the decoder layer mirrors that of the encoder across all N=6 layers in the transformer model. Each layer comprises three sublayers: a multi-headed masked attention mechanism, a multi-headed attention mechanism, and a fully connected position-wise feedforward network.

Additionally, the decoder introduces a third primary sublayer, namely the masked multi-head attention mechanism. In the output of this sublayer, words beyond a certain position are masked, ensuring that the transformer generates predictions based solely on preceding information and without access to subsequent parts of the sequence. This approach safeguards against the model inadvertently accessing future context during inference.

## References
<a id="1">[1]</a> 
[Attention Is All You Need](https://arxiv.org/abs/1706.03762)
