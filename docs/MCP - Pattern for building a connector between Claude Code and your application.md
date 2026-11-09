MCP is a way to connect your application to the environments your users are used to, for example Claude Code, Codex or ChatGPT. This is especially convenient for non-technical users: if the server is remote, they don't need to install anything, it's enough to connect it in their environment. All the complexity goes on your side. But at the same time users spend their own tokens and LLM limits, if your server doesn't call an LLM itself.
## Data transfer protocol

1. **Streamable HTTP** - Every request is a POST request, which closes when the response is received. Works well for Serverless architecture, because the call isn't held open, as with HTTP+SSE, and we don't pay for a constantly open connection.

2. **HTTP+SSE** - is considered deprecated. The server kept a constantly open SSE stream for messages to the client, and the client sent its messages as separate POST requests. Between messages the stream sat idle, but stayed open and could cost money on platforms that bill for execution time, for example AWS Lambda. Also this approach scaled poorly: say our backend runs on several servers, the SSE stream is open with one of them, and the POST went through the load balancer to another. You had to pin the client to one server (sticky sessions) or connect the servers through a shared broker like Redis.

### Common traps you need to know about

`subscriptions/listen` - an MCP method the client uses to open a long-lived stream, to learn about changes to the lists of tools, prompts and resources or about updates to specific resources. While the stream is open, on platforms that bill for execution time (for example, AWS Lambda) money is charged even when idle. So before enabling subscriptions check your hosting's billing model. If subscriptions aren't needed, the server can choose not to support them.

## Saving memory

MCP has no built-in memory between tool calls.

In the latest MCP revisions, for saving information about the work, and also if a workflow requires calling several tools in sequence - it's recommended to use a handle. It's an id of an "event" that we can pass inside different tools. We store it in our database.

**Important nuance:** Always store the handle linked to the user, so you don't accidentally give information to another user
