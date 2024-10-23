#causalRE 
# Paper: A survey on extraction of causal relations from natural language
[@yang2022surveycer]

>[!info] relationship extraction: extraction + classification of semantic relationships from text
>e.g. whole-party, product-producer, cause-effect

![[Pasted image 20240521152747.png]]

#### Terminology around #causality

**explicit causality**: relations connected by explicit links
- causal links (so, hence, etc)
- causative verbs (break, kill)
- resultative phrases
- conditional phrases
- casuative verbes and adjective

**implicit causality**: ambiguous connectives 

**intra-sentential causality**: cause and effect in single sentence
**inter-sentential causality**: cause and effect in different sentences

-> explicit and intra-sentential is easier, implicit and inter-sentential are more complex

## Benchmark datasets
#benchmarks
##### SemEval-2007 Task 4
[@girju-etal-2007-semeval]
[Website](https://sites.google.com/site/semeval2007task4/home)

- 7 relations, 140+70 samples for each, binary classification
##### SemEval-2010 Task 8
[@hendrickx-etal-2010-semeval]
[PapersWithCode](https://paperswithcode.com/dataset/semeval-2010-task-8)

- 9 relations, 10k samples, multi-class
##### PDTB 2.0 (Penn Discourse TreeBank)
[@prasad-etal-2008-penn]

- 72k non-causal, 9k causal samples
- AltLex corpus: implicit causality
##### TACRED
[PapersWithCode](https://paperswithcode.com/dataset/tacred)

- general RE dataset, only 269 `cause_of_death`
##### BioInfer

- 2.6k relations, 1.5k cause-effect

##### ADE

- entity extraction (drugs, diseases) + relations (drugs - adverse effects)
- 6.8k
## Evaluation
#evaluation

- precision, recall, F-score, accuracy
- [Matthews correlation coefficient](https://en.wikipedia.org/wiki/Phi_coefficient) (MCC) - for imbalanced datasets
- [geometric mean](https://en.wikipedia.org/wiki/Geometric_mean)/G-mean - balance between performances on classes
- application-specific
- causality confidence + topic purity (?)

## Causal relation extraction methods
#causalRE 
#### 1. Knowledge-based
-> either pattern-based or rule-based
##### Explicit Intra-sentential causality
- lexico-syntactic patterns
- domain-specific causal knowledge as linguistic cues
- Pundit algorithm/generalization rule (poor recall)
- WordNet-based: capture noun features
- pattern clustering
##### Implicit causality
- minimal supervision: 3 pre-defined types of implicit causality
- concept identification module + knowledge=based module for relation
##### Inter-sentential causality
- verbal linguistic CE patterns
	- sentence simplification, keywords -> expensive
#### 2. Statistical ML-based
-> third-party NLP tools
##### Explicit Intra-sentential causality
tool:
- decision trees
- Bayesian inference
- probabilistic topic model + time-series causal analysis

features:
- sentence-level: contextual, constituent/dependency parse
- production rules
- word pairs
- causal connectives
##### Implicit causality
- connectives from AltLex as seed data for new alternative lexicalizations in other corpors + SVM
- NB
- ME
##### Inter-sentential causality
- lexical pair probability
- ME classifier, data constructed via heuristics
#### 3. Deep learning-based approaches
-> mostly mentioned here: LSTMS, BERT, CNNs



# Paper: LLMs and Causal Inference in Collaboration: A Comprehensive Survey
[@liu2024largecausal]

#### 5.3 Causal Relationships Discovery
#causalRE 

- LLMs as query tool to determine edge direction
	- [44, 71, 4, 5]
- sensitivity to prompt design [61] -> incorporate special consistency properties
- LLMs + traditional
![[Pasted image 20240521164236.png]]

# Paper: Causality extraction: A comprehensive survey and new perspective
[@ali2023causalityextraction]

TODO


----

# Miscellaneous

PapersWithCode: no explicit subtask of causal relation extraction
