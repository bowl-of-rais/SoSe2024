
The goal of the following preprocessing step is to identify the **start and end tokens of the causal span** after tokenizing the sentence.

The tokenizer returns a dict containing:
- `input_ids`
- `token_type_ids`
- `attention_mask`
- `offset_mapping`
- `overflow_to_sample_mapping`
which contains values for each sample in the split.
Mainly important here is the `offset_mapping`. For each sentence, it has a list of tuples that indicate the character spans of tokens in the raw input text.

Example: `[(0, 0), (0, 2), (2, 3), ...]`. Note that the first entry is the `[CLS]` token.

To determine the start and end tokens of the causal spans, all that needs to be done is
1. find the start and end of the causal span in the raw sentence (`cause_start_char`, `cause_end_char`)
2. find the indices of the tuples in the `offset_mapping` that the start and end fall into.
By default (i.e. if the correct tuples cannot be found, or no cause exists in the sentence), the cause is mapped to the `[CLS]` token.

```python
def preprocess(split):

sentences = split['sentence']

causes = split['concept1']

relations_present = split['has_relations']

  

inputs = tokenizer(

sentences,

#max_length=384,

#truncation="only_second",

return_offsets_mapping=True,

return_overflowing_tokens = True,

padding="max_length"

)

  

print(inputs.keys())

  

offset_mapping = inputs.pop('offset_mapping')

start_positions = []

end_positions = []

  

for i, offset in enumerate(offset_mapping):

  

# sentence has no causal relation: label (0, 0)

if(not relations_present[i]):

start_positions.append(0)

end_positions.append(0)

# else find start/end tokens of causal span

else:

cause = causes[i]

cause_start_char = sentences[i].find(cause)

cause_end_char = cause_start_char + len(cause)

  

sequence_ids = inputs.sequence_ids(i)

  

# find first and last offsets belonging to sentence

sentence_start = next((i for i, v in enumerate(sequence_ids) if v == 0), 0)

sentence_end = next((len(sequence_ids) - i - 1 for i, v in enumerate(reversed(sequence_ids)) if v == 0), 0)

  

# start token position: *last* index at which token_start is <= start_char

start_token_index = next((len(offset) - i - 1 for i, (token_start, token_end) in enumerate(reversed(offset))

if sentence_start <= len(offset) - i - 1 <= sentence_end and token_start <= cause_start_char <= token_end), 0)

# end token position: *first* index at which end_char is <= end_char

end_token_index = next((i for i, (token_start, token_end) in enumerate(offset) if

sentence_start <= i <= sentence_end and token_start <= cause_end_char <= token_end), 0)

  

start_positions.append(start_token_index)

end_positions.append(end_token_index)

inputs["start_positions"] = start_positions

inputs["end_positions"] = end_positions

return inputs
```