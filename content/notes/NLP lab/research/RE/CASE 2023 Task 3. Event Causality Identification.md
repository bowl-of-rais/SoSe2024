links:
[Git](https://github.com/tanfiona/CausalNewsCorpus)
[Proceedings of CASE 2023](https://aclanthology.org/volumes/2023.case-1/)
[PapersWithCode task](https://paperswithcode.com/task/event-causality-identification)

task description:
>Causality is a core cognitive concept and appears in many natural language processing (NLP) works that aim to tackle inference and understanding. We are interested in studying event causality in the news and, therefore, introduce the Causal News Corpus. The Causal News Corpus consists of 3,767 **event sentences extracted from protest event news, that have been annotated with sequence labels on whether it contains causal relations or not**. Subsequently, causal sentences are also annotated with **Cause, Effect and Signal spans**. Our subtasks work on the **Causal News Corpus**, and we hope that accurate, automated solutions may be proposed for the detection and extraction of causal events in news.

>[!idea] approaches might fit with LogicClimate dataset? 

### Subtasks

1. Causal Event Classification: binary classification
2. Cause-Effect-Signal Span Detection: identify all spans for all causal relations in a sentence

### Dataset: Causal News Corpus
[@tan-etal-2022-causal]

- 3.5k English sentences
- binary labeling: `causal` vs `non-causal`

### Baselines

- dummy: all examples predicted as causal
- NN: BERT and LSTM

### Overview over approaches
[@tan-etal-2023-event-causality]

- all BERT-based models
- mostly sequence classification
- alternative: sequence tagging

---

# Derived Research
[ConnectedPapers](https://www.connectedpapers.com/main/220b915c3f86925c7dc05a59e613f53e485ad87d/Event-Causality-Identification-with-Causal-News-Corpus-%20-Shared-Task-3%2C-CASE-2022/derivative)

### Zero-shot cross-lingual document-level event causality identification with heterogeneous graph contrastive transfer learning
[@he2024zeroshot]

- graph interaction network models long-distance dependencies between events
- transfer learning module to transfer between languages

### Identify event causality with knowledge and analogy
[@wu2023eventcausalityknowledge]

- external knowledge: commonsense knowledge graph (ConceptNet) -> enrich event description, reveal commonalities/associations between events
- internal analogies: retrieve similar events as analogy examples

![[Pasted image 20240521190756.png]]

### Prompt-based event relation identification with constrained prefix ATTention mechanism
[@ZHANG2023111072]

![[Pasted image 20240521191048.png]]
"traditional prompt-based method": convert problem into MLM task

![[Pasted image 20240521191145.png]]
- templates: `X is the cause of Y`, `X has no relation to Y`
- prefix-biased fully-connected attention:
![[Pasted image 20240521191711.png]]

### PPAT: Progressive graph pairwise attention network for event causality identification
[@liu2023ppat]
[Git](https://github.com/HITsz-TMG/PPAT)

- sentence boundary event relational graph:
	- nodes - event pairs, edge - event shared by nodes
	- intra-sentence nodes only connect with eachother
	- ![[Pasted image 20240521193502.png]]
- PPAT: attend to different reasoning chains on the graph
- causality-guided training strategy: additional loss
- datasets: EventStoryLine, MAVEN-ERE, Causal-TimeBank
- progressive reasoning strategy: 
	1. predict intra-sentence causality with local context
	2. reason inter-sentence causality

![[Pasted image 20240521193600.png]]

