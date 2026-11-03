The pattern is that we simultaneously run many different agents on the same task ( most often different LLMs with the same instructions ).

We expect that they will make many different findings, and we will average the results.

![Pattern of parallel evaluation by different agents](../files/Pattern%20of%20parallel%20evaluation%20by%20different%20agents.png)

Diagram: [Excalidraw (online)](https://app.excalidraw.com/s/4fuBV9gHb6h/9APWF3pzYEy) · [.excalidraw file](../files/Pattern%20of%20parallel%20evaluation%20by%20different%20agents.excalidraw)

## Downsides of the approach
1) Agents often have the same "blind" spots.
2) Averaging works very poorly with non-deterministic output. For example, the same code with a possible bug is described differently by different models
