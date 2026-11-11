As of 2026, several main patterns stand out in building orchestrators.

Goals:
- Minimizing the cost of building a feature
- Minimizing human involvement in the process

![Pattern for building your own orchestrators](../files/Pattern%20for%20building%20your%20own%20orchestrators.png)

Diagram: [Excalidraw (online)](https://app.excalidraw.com/s/4fuBV9gHb6h/1QUD1cMvhu6) · [.excalidraw file](../files/Pattern%20for%20building%20your%20own%20orchestrators.excalidraw)

## Moving repetitive steps into deterministic scripts

All repetitive processes are moved into deterministic scripts. An example of this approach: [Beefeater84/sdd-spec-kit](https://github.com/Beefeater84/sdd-spec-kit).

### Pros
- We get a deterministic result every time, and there is no need to explain all the nuances to the model. For example, I tell the model "call start-feature", and it calls the scripts that update staging from origin, create a new branch from it, pick a name based on the feature, and switch to this branch. Without scripts, the model often forgot to update staging and tried to create the branch from the release one.
- The decision to execute scripts can be given to a cheaper model, which saves resources.

### Cons
- Deterministic steps are hard to adapt to a team. Everyone gets the same result, the one the author needs. But if I want to change the result for myself, I need to rewrite the script.
- Scripts need to be maintained, and they work very poorly in non-standard cases.

## A strong model plans, cheap ones execute

The main goal is reducing the cost of work. Planning is a complex process that requires a lot of context. Different approaches and solutions are discussed along the way, but once the plan is made, it can be described in an artifact and handed over for execution to a less expensive model.

But we also have to plan how we verify the result and how we handle changes to the plan during the work. And also: if a few days after the work we discover that something is wrong, where are the plan description and its original intent stored, so that the models don't have to explain and plan everything again.

### Pros
- Saving the overall cost by having execution done by cheaper models.
- Saving the orchestrator's context. For example, planning may take 400k tokens of context, and implementation 700k. Only 400k will be spent in the orchestrator, and there will still be room left for verification and fixes.

### Cons
- The subagent's context doesn't get into the orchestrator, only the result does. If the subagent made some decision or deviated from the plan during the work, it doesn't always report this to the orchestrator.
- If the subagent needs to make a decision during the work, that decision may not be as good as a frontier model's.

## Creating specialized subagents

A group of agents is created, each with its own role (for example: Product agent, Security agent, Frontend agent, Backend agent, etc.). There can also be several agents per role, each with its own specialization.

The orchestrator brings each of them into the discussion and review depending on the feature.

The goal of the pattern is minimizing human involvement. The agents must look at the problem from all sides and decide on their own how to implement it.

The human evaluates only the [RFC](Request%20for%20Comments%20(%20RFC%20).md), the [ADR](Architecture%20Decision%20Record%20(%20ADR%20).md), state diagrams and correspondence diagrams.

But evaluating the quality of the solution remains a big problem. Patterns for evaluating it are only now taking shape, because agents can evaluate the code, but not the "user's intent". The current approaches for this are BDD (Behavior-Driven Development), manual tests, and reverse tracing of incidents (when incidents are linked to PRs and investigated based on the decisions that were made).

### Cons
- A large number of agents eats a huge amount of tokens.
- Each agent has its own opinion, and it is very hard to set up the balance for when they must make a decision. Described in [Pattern for verifying work: Reviewer and Validator](Pattern%20for%20verifying%20work%20Reviewer%20and%20Validator.md).

### Requirements for implementing the pattern
- For the pattern to work well, the agents must gather context efficiently, so working with memory must be set up in the project.
- For minimal human involvement, the classic pitfalls described in the [patterns](../00%20Patterns.md) need to be solved.

### Evals
- The number of requests to the human relative to the amount of work done
- That the number of incidents doesn't increase
