A pattern where we check the result with 2 models:

One should find as many problems as possible ( Perseption ), and the second model should criticize all the findings and demand additional evidence, drop some of the bugs, etc.

![Pattern for verifying work: Reviewer and Validator](../files/Pattern%20for%20verifying%20work%20Reviewer%20and%20Validator.png)

Diagram: [Excalidraw (online)](https://app.excalidraw.com/s/4fuBV9gHb6h/5dVzWNPOzEn) · [.excalidraw file](../files/Pattern%20for%20verifying%20work%20Reviewer%20and%20Validator.excalidraw)

## Specifics

### Can't evaluate the intent, can only evaluate the code.
This problem is not solved yet.

### Reviewer and Validator agree with each other too easily or are too strict with each other.

At the moment, the aggressiveness of the Reviewer and Validator is handled by "tweaking" the input prompts.

What needs to be checked:
- That the agents haven't entered an infinite loop of passing things back and forth to each other. Because if you ask an agent "what can be improved", it will always find something.
- That the critic doesn't find new and new bugs on every iteration.

## Evals

Main metrics Perseption / Recall
Perseption - how many bugs were found in total
Recall - how many of the found bugs are actually bugs
