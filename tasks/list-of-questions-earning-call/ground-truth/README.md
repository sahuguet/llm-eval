For the ground truth, we ran the content through `gpt-3.5-turbo` and then manually reviewed against the original document.


We created a `llm` template using the following command:

```
% llm --system 'This is an earning call transcript. Please provide the list of questions from analysts; use JSON format with name, affiliation and question' --save earning-call
```

For each earning call file, we call the `llm` tool as follows:
```
% cat file.txt | llm -t earning-call
```

When we manually check the original transcript, if some entries are missing we add a `"__note__": "added manually"` in the JSON.

Surprisingly, the extraction is far for 100% accurate, 3 out of five files missing one or more entry.
```
% grep -l "manually" *.json
apple-q2-2024.json
choice-hotels-q1-transcript.json
netflix-inc-_earnings-call_2024-04-18_english.json
```

Even more surprisingly, the missing entries look pretty random. For Netflix, we get a bunch of missing analysts in the middle and the last one is correctly extracted.

