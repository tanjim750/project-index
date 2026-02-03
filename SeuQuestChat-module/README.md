<h3 align="center">SEUQuest</h3>

<p align="center">
  <img src="seu_logo.png" alt="SEUQuest" width="100" height="100"/>
</p>

**SEUQuest** is an AI-powered Telegram chatbot for Southeast University. It answers student questions using retrieval-augmented generation (RAG), surfaces the latest university notices, and can search the library catalog for books.

[![SEUQuest ready-to-use](https://img.shields.io/badge/Gitpod-redy--to--code-908a85?logo=gitpod)](https://github.com/tanjim750/SeuQuest)

## What It Does

- **University Q&A** for departments like CSE, EEE, Pharmacy, Textile, Architecture, BBA, Law, English, Bangla, Economics, and MDS.
- **Notice updates** by scraping the SEU notice board PDFs and ingesting them into the vector store.
- **Library catalog search** (by title or author) from the SEU Koha catalog.
- **Feedback + training loop**: positive/negative feedback, and trainer mode for retraining responses.
- **RAG stack** using LangChain + Qdrant (cloud) and optional FAISS assets in `vectors/`.

## Project Layout

- `Main/main.py` - Telegram bot entry point.
- `Main/Telegram/` - Telegram API wrapper and command handling.
- `Main/Scraper/` - Notice and library scrapers.
- `Main/Reply/` - Response generation, sentiment gating, and training hooks.
- `Main/Seuquest_bot/` - RAG pipeline with LangChain + Qdrant.
- `notice_pdf/` - Downloaded notice PDFs.
- `vectors/` - Local vector store artifacts.

## Setup

1. Clone the repo:

```console
(.venv) $ git clone https://github.com/tanjim750/SeuQuest.git
```

2. Install dependencies (note: the file is named `requierments.txt` in this repo):

```console
(.venv) $ pip install -r requierments.txt
(.venv) $ python -m spacy download en_core_web_md
```

## Configuration

Before running, replace the hardcoded keys and URLs with your own:

- Telegram bot token in `Main/main.py` and `Main/Reply/reply.py`
- Qdrant URL + API key in `Main/Telegram/commad_handler.py`, `Main/Reply/reply.py`, and `Main/Scraper/notices.py`
- OpenAI API keys in `Main/Seuquest_bot/bot.py`
- Backend API base URL in `Main/Api/api.py`

For production, move these to environment variables or a secrets manager.

## Running

```console
(.venv) $ python Main/main.py
```

## Telegram Commands

- `/notices` - Get university notices
- `/find_book` - Search the library catalog (title or author)
- `/cse_dept`, `/eee_dept`, `/pharmacy_dept`, `/textile_dept`, `/architecture_dept`, `/bba_dept`, `/law_dept`, `/english_dept`, `/bangla_dept`, `/economics_dept`, `/mds_dept`

## Notes

- Notice scraping runs continuously in a background thread and updates the vector store on new PDFs.
- Trainer mode enables targeted retraining when responses are flagged.


