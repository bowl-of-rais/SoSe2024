# Goal format
from general-language dataset [@al-khatib-etal-2023-new]

`.tsv` file:

| Column                    | Format | Description                                                                                                                                     |
| ------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Input.Number              | number |                                                                                                                                                 |
| Input.Sentence            | str    | processed text (all lowercase, whitespaces around special characters, removal of some special characters (-), TODO: what else?)                 |
| Answer.relations          |        | relations in the sentence<br><br>relations are represented by dictionaries: `concept1` (cause), `relation`, `concept2` (effect), `relationtype` |
| Answer.if_single_relation | 0/1    | whether it's always the same concepts involved in the relations?                                                                                |
| Answer.if_positive_agg    | 0/1    | whether the aggregated positive score is > 0                                                                                                    |
| Answer.if_negative_agg    | 0/1    | whether the aggregated negative score is > 0                                                                                                    |
| Answer.relationtype       |        | for negative, positive: min/max number of people,                                                                                               |
#### Example

sentence: `the oceans , trees and plants were preventing any human produced co2`

relations:
```python
[
	 [
		 {
			 'concept1': 'the oceans , trees and plants', 
			 'relation': 'preventing', 
			 'concept2': 'human produced co2', 
			 'relationtype': 'negative'
		}
	], 
	[
		{
			'concept1': 'oceans, trees and plants', 
			'relation': 'preventing', 
			'concept2': 'human produced co2', 
			'relationtype': 'negative'
		}
	], 
	[
		{
			'concept1': 'oceans', 
			'relation': 'preventing', 
			'concept2': 'human produced co2', 
			'relationtype': 'negative'
		}, 
		{
			'concept1': 'trees', 
			'relation': 'preventing', 
			'concept2': 'human produced co2', 
			'relationtype': 'negative'
		}, 
		{
			'concept1': 'plants', 
			'relation': 'preventing', 
			'concept2': 'human produced co2', 
			'relationtype': 'negative'
		}
	]
]
```

if_single_relation: 0

if_positive_agg: 0

if_negative_agg: 1

relationtype:
```python
{
	 'positive': {
			  'min': 0, 
			  'max': 0, 
			  'agg': 0
			  }, 
	 'negative': {
			 'min': 1, 
			 'max': 3, 
			 'agg': 1
			 }
}
```

# BioCause format
[@mihuailua2013biocause]

#### Terms

```
T<ID>    <type> <start_offset> <end_offset>    <TEXT>
```

#### Events

```
sE<ID>    <TRIGGER>:T<ID> [<ARGUMENT>:T<ID>]
```

-> triggers are terms too

#### Other
- comments starting with `#` -> filter out
- equivalences, etc? with `*` -> filter out too (?)
- modifications, starting with `M` -> Negations and Speculations

## Parsing

- [rule-based matching in spacy](https://spacy.io/usage/rule-based-matching)
- what does the sentence tokenizer do exactly?




titles?
citations/references?
lists? i), ii), etc



-----

# Remarks

- biocause has 2 types of causal relations:
	- cause-effect
	- effect-evidence
	- ill keep them separate for now
- citations/references might be better to filter out?

# Stats

3223 sentences
451 sentences with causal relations
367 inter-sentence relations

-> bit few?

options:
- explore equivalences
- explore negative_regulations and positive_regulations
- explore modifications


----

# TO DO

- M-type annotations
- cleaning
- what is up with equivalences?
- what about Negative_regulation and Positive_regulation?
	- eg phrasings like `induced`, `activating`, `repressed`
	- also a kind of causal relation ()
- effect2 (1 instance)


- also what about citations?