# fast-rlm

**Recursive Language Models**

fast-rlm is an inference technique where an LLM interacts with arbitrarily long prompts through an external REPL. The LLM can write code to explore, decompose, and transform the prompt. It can recursively invoke sub-agents to complete smaller subtasks. Crucially, sub-agent responses are not automatically loaded into the parent agent's context — they are returned as symbols or variables inside the parent's REPL.

[:material-download: Install](getting-started/installation.md){ .md-button .md-button--primary }
[:material-play: Quick Start](getting-started/quickstart.md){ .md-button }

---

## How it works

```
User Query
    |
    v
Root Agent (primary_agent)
    |-- writes Python code in REPL
    |-- calls llm_query() to spawn sub-agents
    |       |
    |       v
    |   Sub-Agent (sub_agent)
    |       |-- explores a chunk of context
    |       |-- returns result as a variable
    |       v
    |   (result flows back as symbol, not raw text)
    |
    v
Final Answer
```

The root agent orchestrates the task by writing Python code. When it needs help with a subtask, it calls `llm_query()` which spawns a child agent. Child agents can spawn their own children, up to `max_depth` levels deep. Each agent has a budget of `max_calls_per_subagent` LLM calls.

## Features

- **Recursive decomposition** — agents spawn sub-agents to handle subtasks
- **REPL-based reasoning** — agents write and execute Python code iteratively
- **Custom Python tools** — register your own functions; the agent calls them like normal Python, and can even define new tools mid-run to hand to sub-agents ([guide](guide/advanced.md#tool-calling-process))
- **Scoped environment variables** — inject credentials into the REPL via `env_variables=` without exposing them to the model ([guide](guide/advanced.md#environment-variables))
- **Typed inputs & outputs** — pass a `dict` query and constrain `FINAL` with a Pydantic model or JSON Schema ([guide](getting-started/quickstart.md#structured-input-output))
- **Budget controls** — set hard limits on depth, calls, and dollar spend
- **Any OpenAI-compatible API** — works with OpenRouter, OpenAI, local models, etc.
- **Structured logging** — every step logged as JSONL, viewable in an interactive TUI

## Learn More

[:material-file-document: Read the original RLM paper on arXiv](https://arxiv.org/abs/2512.24601)

!!! note
    my-fast-rlm is an independent implementation of the ideas from the RLM paper. This project is not affiliated with the original authors.
