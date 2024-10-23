[@feder2021causalm]

>[!idea] manipulate text representations to create counterfactual examples -> easier

- *treated concepts* vs *control concepts*
- pre-train **additional instance of language rep model** with **adversarial component**: forget concept of choice, keep control concepts
	- compare classifier performance with two representations
- explanation method: classifier training data + concept of interest -> causal effect of concept on classifier in test set 

---
# 2. Causality and Interpretability

## 2.1 Causal Inference and NLP

- **causal inference**: crucial in social sciences
	- requires knowledge/explicit assumptions re data generation
	- using CI frameworks in NLP challenging (high-dim)

- previous attempts:
	- measure other confounders via text, use text as confounder
	- causally driven representation learning (done when treatment affects text)
	- new BERT objective to create low-dim text embeddings -> variables for answering causal questions (veitch, sridhar, blei)
	- Vig et al 2020: manual queries/conterfactuals to understand how info flows through model

>[!int] here: counterfactual analysis as method for interpreting DNN-based models, generalized method for any textual concept, no manual creation
## 2.2 Model Interpretations and Debiasing in NLP

**model interpretability**: degree to which human can consistently predict model's outcome
- local explanation: feature values *of some instance* -> model prediction
- global explanation: feature -> model prediction

**debiasing**: creating models/language representations unaffected by unwanted biases
- manipulation of training data, altered training processes, changes in model itself
- correlations!!

---
# 3. Causal Model Explanation
Estimating Causal effects from observational data using language representations

>[!info] causal model explanation: estimate causal effect of given variable (=treatment) on model's predictions -> formalized as [causal inference](https://en.wikipedia.org/wiki/Causal_inference) problem
## 3.1 Causal Inference and Language Representations

![[Pasted image 20240422143325.png|causal graphs: original data generation, direct manipulation, manipulation of representations|500]]
##### Confounding Factors

formalization:
- set of concepts -> binary variables $C_{i} \in \{0,1\}$
- pre-trained lang rep model $\phi$
- trained classifier $f : \phi(X) \to l \in L$
- probability of output $z_{l}$, vector of all: $\vec{z}(f(\phi(X)))$

naive approach: difference between averaged $\vec{z}$, but: concepts may be confounded
##### Do-operator

$do(C_{i})$: external intervention, change $C_{i}$
- conditioning on $C_{i}$: passive observation setup, probability that ... given concept is present (example: if positive adjectives are present)
- conditioning on $C_{adj}$: probability that ... after all info about concept has been removed that previously contained it 
##### Counterfactual Text Representations

intervene on concept(s) of interest *only*
- isolation is non-trivial depending on nature of concept (e.g. political figure likely affects other important concepts generating the text)

>[!idea] assume concepts generate representations $\phi(X)$ directly

