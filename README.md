# Newsletter Agent: Research-Driven Article Generation with LangGraph

A multi-step LangGraph pipeline that takes a topic, researches it with
LLM-chosen tools, writes a sourced article, and outputs clean Markdown or HTML.

## Architecture

START → researcher_agent ⇄ tools → collect_research → outliner → writer
      → (route by output_format) → markdown_formatter | html_formatter → END

## Features
- **Tool-calling research agent**: the model chooses between web search,
  Wikipedia and a date tool, in a loop with a hard tool budget
- **Grounded writing**: facts come only from retrieved sources, with a Sources section
- **Conditional routing** to separate Markdown and HTML formatter nodes
- **State management** with typed state and the `add_messages` reducer
- **Safety limits**: tool-call budget plus LangGraph recursion limit

## Tech stack
Python, LangGraph, LangChain, OpenAI (gpt-4o-mini), DuckDuckGo search, Wikipedia API

## Quick start
1. Open `newsletter_agent.ipynb` in Google Colab
2. Add `OPENAI_API_KEY` to Colab Secrets
3. Run all cells, then:
```python
   generate_newsletter_v4("Remote work trends this year", "html")
```

## Example output
See [examples/sample_newsletter.md](examples/sample_newsletter.md).



## What I learned
- Designing graph state and reducers
- Letting the model choose tools, and why tool descriptions drive its choices
- Fixing a real failure: the agent assumed the wrong year until I forced a date lookup first
- Bounding loops with a hard budget

## Limitations and next steps
- Search snippets only (no full-page reading); results vary between runs
- Planned: reviewer loop, human approval step, checkpointing, LangSmith tracing
