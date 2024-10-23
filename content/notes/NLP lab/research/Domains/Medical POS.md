[@coden2005domain]

-----

>[!info] quantify differences between general and specialized (medical) English in terms of PoS

- adapt general-purpose POS tagger
	- manual annotation of docs from target domain + add to training data
	- or use lexicon derived from target domain

---

# 2. Related Work

---

# 3. Problem description

medical subdomains:
1. clinical notes from physicians
2. biomedical literature abstracts (PubMed)

>[!idea] look at characteristics typically used by statistical POS taggers

experiments: 1-/2-/3-gram models, uni-gram also with simple smoothing (for out-of-vocabulary words)
## 3.1 Corpora

1. **Penn Treebank-2** (subset)
	- general purpose
	- manual POS tags
1. **GENIA**
	- 2000 abstracts
	- keywords Human/Blood Cells/Transcription Factors
	- also manual POS tags (diff guidelines)
2. **MED**
	- 16M Clinical Notes (HL7)
	- speech to text -> speech phenomena
## 3.2 Similarities and Differences

##### Words and Word Types

|            | TB-2 | MED  | GENIA |
| ---------- | ---- | ---- | ----- |
| words      | 1.3M | 100k | 500k  |
| word types | 45k  | 9k   | 22k   |
more specific language -> probably less stop words etc, thus larger relative number of word types
##### Number Normalization
>[!idea] words differing in digits only have the same tag
- GENIA: many words differ only in digits
##### Vocabulary sizes
- GENIA < MED < TB-2
##### Average sentence length
- especially short in MED corpus -> sentence fragments, also shorter sentences are a speech phenomenon

>[!info] shorter sentences: challenge for POS tagging
>context important for words
##### Distribution of tag groups
- GENIA: higher percentage of nouns and adjectives
##### Transition statistics
- TB-2: more det-noun transitions, but similar fractions of words classified as determiners across all
- MED, GENIA: more JJ-NN and NN-NN transitions

----
# 4. Adaptation Study

>[!info] performance measure: accuracy

## 4.1 Training and test corpus from same domain

![[Pasted image 20240424195627.png]]

- MED smaller -> lower performance
- *unambiguous* words (only 1 tag in corpus): GENIA 90.74%, TB-2 84.62%, MED 83.84%
## 4.2 Training with TB-2

![[Pasted image 20240424200324.png]]
![[Pasted image 20240424200341.png]]

- performance degraded
- higher OOV rates
## 4.3 Adaptation with domain corpus

![[Pasted image 20240424200501.png|6: minor improvements from adding GENIA to TB-2|500]]

![[Pasted image 20240424200631.png|8: improvement esp in unigram model|500]]

![[Pasted image 20240424200640.png|9: again, minimal changes|500]]

>[!hyp] hypothesis: combining lexica leads to higher percentage of ambiguous word types
## 4.4 Use of lexicon

- MED corpus -> 500 most frequent words -> removed stop words etc -> manual POS tags
- improvements from small lexicon too, but improvement grows with size of lexicon

----

# 5. Conclusion

- both additional domain-specific corpora and (more cheaply) lexicons improve performance