
- [ ] need spans from original dataset
- [x] stratify split! ✅ 2024-06-26


-------


https://github.com/tamuhey/textspan?tab=readme-ov-file

https://www.lighttag.io/blog/sequence-labeling-with-transformers/

----

# Word tokenization

## Desiderata

- split at characters while keeping them as tokens: `/`, `-`
- split contractions: `"I'll"` -> `["I", "'ll"]`
- split at whitespaces
- periods are only separated if they are final



## Current Behavior

using TreebankWordTokenizer():
dash not split around in 
```
sentence: The lack of a canonical sigmaE sequence at these promoters suggests that another regulatory gene may be epistatic to sigmaE or that these promoters encode functional, but non-canonical sigmaE-binding sites due to their horizontal acquisition and gradual integration into the sigmaE regulatory network.

tokens: ['The', 'lack', 'of', 'a', 'canonical', 'sigmaE', 'sequence', 'at', 'these', 'promoters', 'suggests', 'that', 'another', 'regulatory', 'gene', 'may', 'be', 'epistatic', 'to', 'sigmaE', 'or', 'that', 'these', 'promoters', 'encode', 'functional', ',', 'but', 'non', '-', 'canonical', 'sigmaE', '-', 'binding', 'sites', 'due', 'to', 'their', 'horizontal', 'acquisition', 'and', 'gradual', 'integration', 'into', 'the', 'sigmaE', 'regulatory', 'network', '.']

cause: ['The', 'lack', 'of', 'a', 'canonical', 'sigmaE', 'sequence', 'at', 'these', 'promoters']
effect: ['that', 'these', 'promoters', 'encode', 'functional', ',', 'but', 'non-canonical', 'sigmaE-binding', 'sites']
relation: ['suggests', 'that']
```

# Misc

```
cause range: 0, 80 
effect range: 81, 204 
relation range: 81, 81
```

right now: skip relations



PMC2565068-00-TIAB had a typo
- `invaive` instead of `invasive`
- manually corrected all positions > 950


# BioCause specialties

things  that should maybe not be separated??
- BALB/c
- 


---

# Archive


```python
def label_samples2(data: Dataset) -> Dataset:

"""Adds BIO tags for the causal span using the spacy tokenizer."""

  

# find spans first

data = data.map(find_cause, num_proc=4)

  

# then tokenize

nlp = English()

tokenizer = nlp.tokenizer

  

# create Span objects with labels

doc: Doc

for i, doc in enumerate(tokenizer.pipe(data['sentence'], batch_size = 50)):

print("doc.text:", doc.text)

  

start = data[i]['cause_start']

b_end = data[i]['cause_firstword_end']

i_end = data[i]['cause_end']

  

causal_first = doc.char_span(start, b_end, label=Tags.B_cause, alignment_mode='expand')

causal_rest = doc.char_span(b_end + 1, i_end, label=Tags.I_cause, alignment_mode='expand')

  

print("causal_first.text: ", causal_first.text)

  

doc.set_ents([causal_first, causal_rest])

  
  

return data
```


```python

def label_samples(data: Dataset) -> Dataset:

"""Adds BIO tags for the causal span using the spacy tokenizer."""

  

# find causal spans first

data = data.map(find_spans, num_proc=4)

#print(data[0])

  

print("found causal spans")

  

# then tokenize, remembering the offsets

data = data.map(split_with_offsets)

print("tokenized with offsets")

  

# then add BIO tags

data = data.map(tag_single_sample, num_proc=4)

print("added BIO tags")

  

return data
```

```python
if token_start <= c_start <= token_end:

tags.append(Tags.B_cause)

  

elif c_start < token_start and c_end >= token_end:

tags.append(Tags.I_cause)

  

if token_start <= e_start <= token_end:

tags.append(Tags.B_effect)

  

elif e_start < token_start and e_end >= token_end:

tags.append(Tags.I_effect)

  

if token_start <= r_start <= token_end:

tags.append(Tags.B_relation)

  

elif r_start < token_start and r_end >= token_end:

tags.append(Tags.I_relation)

  

else:

tags.append(Tags.O)
```