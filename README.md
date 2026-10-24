# Blog Writing Agent

A Streamlit application that turns a topic into a structured, research-aware blog post. It uses LangGraph to route the request, create an outline, write sections, and optionally plan and generate images.

## Features

- Generate a blog outline with audience, tone, blog type, and section tasks.
- Route topics as evergreen, hybrid, or current-events research.
- Optionally retrieve web sources with Tavily and display the collected evidence.
- Generate blog sections in parallel and combine them into Markdown.
- Optionally create image prompts and generate images with Gemini.
- Preview and download the Markdown, or download it with generated images as a ZIP.
- Browse Markdown blogs saved in the project directory.

## Requirements

- Python 3.10 or newer
- A Google Gemini API key
- A Tavily API key for web research (optional)

## Setup

Open a terminal in the project directory and create a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the dependencies:

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Create a `.env` file in the project directory:

```dotenv
GOOGLE_API_KEY=your_google_api_key
TAVILY_API_KEY=your_tavily_api_key
```

`GOOGLE_API_KEY` is required for blog generation and Gemini image generation. `TAVILY_API_KEY` is optional; without it, web search will not return research results. Keep `.env` private and do not commit API keys.

## Run

Start the Streamlit app from the project directory:

```powershell
streamlit run frontend.py
```

Streamlit prints a local URL in the terminal (typically `http://localhost:8501`). Enter a topic, choose an as-of date, and select **Generate Blog**.

## How it works

1. The router chooses `closed_book`, `hybrid`, or `open_book` based on the topic.
2. If research is needed, the app searches with Tavily and collects evidence.
3. The planner creates an outline and assigns section-writing tasks.
4. Section workers write the blog, which is assembled into Markdown.
5. The image planner may add image placeholders. If images are requested, Gemini generates them and the app saves them under `images/`.
6. The generated Markdown is saved in the project directory and shown in the app.

Use the **Plan**, **Evidence**, **Markdown Preview**, **Images**, and **Logs** tabs to inspect the run. The preview tab offers Markdown and ZIP downloads.

## Generated files

- `*.md` — generated blog posts in the current working directory
- `images/` — generated image files

These files and local environment artifacts are excluded from Git by `.gitignore`.

## Troubleshooting

- **Missing Google API key:** Add `GOOGLE_API_KEY` to `.env`, then restart Streamlit.
- **No research evidence:** Add a valid `TAVILY_API_KEY`. Evergreen topics may not trigger research.
- **Image generation fails:** Blog text can still be generated; check the displayed image-generation error and verify the Google API key and image-model access.
- **Changes do not appear:** Stop the running Streamlit process with `Ctrl+C` and run the start command again.

## Project files

- `frontend.py` — Streamlit user interface
- `backend.py` — LangGraph workflow, research, writing, and image generation
- `requirements.txt` — Python dependencies