so, generate $\phi^{C}(X)$ **unaffected by existence of chosen concept**, aka $$\vec{z}(f(\phi^{C}(X))) = \vec{z}(f(\phi^{C}(X')))$$where $X, X'$ must be equal for all generating concepts except $C$
## 3.2 Textual Representation-based Average Treatment Effect (TReATE)

##### Average Treatment Effect (ATE)
$$ATE_{T}= \mathbb{E}[Y \mid do(T=1)] - \mathbb{E}[Y \mid do(T=0)]$$

**structural causal model** for a document $X$:
- $(C_{0}, ..., C_{k}) = h(\epsilon_C)$ %%generating process for concepts%%
- $X = g(C_{0}, ..., C_{k}, \epsilon_X)$
- $C_{j} \in \{0, 1\} \forall j \in K$
where $\epsilon_C$ and $\epsilon_X$ are independent variables, $g$ is a generative process
##### Causal Concept Effect (CaCE)
$$CaCE_{C_{j}} = \mid\mid \mathbb{E}_{g}[\vec{z}(f(\phi(X))) \mid do(C_{j} = 1)] - \mathbb{E}_{g}[\vec{z}(f(\phi(X))) \mid do(C_{j} = 0] \mid\mid_{1}$$
aims to measure change, but we want to measure existence -> alternative data-generating process $g^{C_{0}}$ unaffected by $C_{0}$
- $(C_{0}, ..., C_{k}) = h(\epsilon_C)$ %%generating process for concepts%%
- $X' = g^{C_{0}}(C_{1}, ..., C_{k}, \epsilon_X)$
- $C_{j} \in \{0, 1\} \forall j \in K$
##### Example-based Average Treatment Effect

>[!imp] causal effect of concept $C_{j}$ on class probability distribution $\vec{z}$ of classifier $f$ under generative processes $g, g^{C_{0}}$

$$EATE_{C_{j}} = \mid\mid \mathbb{E}_{g^{C_{j}}}[\vec{z}(f(\phi(X')))] - \mathbb{E}_{g}[\vec{z}(f(\phi(X)))] \mid\mid_{1}$$
requires counterfactual example generation
##### Textual Representation-based Average Treatment Effect

>[!imp] causal effect of concept $C_{j}$, controlling for (potentially confounding) concept $C_{m}$ on class probability distribution $\vec{z}$ of classifier $f$ under generative process $g$

$$TReATE_{C_{j}, C_{m}} = \mid\mid \mathbb{E}_{g^{C_{j}}}[\vec{z}(f(\phi(X')))] - \mathbb{E}_{g}[\vec{z}(f(\phi(X)))] \mid\mid_{1}$$

## 3.3 Representation-Based Counterfactual Generation 

##### Generating Synthetic Examples

- manual augmentation: costly and time-consuming
- automatic generating w/ generative models: still too hard

##### Interventions on Language Representation Models

>[!idea] add auxiliary tasks during fine-tuning: forget some concepts, remember others

**Treated Concept** objective:
- "forget" concept
- IsMaskedAdjective (IMA) task, similar to MLM task
- DANN/gradient reversal (ganin 2016): discriminate whether treated concept is present 
- modify loss: negative term for target concept, positive term for controls

**Controlled Concept** objective:
- classification, e.g. political orientation of figures

then: calculate *individual treatment effect* (ITE) and estimated TReATE as sum over all test samples

---
# 4. Novel data sets

## 4.1 Product and Movie Reviews

>[!int] sentiment classification data sets, many adjectives and many samples (to create correlations), also: variation in topics to estimate causal effect of topics

combine 2 datasets from 5 domains: books/dvd/electronics/kitchen appliances (1000/1000 each) + movies (sample 1000/1000)

adjectives
- POS tags, generate counterfactuals w/o adjectives
- variable? adjective : non-adjective ratio

topic: latent dirichlet allocation topic model -> 50 topics
- *treatment concept* topic: most associated with domain
- *control concept* topic: second-most associated
- binary variable: topic probability above/below median
- can't generate realistic counterfactuals

## 4.2 Enriched Equity Evaluation Corpus

>[!int] gender and racial bias, control s.t. text only differs by gender or race of person it discusses, control data generating process (to create bias)

**Equity Evaluation Corpus**: 8k english sentences
- labeled for mood state (Profile Of Mood States)
- sentence templates: placeholders for name and emotion, filled from list of names (tagged M/F, African American/European) and 4 lists emotions (anger/sadness/fear/joy label -> a bunch of words for each)

but: very short, aka not very useful for training -> **enrich**
- add prefix/suffix phrases (e.g. related place, family member, time, etc)
- concatenate with 13 non-informative sentences, correlation between label and 3 sentences
- add ambiguous emotion words

counterfactuals: replace name + pronouns

---
# 5. Tasks and Experiments
Modifying a BERT-based representation model

**RQ1**: approximation of ATE using TReATE?
**RQ2**: does BERT_CF forget treated concept?
**RQ3**: does BERT-CF remember control concept?
**RQ4**: can BERT-CF used to mitigate bias?

>[!imp] manually introducing bias: 3 versions of data: **balanced/gentle/aggressive** - different correlations between treated concept and label
## 5.1 Causal effect of adjectives on sentiment classification

treated concept: adjectives
control concepts: other PoS tags

balanced data: all news
gentle data: delete top 50% of sentences in negative reviews sorted by adjective ratio
aggressive data: gentle data + delete bottom 50% of sentences in positive reviews

TC objective: Is Masked Adjective (mask non-adjectives too in equal number)
CC objective: classify PoS of non-adjectives

## 5.2 Causal effect of topics on sentiment classification

>[!int] topics can have effect on probabilities (e.f. movie genre -> more negative reviews)

test TReATE performance using domain variation in reviews data set + correspondence of topics to domains

create balanced/gentle/aggressive data similarly to before

TC objective: Is Treated Topic
CC objective: Is Control Topic

## 5.3 Causal effect of gender and racial bias

control correlation:
balanced data: randomly choose name
gentle: 90% of joy are female, 50% of anger/sadness/fear are male
aggressive: 90% of joy female, 10% of anger/sadness/fear
analogously for race

TC objective: classify gender
CC objective: classify race
(if TC gender, CC race)

## 5.4 Comparing causal estimates to ground truth

##### Correlation-based baselines

>[!idea] simple diff between examples with concept vs without concept

- CONEXP (goyal 2019a): conditional expectations of prediction scores, based on passive observations
- TPR-GAP (de-arteaga 2019): diff between fraction of correct predictions with vs without concept
	- compares accuracy -> not directly comparable to TReATE
##### Language Representations

- BERT-O: pre-trained BERT
- BERT-MLM: fine-tuned on data
- BERT-CF: additional training objectives

##### Measures

- ground-truth causal effect: $ATE_{gt}(O)$
- $TReATE(O, CF)$
- also: $TReATE(O, MLM)$

---
# 6. Result
Comparison: estimated causal effect vs ground truth
## 6.1 Estimating TReATE (Causal Effect)

- TReATE(O, CF) and $ATE_{gt}(O)$ very similar across experiments, regardless of bias introduced
- can also successfully detect bias even if true effect is close to 0
- baseline CONEXP underestimates effects of concepts
##### TReATE without ground truth

sanity checks
- causal effect can be estimated for topics too
- effect of topic is highest on most correlated domain, and has higher effect on more similar domains
- not substantially changed by adding control for additional confounding topic
## 6.2 Analyzing counterfactual model

##### Detecting treated concepts

compare accuracy of BERT-O, BERT-MLM, BERT-CF
- treated representations have much weaker performance
- this is only a sanity check
##### Detecting control concepts

- using treated representations does not hurt performance

##### Mitigating Bias

>[!idea] test models trained on aggressive data on balanced test set -> test generalization

- BERT-CF-based models can generalize better when correlations don't exist
- performs well when correlation between treated concept and label changes

## 6.3 Analyzing stage 2 multitask training scheme

- TC tasks converge after 1-2 epochs
- adding CC increases loss at first, but then also converges quickly
- 
- MLM losses have similar behaviors -> adding tasks does not drastically harm BERT training/resulting reps

---
# 7. Discussion

##### How to estimate impact of a given concept on a DNN

1. design world model: causal graph
	- concepts that generate text and relations between them
	- explicitly state assumptions on world and data
2. choose control concepts
	- sanity checks: did we really control for this?

##### Limitations

- concepts can be switched on/off, but irl, "deleting" a concept is not always feasible
- global vs local concepts, global can be hard to model
- validating quality ofcausal explanation method