# Function Calling Using LLMs — Martin Fowler
[martinfowler.com/articles/function-call-LLM.html](https://www.martinfowler.com/articles/function-call-LLM.html)

---

## Core clarification

The LLM never executes the function. It analyses the prompt, selects the appropriate function, extracts the arguments, and outputs a structured JSON description of the call. The host application does the actual execution.

## Key pattern

1. System prompt defines available functions and their schemas
2. User prompt provides the request
3. LLM outputs structured JSON: `{ "function": "...", "args": {...} }`
4. Application parses, executes, optionally feeds result back to LLM

## Critical insight on security

Restricting the agent's action space with explicit conditional logic is essential — prevents prompt injection attacks. The more expressive the action space, the larger the attack surface.

## LLMs vs rules engines

Function calling gives "context-aware, adaptive behaviour driven by language understanding" rather than rigid static rules that create combinatorial explosion. Tradeoff: less transparent, less deterministic.

## MCP (Model Context Protocol)

Enables dynamic tool discovery — model can discover available tools at runtime rather than having them hardcoded. Adds flexibility but also complexity. Only justified for sophisticated agents.
