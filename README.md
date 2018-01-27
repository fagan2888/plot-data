# plot-data

### Extracting examples from querylog
  ```
  jq -c 'if .q[0]=="accept" then .q[1] else empty end' querylog/turk0721.jsonl > examples-turk0721.jsonl
  ```

### Preprocessing

Make sure you strip the `_id` key from data, since it changes between context and targetValue,
and results in erroneously flagging correct parses as incorrect.  Try running:

    jq -c 'del(.context.data.values[]?._id) | del(.targetValue.data.values[]?._id)' raw.jsonl > fixed.jsonl

Alterantively, run `fix_file.sh`, which does the same thing.

### Generating splits

Use `split_data.py` to split data into train/test (no dev since all the Turk data is dev data):

    python split_data.py randomWithNoCanon.jsonl randomWithNoCanon_splitIndep  # Split each example separately 
    python split_data.py -s randomWithNoCanon.jsonl randomWithNoCanon_splitSess  # Split by sessionId == MTurk ID

### Examples

* `human/megha.ex.jsonl`: 31 unique utterances, 121 distinct examples
* `human/sidaw.ex.jsonl`: 14 unique utterances
* `raw/`: utterances are reasonable, but some context are messed up. in examples-megha contains 500 utterances, the first 80 has correct context. 
* `summer2017turk/turk_overnight_2k.jsonl`: 1954 utterances, utterance quality is good, but only for a few actions
* `summer2017turk/turk0721.ex.jsonl`: 226 unique utterances, 561 examples. Most context values are messed up (#78 and after are all messed up).
* `summer2017turk/turk0803_interactive.jsonl`: 241 unique utterances, 241 examples - user initiative
* `summer2017turk/turkRandom.jsonl`: reasonable data quality, formula generated by json diff, had some old object values such as setting data.url to something in the examples
* `summer2017turk/randomWithNoCanon|randomWithCanon.jsonl`: summer datasets. Reasonable quality. NoCanon has a lot of no change and misinterpretations.

