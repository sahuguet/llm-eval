# A another text experiment

We ask the LLM to extract sentences that reference a given entity.

Here is our toy dataset, from [Apple 10Q form](https://s2.q4cdn.com/470004039/files/doc_financials/2024/q2/_10-Q-Q2-2024-As-Filed.pdf), page 19.

> **Epic Games, Inc.** (“Epic”) filed a lawsuit in the U.S. District Court for the Northern District of California (the “California District Court”) against the Company alleging violations of federal and state antitrust laws and California’s unfair competition law based upon the Company’s operation of its App Store.
The California District Court found that certain provisions of the Company’s App Store Review Guidelines violate California’s unfair competition law and issued an injunction enjoining the Company from prohibiting developers from including in their apps external links that direct customers to purchasing mechanisms other than Apple in-app purchasing.
The injunction applies to apps on the U.S. storefront of the iOS and iPadOS® App Store.
On January 16, 2024, the Company implemented a plan to comply with the injunction and filed a statement of compliance with the California District Court.
On March 13, 2024, **Epic** filed a motion with the California District Court disputing the Company’s compliance plan and seeking to enforce the injunction.
The Company has filed an opposition to **Epic’s** motion. The Company believes it has substantial defenses and intends to vigorously defend itself.

We ask the LLM to return all sentences that reference Epic. There are 3 such sentences (see Epic highlighted on the content above).

## GPT-3.5-turbo ❌
```
% cat apple-10Q.txt | llm -m gpt-3.5-turbo "return sentences that mention Epic; format result as JSON array; just return the JSON"  
[
    "Epic Games, Inc. filed a lawsuit in the U.S. District Court against the Company alleging violations of federal and state antitrust laws and California\u2019s unfair competition law.",
    "On March 13, 2024, Epic filed a motion with the California District Court disputing the Company\u2019s compliance plan and seeking to enforce the injunction.",
    "The Company believes it has substantial defenses and intends to vigorously defend itself."
]
```
1. it does not get the correct result.
2. the last entry does not refernece Epic.
3. More concerning, sentences are not actual sentences from the original document. They have been rewritten.

Adding `verbatim` to the prompt fixes the issue but the answer is still incorrect.

```
% cat apple-10Q.txt | llm -m gpt-3.5-turbo "return verbatim sentences that mention Epic; format result as JSON array; just return the JSON" 
[
    "Epic Games, Inc. (\u201cEpic\u201d) filed a lawsuit in the U.S. District Court for the Northern District of California (the \u201cCalifornia District Court\u201d) against the Company alleging violations of federal and state antitrust laws and California\u2019s unfair competition law based upon the Company\u2019s operation of its App Store.",
    "The California District Court found that certain provisions of the Company\u2019s App Store Review Guidelines violate California\u2019s unfair competition law and issued an injunction enjoining the Company from prohibiting developers from including in their apps external links that direct customers to purchasing mechanisms other than Apple in-app purchasing.",
    "On March 13, 2024, Epic filed a motion with the California District Court disputing the Company\u2019s compliance plan and seeking to enforce the injunction."
]
```

Same outcome with GPT-4o: the same sentence is missing.
  
```
% cat apple-10Q.txt | llm -m gpt-4o "put sentence in a JSON array; go through each sentence and keep it if it references the company Epic; return the JSON"
```json
[
  "Epic Games, Inc. (“Epic”) filed a lawsuit in the U.S. District Court for the Northern District of California (the “California District Court”) against the Company alleging violations of federal and state antitrust laws and California’s unfair competition law based upon the Company’s operation of its App Store.",
  "On March 13, 2024, Epic filed a motion with the California District Court disputing the Company’s compliance plan and seeking to enforce the injunction."
]
```

##  Claude ❌

```
% cat apple-10Q.txt | llm -m claude-3-sonnet "return sentences that mention Epic; format result as JSON array; just return the JSON"      
[{"sentence": "Epic Games, Inc. (\"Epic\") filed a lawsuit in the U.S. District Court for the Northern District of California (the \"California District Court\") against the Company alleging violations of federal and state antitrust laws and California's unfair competition law based upon the Company's operation of its App Store."},
{"sentence": "On March 13, 2024, Epic filed a motion with the California District Court disputing the Company's compliance plan and seeking to enforce the injunction."}]
```

Claude only gets 2 out of 3.

## Gemini ❌

```
% cat apple-10Q.txt | llm -m gemini-pro "return verbatim sentences that mention Epic; format result as JSON array; just return the JSON"                        
[
  "Epic Games, Inc. (“Epic”) filed a lawsuit in the U.S. District Court for the Northern District of California (the “California District Court”) against the Company alleging violations of federal and state antitrust laws and California’s unfair competition law based upon the Company’s operation of its App Store.",
  "On March 13, 2024, Epic filed a motion with the California District Court disputing the Company’s compliance plan and seeking to enforce the injunction."
]
```

Same outcome with the last sentence missing.

A more elaborate prompt lead to the same incorrect outcome.

```
% cat apple-10Q.txt | llm -m gemini-pro "In the content above, the term 'The Company' references Apple Inc, not Epic. Put the sentences in a JSON array; go through each element of the array and filter out elements that do not mention Epic. Return the JSON array."
```





