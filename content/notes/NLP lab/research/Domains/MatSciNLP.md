[@song2023matscinlp]

>[!info] contributions
>- MatSci-NLP benchmark + analysis of different BERT-based models
>- text-to-schema multitasking

----
# 2. Background

- in general: many domain-specific LMs and benchmarks for scientific language already exist
- challenge: data from journals/scientific documents
	- often restricted or in pdf format
	- advanced data mining required to process niche scientific texts (kuniyoshi, wang2022b)

# 3. MatSci-NLP Benchmark

>[!idea] unify various smaller datasets to create benchmark
##### Data Format
- JSON-based
- text, task definitions, annotations
- can be refactored into input schemas
##### Tasks
1. NER
2. Relation Classification
3. Event Argument Extraction
4. Paragraph Classification
5. Synthesis Action Retrieval (domain-specific)
6. Sentence Classification
7. Slot Filling (domain specific)

>[!int] why these tasks?
>- conventional NLP tasks: better process/understand textual data
>- domain-specific tasks: solve challenges in the domain

# 4. Unified Text-to-Schema Language Modeling

![[Pasted image 20240424213101.png|overview: text-to-schema method, including answer schemata|300]]

- modular architecture, can exchange for domain-specific models
- fine-tuning on collected tasks (4.3)
- more structure -> can better handle tasks at different levels (token level vs higher) vs unstructured seq2seq
- simplifies complex tasks: structure as context
##### Text-to-Schema Modeling

>[!int] inspired by question answering

4 general components:
1. **text**: raw text, input to LM
2. **description**: describes task according to schema
3. **instruction options**: answer choices/example of input output pair/pre-defined answer schema
##### Language Decoding & Evaluation

1. filter answers through schema
2. match prediction with most similar valid class given by annotation for particular tasks -> reformulate task as classification problem
3. cross-entropy loss
# 5. Evaluation/Results

- low-resource setting: 1% training, 99% test -> check knowledge acquired in pre-training
##### Models used as encoder

- **MatSciBERT**: materials science
- **MatBERT**: materials science
- **BatteryBERT**: just batteries
- **SciBERT**: general scientific articles
- **ScholarBERT**: general scientific articles
- **BioBERT**: biomedical corpora
- **BERT**: vanilla
### 5.1 Effectiveness of domain-specific language models as encoders

![[Pasted image 20240424215724.png|top: microF1, bottom: macroF1]]
##### Effect of domain-specific pre-training

- MatBERT performs best: specific pretraining helps
	- MatSciBERT underperforming: curation is relevant too
- SciBERT vs ScholarBERT
- most outperform BERT trained on general language, *pretraining on high-quality scientific text is beneficial*
	- note BioBERT: high performance even though different sub-domain
##### Effect of dataset balance

- micro-F1 much higher than macro-F1 -> consistent imbalance
- "semantically meaningful understanding of task": microF1 > 0.66, macroF1 > 0.5

to alleviate: weighted loss functions (e.g. [Focal loss](https://paperswithcode.com/method/focal-loss)), class-balanced samplers or architecture changes (e.g. separate prediction heads for minorities or L2 reg/dropout)
### 5.2 In-context data schema and multi-tasking in low-resource training settings

4 different schemas:
1. no explanations: only task description
2. potential choices: include class labels
3. examples: include example
4. task schema: model receives proposed schema

compared with:
1. single task fine-tuning
2. single task prompt (format task but train each separately)
3. MMOE (multiple encoders/embeddings + task-specific gate)

![[Pasted image 20240424220559.png| top: microF1, bottom: macroF1]]

- text-to-schema have better performance across models
- BERT: even outperforms domain-specific models in some settings (except Task-Schema)

# Limitations

- low quantity of data
- small datasets
- generalization beyond material science
- only BERT-based models (availability of pre-trained models)