# DSPy Demo

This file shows the usage of a simple DSPy predictor and various optimization techniques.

The dataset was pulled from UC Irvine's ML repository, [here](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)

## Optimization

### Baseline

The baseline evaluation is done against a simple `dspy.Predict` pipeline against our `TextClassifier` and scored correctly classified 17/20 of our eval set.

### Labeled Few Shot

This is the simplest few shot example. You simply pass in your examples and it will sample between 16 and `len(train_set)` few-shot examples to include in our prompts.

This experiment raised our accuracy from 85% to 90%, only incorrectly classifying 2/20 results in our training set.

### Bootstrap Few Shot

A step above labeled few-shot is our bootstrap few shot, which both samples from our training set but also bootstraps new examples.

This experiment again raises our accuracy, this time only missing a single evaluation and scoring 95% on our simple metric.

### MIPROv2

The most complex optimizer in this example, MIPROv2 focuses both on optimizing few-shot examples as well as optimizing our instructions that are sent to our LLM for future classifications.

This is by far the most expensive optimizer in terms of time and token usage. This simple example ran 34 mini-batches and a number of full batches against our training data set. This optimization used between 2-3 million input tokens and about 20,000 output tokens. The time it took to complete this work was about 12 minutes, 20 seconds.

The increased time required to optimize our DSPy program paid off and this example scored 100% on our 20 message eval set and cumulatively 96.88% on our entire training set of 200 examples.

### Repo

If you want to test this yourself, the requirements are fairly straight forward:

* Phoenix is used for observability and the ability to see traces. Run `docker compose up -d` to start this container. You can view traces in your browser at http://localhost:6006
* Package management is done using `uv`.
* The python version used for this demo was 3.12.8
* Simply run `uv install` and begin running cells in `dspy.ipynb`!

Sample output configs for the following DSPy programs were saved:

* labeled few shot - labeled_few_shot.json
* bootstrap few shot - bootstrap-few-shot.json
* MIPROv2 - miprov2.json
