**Problem:** Our agent is used by end users, not by us, and those users have different access rights. Each user should only receive the information they are authorized to see.

**Example:**
An HR platform has a built-in chat with an agent. A user can look up their own salary but not their colleagues' salaries.

If the user asks about their own salary, they get an answer. If they ask about a colleague's salary, the LLM recognizes that they lack permission and refuses. But if the user asks, "How does my salary compare to my colleague's?", the LLM answers, and the colleague's salary leaks.

**Solution:**
Put a deterministic permission layer between the LLM and the API. The LLM should not decide what data the user receives. It decides which endpoint to call, and every call then goes through a mandatory permission check before the request is sent.

![API CALL: Setting Permissions](../files/API%20CALL%20-%20Setting%20Permissions.png)

Diagram: [Excalidraw (online)](https://app.excalidraw.com/s/4fuBV9gHb6h/AjCixfU62pd)

GraphQL APIs show this most clearly, because we define which fields the schema exposes. The LLM chooses which fields to request, and then our filter keeps only the fields the user is allowed to access.

A second approach is to build a fixed set of API endpoints up front (REST or GraphQL). The LLM decides which one to call, and the permission layer checks that call against the user's permissions before the request is sent. We don't filter the response afterwards: a call the user isn't allowed to make never reaches the API.

**Conclusion:**
So it's not the LLM that decides whether to reveal the data or not. Data the user isn't allowed to see never reaches the LLM in the first place.
