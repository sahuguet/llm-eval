For the ground truth, we ran the content through GPT-4o and then manually reviewed against the original document.


We created a `llm` template using the following command:

```
% llm --system 'This is an earning call transcript. Please provide the list of questions from analysts; use JSON format with name, affiliation and question' --save earning-call
```

For each earning call file, we call the `llm` tool as follows:
```
% cat file.txt | llm -t earning-call
```


When we manually check the original transcript, if some entries are missing we add a `"__note__": "added manually"` in the JSON.



