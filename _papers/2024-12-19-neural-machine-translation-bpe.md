---
layout: papers
title: "Neural Machine Translation with Byte-Pair Encoding"
authors: "Rico Sennrich, Barry Haddow, Alexandra Birch"
year: 2016
venue: "ACL 2016"
paper_url: "https://aclanthology.org/P16-1162/"
categories: [NLP, Machine Translation, Subword Segmentation]
date: 2024-12-19
---

## Abstract

This paper introduces Byte-Pair Encoding (BPE) as a simple data compression technique that can be effectively applied to neural machine translation. The approach addresses the challenge of handling rare and unknown words in neural translation models by learning subword representations automatically from training data.

## Problem Statement

### The Core Challenge

Neural Machine Translation (NMT) systems face a fundamental vocabulary problem:

1. **Out-of-Vocabulary (OOV) Words**: When the model encounters words not seen during training, it can't translate them
2. **Rare Words**: Even words in the vocabulary that appear infrequently are poorly translated
3. **Morphologically Rich Languages**: Languages with complex word forms (like German compounds) create massive vocabularies
4. **Fixed Vocabulary Size**: Models must choose between computational efficiency and vocabulary coverage

### Real-World Impact

- Google Translate struggling with technical terms
- Poor translation of proper nouns and domain-specific vocabulary
- Inability to handle new words that emerge after training

### Previous Approaches and Their Limitations

**Dictionary Lookup with UNK Tokens:**

Neural Machine Translation systems handled rare or unseen words using fixed vocabularies. Any word outside this vocabulary was replaced with a generic `[UNK]` token.

**How it worked:**
- During training, rare or unseen words were replaced with `[UNK]`
- At inference, if `[UNK]` appeared in the translation, the model looked up the source word in a precompiled bilingual dictionary and tried to substitute a likely translation

**Why it didn't work:**
- Requires external resources (bilingual dictionaries aren't always available or complete)
- Fails for morphologically rich languages, compounds, or creative words (e.g., "moonwalking")
- Breaks end-to-end training: Translation and word recovery are disconnected steps
- Can't adapt dynamically to context or domain-specific terms

## Main Idea

### The Breakthrough Insight

Instead of treating words as indivisible units, break them down into smaller, meaningful subword pieces that can be recombined to represent any word.

**Example:** 
```
"unparalleled" → ["un", "parallel", "ed"]
```

This enables the model to:
- Learn patterns at a finer level
- Generalize to unseen words (e.g., translate "toothbrush" even if only seen "tooth" and "brush")

### Key Innovation

Use **Byte-Pair Encoding (BPE)** - a simple data compression algorithm - to learn how to split words into subword units automatically from the training data.

## How BPE Works

BPE is a compression algorithm that iteratively merges the most frequent character pairs into subword units. You get a fixed-size subword vocabulary, but can compose any word from subwords. No UNK tokens are needed anymore.

### Complete BPE Algorithm

#### **Phase 1: Training BPE**

**Input:**
- Large corpus of text
- Target vocabulary size (e.g., 50K subwords)

**Step 1: Initialize Character-Level Vocabulary**
- Take every word in your corpus with its frequency count
- Split each word into individual characters
- Add a special end-of-word marker `</w>` to each word
- This gives you the initial vocabulary where every character is a separate token

**Step 2: Count All Adjacent Symbol Pairs**
- Look at every word in your vocabulary
- Count how many times each pair of adjacent symbols appears
- Create a frequency table of all symbol pairs

**Step 3: Find and Merge Most Frequent Pair**
- Identify the symbol pair that appears most frequently across all words
- Replace all occurrences of the most frequent pair with a single merged symbol
- Update your vocabulary with the merged version
- The merged symbol becomes a new subword unit

**Step 4: Record and Repeat**
- Save this merge operation in your merge list
- This list will be used later for encoding new text
- Go back to Step 2 and repeat the process
- Continue until you've learned the desired number of merge operations
- Typically, you'll perform 10,000-50,000 merge operations

#### **Phase 2: Encoding with BPE**

**Input:**
- New text to encode
- List of merge operations learned during training

**Step 1: Initialize with Characters**
- Take the input word
- Split it into individual characters
- Add the end-of-word marker `</w>`

**Step 2: Apply Merge Operations in Order**
- Take the first merge operation from your learned list
- Scan through the character sequence looking for that specific pair
- If found, merge those two symbols into one

**Step 3: Continue with Next Merge**
- Apply the second merge operation to the current sequence
- Keep going through all merge operations in the order they were learned

**Step 4: Stop When No More Merges Apply**
- Continue applying merge operations until no more pairs in your sequence match any remaining merge operations
- The final sequence is your BPE encoding

## Why BPE Works

### 1. Eliminates Out-of-Vocabulary (OOV) Problem
- Any word can be represented as a sequence of subwords
- If you can't find "unparalleled", you can use "un-parallel-ed"
- **Mathematical Guarantee**: With character-level fallback, coverage is 100%

### 2. Captures Linguistic Patterns
- Learns meaningful morphological units: prefixes, suffixes, roots
- "un-", "-ness", "-ing" become natural subword units
- Frequency-based learning captures language statistics

### 3. Balances Efficiency and Coverage
- Frequent words stay whole: "the", "and", "is"
- Rare words get broken down: "antidisestablishmentarianism"
- Optimal vocabulary size for computational efficiency

## Experimental Results

The paper demonstrates significant improvements:
- **English-German**: +1.1 BLEU over baseline
- **English-Russian**: +1.3 BLEU over baseline
- **Rare word translation**: 60% improvement in accuracy

## Mathematical Formulation

The BPE algorithm can be formalized as:

$$\text{BPE}(w) = \text{merge}(\text{split}(w))$$

Where:
- $$w$$ is the input word
- $$\text{split}(w)$$ creates character-level segmentation
- $$\text{merge}()$$ applies learned merge operations

## Implementation Example

Here's a simplified Python implementation:

```python
def get_stats(vocab):
    pairs = {}
    for word, freq in vocab.items():
        symbols = word.split()
        for i in range(len(symbols)-1):
            pair = (symbols[i], symbols[i+1])
            pairs[pair] = pairs.get(pair, 0) + freq
    return pairs

def merge_vocab(pair, vocab):
    bigram = ' '.join(pair)
    replacement = ''.join(pair)
    new_vocab = {}
    for word in vocab:
        new_word = word.replace(bigram, replacement)
        new_vocab[new_word] = vocab[word]
    return new_vocab
```

## Conclusion

BPE represents an elegant solution to a fundamental problem in neural machine translation. Its simplicity, effectiveness, and broad applicability have made it a cornerstone technique in modern NLP. The paper's impact extends far beyond machine translation, influencing the entire field of natural language processing.