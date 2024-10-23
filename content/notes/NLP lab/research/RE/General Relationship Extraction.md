# Notes from NLP lecture

### Relation Extraction

- all kinds of relations
- unary: NER

#### Ontological Databases

- Semantic Web
  - RDF: subject-predicate-object triples (Resource Description Framework)
  - DBPedia: derived from wikipedia, RDF triples
- Wikipedia
- Freebase
- Wordnet
- domain-specific ontologies
- TACRED: hand-labeled, 41 general relation types

#### Pattern-Based

- lexico-syntactic patterns (Hearst rules)
  - e.g. $"NP"_0 "such as" "NP"_1$
- patterns using NE types
- high precision, but low recall

#### Using Supervised ML

- 3 sub-calssifiers: entities, relationship presence, relationship type
- possible features: headwords, bag of words, NE types, constituent path

#### Neural RE

- replace entities with tags -> encoder -> linear classifier
- Hearst-style queries

#### Bootstrapping

- learn new rules/patterns from seed rules/tuples
- avoid semantic drift: assign confidence values

#### Distant Supervision

- large number of seeds from relation databases
- "no-relation" relation for tuples not in data
- rich feature(conjunction)s, hand-created knowledge -> high-precision, no semantic drift, no genre bias. relies on database.

#### Unsupervised

- ReVerb (2001): TODO
- confidence classifier
- large numbers of relations without specifying in advance

#### Evaluating

- supervised: as usual
- less supervised: estimated precision + recall

### Event Extraction

- mostly verbs
- classes of events + sub-classes -> supervised ML with IOB tagging

### Template Filling

- common situations/event sequences with slots that need to be filled
  - template recognition + role filler extraction