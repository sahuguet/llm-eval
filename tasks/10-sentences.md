# A simple text experiment

We compare various LLMs against a simple task.

We use the excellent Python `llm` package (see https://llm.datasette.io/en/stable/).

## We create a toy dataset
We start with 10 random sentences.
```
 % llm "generate ten sentences"
1. The sun was shining brightly in the clear blue sky.
2. The children were playing happily in the park.
3. The birds were chirping in the trees, creating a beautiful symphony.
4. The smell of freshly cut grass wafted through the air.
5. People were out for a leisurely stroll, enjoying the warm weather.
6. A gentle breeze rustled the leaves on the trees.
7. The sound of laughter could be heard all around.
8. The colorful flowers were in full bloom, adding a pop of color to the landscape.
9. The feeling of peace and tranquility surrounded the area.
10. It was a perfect day to be outdoors, soaking up the beauty of nature.
```

We then run a couple of simple queries against this tiny dataset.

## We count sentences

### gpt-3.5-turbo
```
% cat 10-sentences.txt | llm -m gpt-3.5-turbo "how many sentences? Return a number"
There are 10 sentences. ✅
```

### claude-3-haiku
```
 % cat 10-sentences.txt | llm -m claude-3-haiku "how many sentences"
The passage you provided consists of 10 sentences. ✅
```

### gemini-pro
```
% cat 10-sentences.txt | llm -m gemini-pro "how many sentences?"                                    
10
```


## We get the first, last and fifth sentences
### gpt-3.5-turbo
```
cat 10-sentences.txt | llm -m gpt-3.5-turbo "first, last and fifth sentence, verbatim"
The sun was shining brightly in the clear blue sky. ✅
It was a perfect day to be outdoors, soaking up the beauty of nature. ✅
People were out for a leisurely stroll, enjoying the warm weather. ✅
```

### claude-3-haiku
```
% cat 10-sentences.txt | llm -m claude-3-haiku "return first, last and fifth sentences"
The sun was shining brightly in the clear blue sky. ✅
It was a perfect day to be outdoors, soaking up the beauty of nature. ✅
The feeling of peace and tranquility surrounded the area. ❌  (this is the 9th, not the 5th)
```

`claude-3-sonnet` seems to have zero problem with this task.
```
% cat 10-sentences.txt | llm -m claude-3-sonnet "return first, last and fifth sentences"       
Here are the first, last, and fifth sentences from the given text:

First sentence: The sun was shining brightly in the clear blue sky. ✅
Fifth sentence: People were out for a leisurely stroll, enjoying the warm weather. ✅
Last sentence: It was a perfect day to be outdoors, soaking up the beauty of nature. ✅
```


### gemini-pro
```
% cat 10-sentences.txt | llm -m gemini-pro "return first, last and fifth sentences"
The sun was shining brightly in the clear blue sky. ✅
People were out for a leisurely stroll, enjoying the warm weather. ❌ (this is the fifth one)
It was a perfect day to be outdoors, soaking up the beauty of nature. ❌ (this is the last one)
```
The Gemini provides the correct answers but out of order. Forcingg the LLM to use a JSON array fixes the issue.
```
% cat 10-sentences.txt | llm -m gemini-pro "return the first, the last and the fifth sentences as a JSON array with index (starting from 1) and sentence"
[
  {
    "index": 1,
    "sentence": "The sun was shining brightly in the clear blue sky." ✅
  },
  {
    "index": 5,
    "sentence": "People were out for a leisurely stroll, enjoying the warm weather." ✅
  },
  {
    "index": 10,
    "sentence": "It was a perfect day to be outdoors, soaking up the beauty of nature." ✅
  }
]
```




