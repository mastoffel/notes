Data wrangling in Python *seems* clunky. 
Yet, it doesn't have to be. Here is **how to pipe in Python.** 

## The problem

In `R`, we process data beautifully with `dplyr` and `%>%`:

```r
suppressMessages(library(dplyr))
iris %>%
  mutate(sepal_ratio = Sepal.Width / Sepal.Length) %>%
  filter(sepal_ratio > 0.5) %>%
  select(Species, sepal_ratio) %>%
  group_by(Species) %>%
  summarise(mean_sepal_ratio = mean(sepal_ratio))
```

In Python's `pandas`, most data wrangling code looks much less great, often like this:

```python
import pandas as pd
url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv"
iris = pd.read_csv(url)

iris["sepal_ratio"] = iris["sepal_width"] / iris["sepal_length"]
iris = iris[iris["sepal_ratio"] > 0.5]    #filter
iris = iris[["species", "sepal_ratio"]]   #select
iris = iris.groupby("species")            #group by
iris = iris.agg({"sepal_ratio": "mean"})  #summarise
```

This is hard to read because there's lots of repetition. 

## A new hope

But don't despair! We can pipe in Python, too. Here is how:

```python
iris = pd.read_csv(url)

(iris
  .assign(sepal_ratio = lambda x: x.sepal_width / x.sepal_length)
  .query("sepal_ratio > 0.5")
  .loc[:, ["species", "sepal_ratio"]]
  .groupby("species")
  .agg({"sepal_ratio": "mean"})
  )
```

For now, there are only two main principles to remember:

1. Use `.` instead of `%>%` to pipe. Put `.` at the beginning of the line.
2. Use `()` around the whole expression. Python will complain otherwise.

There is a pipeable method for most tasks. Sometimes, though, there isn't, but you can still make it work. 

1. Use `lambda` to define a function on the fly (like in .assign() above).
2. Use `.pipe()`, which takes a function as an argument and allows you to pipe it. 

Here is an example of .pipe():

```python
def sepal_ratio(df):
  return df.assign(sepal_ratio = df.sepal_width / df.sepal_length)

(iris
  .pipe(sepal_ratio)
  .query("sepal_ratio > 0.5")
  .loc[:, ["species", "sepal_ratio"]]
  .groupby("species")
  .agg({"sepal_ratio": "mean"})
  )
```

That's it! Some people don't like piping because, they say, it's harder to debug. I like to simply comment out lines, allowing you to run it line by line, which makes it actually very easy to debug. A pipe is also easy to read as it's basically like a recipe. Start with a dataframe and change stuff step by step.

Here's a quick summary over the most common data wrangling tasks and their pipeable methods and functions in `dplyr` and `pandas`:

| task              | dplyr        | pandas           |
| ----------------- | ------------ | ---------------- |
| filter rows       | filter()     | df.query()       |
| pick columns      | select()     | df.loc[]         |
| group by          | group_by()   | df.groupby()     |
| summarise         | summarise()  | df.agg()         |
| make new variable | mutate()     | df.assign()      |
| join dfs          | inner_join() | df.merge()       |
| sort df           | arrange()    | df.sort_values() |
| rename columns    | rename()     | df.rename()      |

## Not so secret bonus: `siuba`

There is also the beautiful [siuba](https://siuba.org/) package. If you come from `R`, this might be the way to go. But even if not, it's still less verbose than `pandas`. Last time I tried it, the package didn't quite have everything I needed but I think it grew a lot since then. Here is the same pipeline, but with `siuba`:

```python
from siuba import _, mutate, filter, select, group_by, summarize

(iris
  >> mutate(sepal_ratio = _.sepal_width / _.sepal_length)
  >> filter(_.sepal_ratio > 0.5)
  >> select(_.species, _.sepal_ratio)
  >> group_by(_.species)
  >> summarize(mean_sepal_ratio = _.sepal_ratio.mean())
  )
```