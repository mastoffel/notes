---
draft: false
tags:
  - learning
---
This is an in-progress document capturing my thoughts about under-appreciated aspects of learning new things.
#### Transfer learning
[Transfer learning](https://en.wikipedia.org/wiki/Transfer_learning) is a method from machine learning. Instead of training a model from scratch to learn something, we chose a model that has been trained already. Then, we fine-tune it with little effort to work on a closely related task. Let's say we want to identify different species of kingfisher from photos. We can take a model that has been trained on categorising images like a [resnet](https://en.wikipedia.org/wiki/Residual_neural_network) and then merely fine-tune it on kingfisher images. This just means that we train it for a bit longer, on a few more images. This way, we can get a great result at a fraction of the cost / time to train the model from scratch. 

Now often when we learn a new thing, it's tempting to start from scratch, forgetting about everything we know already. But this doesn't have to be so. Instead, we can start by identifing 

Now an analogue in human learning could be this: When learning a new task, we can find the overlap between that task and something we already know. The challenge is in formulating the new task in a way that *fits*. 

**Language**: If you're an english speaker and want to learn french, there's good news. A substantial part of french and english words a cognates, they sound very similar. If we get a feeling for which words are cognates and how to pronounce them, we get about 30% of the french vocabulary for free.

**Data analysis**: Or you're using dplyr in R for data analysis, you might do something like this;

```r
df %>% 
	filter(age > 25) %>%
	select(name, city, score) %>%
	arrange(desc(score))
```

Now pandas code can look very different. But with a few tricks one can actually get very close;

```python
(df 
	.query('age > 25') 
	[['name', 'city', 'score']] 
	.sort_values('score', ascending=False) 
)
```
### pareto learning: 20% of stuff get you 80% of the way

### Anki

### MindMaps

### To have or to be


