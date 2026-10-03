# AI Document Assistant (RAG Chatbot)

A modular Retrieval-Augmented Generation (RAG) application built with LangChain
and Streamlit. Upload a PDF and have a context-aware conversation about its
content, with a choice of OpenAI, Google Gemini, or Anthropic as the LLM
provider, plus an optional DuckDuckGo web-search fallback when the answer
isn't in the document.

## Features
- PDF ingestion and chunking (PyPDFLoader + RecursiveCharacterTextSplitter)
- FAISS vector store for semantic search over document chunks
- Multi-provider LLM support: OpenAI, Google Gemini, Anthropic
- Conversational memory across turns (chat history passed into the chain)
- Web search fallback (DuckDuckGo) when the document doesn't contain the answer
- Streamlit UI with streaming responses

## Project Structure

```
ai-doc-assistant/
├── app.py                        # Streamlit frontend
├── loaders/
│   └── document_loader.py        # PDF loading (PyPDFLoader)
├── processing/
│   └── text_splitter.py          # Chunking (RecursiveCharacterTextSplitter)
├── embeddings/
│   └── embedding_model.py        # OpenAI / Google embedding configuration
├── vectorstore/
│   └── faiss_store.py            # FAISS vector store management
├── chains/
│   └── conversational_chain.py   # LCEL retrieval chain (prompt + LLM + parser)
├── tools/
│   └── web_search.py             # DuckDuckGo web search fallback
└── requirements.txt               # Project dependencies
```

## Building Blocks

Following the principles of building LLM apps, this project implements:
1. **The Brain (LLM):** choice of GPT-4o-mini/GPT-4o, Gemini, or Claude
2. **Retriever:** FAISS similarity search over document chunks
3. **Memory:** chat history tracked in Streamlit session state and passed
   into the chain for follow-up questions
4. **Chain:** an LCEL pipeline connecting retrieval, prompt, and LLM
5. **Tools:** a DuckDuckGo web search fallback the app invokes when the
   document doesn't contain the answer

## Installation and Setup

```bash
git clone https://github.com/Ezmir12/ai_doc_assistant.git
cd ai_doc_assistant
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Environment Variables

Copy `.env.example` to `.env` and add the API key(s) for whichever
provider(s) you plan to use:

```
OPENAI_API_KEY=your_actual_key_here
GOOGLE_API_KEY=your_actual_key_here
ANTHROPIC_API_KEY=your_actual_key_here
```

(Anthropic has no embedding model of its own, so document embeddings
fall back to `OPENAI_API_KEY` even when Claude is the selected chat model.)

## Usage

```bash
python -m streamlit run app.py
```

Developer: Yonatan Azmir
