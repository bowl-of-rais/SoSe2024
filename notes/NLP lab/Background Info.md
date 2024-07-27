# Models
## BioBERT

[Paper](https://arxiv.org/abs/1901.08746)

used here: `BioBERT v1.2`
- initialized with BERT, pre-trained on PubMed abstracts (1M)
- same vocabulary as original bert-base

## BERT

[Paper](https://arxiv.org/pdf/1810.04805)

# Data
## General

[Preprocessing README](https://github.com/alenatz/BioCauseExtraction/tree/main/preprocessing)

- originally: [brat](https://brat.nlplab.org/examples.html) annotation format
- final format: `.json` with one sample per relation (sentences non-unique), spans + offsets (+ IOB tags)
- sentences can have multiple causal relations
- spans are not always continuous

[Examples](https://github.com/alenatz/BioCauseExtraction/blob/main/preprocessing/examples.md)

>[!int] What is a causal relation?

###### in BECAUSE:

>The new scheme distinguishes three types of causation, each of which has slightly different semantics: 
>**CONSEQUENCE**, in which the cause naturally leads to the effect; 
>**MOTIVATION**, in which some agent perceives the cause, and therefore consciously thinks, feels, or chooses something; and 
>**PURPOSE**, in which an agent chooses the effect because they desire to make the cause true

###### in BioCause:

>Yet, general terms of causality such as “cause” rarely appear in biomedical domain ontologies or other formalisations of the ways in which entities, processes and events are associated with each other. Instead, such formalisations frequently apply terms such as “regulation”, “stimulation” and “inhibition”. Whilst such terms also carry specific senses in biology, their definitions in domain ontologies and use in biocuration efforts show that, typically, their scope effectively encompasses any general causal association.

- **CAUSALITY**: either physical (reason + result) or within discourse (claim + justification)
- **(POSITIVE/NEGATIVE) REGULATION/NEGATIVE REGULATION**: “any process that has any effect on another biological process

## BioCause

[Paper](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-2)
[Website](https://www.nactem.ac.uk/biocause/)

## BECAUSE

[GitHub](https://github.com/duncanka/BECAUSE)
[Paper](https://www.cs.cmu.edu/~jdunietz/publications/because-v2.pdf)

## Rough preprocessing steps

#### 0. Original format

- `.txt` file: raw text, one sentence per line
- `.ann` file: annotations, terms/events identified via offsets in text
- additional arguments: term/event type, arguments+types

#### 1. For classification

- parse `.txt` +  `.ann` files
- filter events for causal relations
- identify intra-sentence causal relations using offsets
- final format: 
	- id, sentence, has_relations
	- list of relations: dictionary, cause/effect/relation spans + offsets within sentence
- exploded: one sample per relation instead of per sentence

#### 2. For span extraction

- goal: **IOB tags**
- tokenize sentence
	- `TreebankWordTokenizer` from NLTK: regex-based
	- also find offsets of tokens in sentence
- find tokens in cause/effect/relation spans
	- offsets make sure we get the right tokens