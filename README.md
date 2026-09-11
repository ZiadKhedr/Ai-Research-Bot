# AI Research Assistant

A multi-tool AI research agent built with **Ollama** (local LLM) and **Gradio**. Give it a topic, and it generates research questions, searches the web, scrapes and summarizes sources, compares them, answers follow-up questions, and produces a structured report — all in one pipeline.

## Features

- **Topic-driven research**: enter a topic, get relevant search queries generated automatically
- **Live web search & scraping**: pulls real sources from the web and extracts clean text
- **Source summarization**: condenses each source into a concise summary
- **Source comparison**: identifies agreements and differences between two sources
- **Structured report generation**: produces an Introduction, Key Findings, Conclusion, and Sources list
- **Follow-up Q&A**: ask questions about the research with conversation context preserved
- **Tool routing**: the LLM decides which tool to call based on the user's request
- **Gradio chat interface**: simple web UI to interact with the assistant

## Requirements

- Python 3.10+
- [Ollama](https://ollama.com) installed and running locally
- An Ollama model pulled (this project uses `qwen3:8b`):
  ```
  ollama pull qwen3:8b
  ```

## Installation

```bash
pip install requests beautifulsoup4 gradio ollama ddgs
```

## Project Structure

The project is a single Jupyter notebook (`Research_assistant.ipynb`) organized into these sections:

| Section | Function(s) | Purpose |
|---|---|---|
| Setup | `ask_ollama()` | Shared wrapper for all Ollama calls |
| Research Questions | `generate_research_questions(topic)` | Breaks a topic into specific search queries |
| Web Search | `search_web(query)` | Finds relevant sources via DuckDuckGo |
| Page Scraper | `scrape_page(url)` | Extracts clean text from a webpage |
| Source Summarizer | `summarize_source(content)` | Condenses a source into a short summary |
| Source Comparator | `compare_sources(source1, source2)` | Identifies agreements/differences between two sources |
| Report Generator | `generate_report(sources, topic)` | Produces a structured Markdown research report |
| Tool Router | `route_tool_call(user_message, session)` | Decides which tool(s) to call for a given request |
| Follow-up Q&A | `answer_question(question, session)` | Answers questions using collected sources + conversation history |
| Orchestration | `run_research(topic, num_sources)` | Runs the full pipeline: questions → search → scrape → summarize |
| UI | `gradio_chat()` + `gr.ChatInterface` | Web chat interface tying everything together |

## Usage

1. Open `Research_assistant.ipynb` in Jupyter or VS Code.
2. Make sure Ollama is running (`ollama serve`) with the model pulled.
3. Run all cells top to bottom (**Restart & Run All** if you've made changes, so functions load in order).
4. Launch the Gradio app (`demo.launch()`) and interact via chat
### Gradio commands
- `research: <topic>` — starts a new research session on that topic
- `report` — generates the full structured report from collected sources
- any other message — treated as a follow-up question about the current research

## Notes & Known Limitations

- Some pages block scraping or require login — these are skipped/return empty content, which can reduce source count for certain topics.
- Web search relies on `ddgs` (DuckDuckGo); occasional timeouts or rate-limiting can occur. The search function retries automatically before failing.
  
## Team

| Part | Owner |
|---|---|
| Web Search, Page Scraper, Tool Routing, Orchestration, Gradio UI | ZiadKhedr |
| Research Questions, Source Summarizer, Source Comparator, Report Generator | mosaad9000 |
