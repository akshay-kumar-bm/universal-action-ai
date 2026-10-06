# Unlimited Input GPT (universal-action-ai)

A Streamlit app that lets you feed an LLM content from many sources at once (web URLs, YouTube videos, PDFs, text files, pasted text), then either summarise it or chat with it.

## Features
- Sources: website URLs (`UnstructuredURLLoader`), YouTube (`YoutubeLoader`), PDF (`PyPDFLoader`), text files and pasted text; several at a time.
- Task "Process Content": predefined actions with summarisation chains (`load_summarize_chain`, with a refine prompt for long input; chunk size is derived from the model context length).
- Task "Interactive Q&A": builds a FAISS index with HuggingFace sentence-transformer embeddings and answers via `RetrievalQA`.
- Models via Groq: `llama3-8b-8192`, `gemma2-9b-it`, `mixtral-8x7b-32768` (selectable in the sidebar).
- A small number of free actions with a built-in key, after which the user supplies their own Groq API key.
- Packaged as a Hugging Face Space (README front-matter, `sdk: streamlit`) with a GitHub Action that force-pushes `main` to the Space.

## Tech stack
Python, Streamlit, LangChain (+ langchain-groq, langchain-community), FAISS (CPU), sentence-transformers, youtube-transcript-api, unstructured, pytube, validators, python-dotenv.

## Structure
```
app.py                     # ContentProcessor class + Streamlit UI
requirements.txt
README.md                  # HF Space metadata only
.github/workflows/main.yml # sync to Hugging Face Space (needs HF_TOKEN secret)
.devcontainer/             # dev container config
```

## Run
```
pip install -r requirements.txt
streamlit run app.py
```
Provide a Groq API key in the sidebar. Deployment sync requires the `HF_TOKEN` GitHub secret.

## Limitations
- Uses older LangChain import paths (`langchain.vectorstores`, etc.) that may need updating on current versions.
- The sidebar free-use key is hard-coded in source (see security notes) and should be replaced with an env/secret.
- Model IDs are the ones valid at the time of writing and may be deprecated by Groq.
