Visit Ethiopia: RAG Chatbot


A production-grade Retrieval-Augmented Generation (RAG) assistant designed to provide comprehensive, grounded information regarding Ethiopian destinations, cultural landmarks, national services, and travel logistics\[cite: 4, 6]. The platform indexes official federal and regional tourism portals into a persistent Chroma vector collection and leverages hybrid retrieval to serve accurate responses\[cite: 1, 2, 6].



---



Core Capabilities



Hybrid Retrieval (Dense + Sparse):Combines semantic vector similarity with BM25 keyword scoring and fuzzy query expansion to catch spelling /typing errors for regional terminology and exact landmark names.

Automated Web Crawler:Headless Selenium and BeautifulSoup crawler tuned to extract domain-specific content across Ethiopian tourism platforms.

Intelligent Ingestion Pipeline: MD5 hash validation avoids duplicate indexing, while an automated orphan-purging routine removes outdated chunks.

Failover \& Multi-Key Management:Round-robin Google Gemini key rotation with fallback handling to local Ollama execution and safe-mode failover.

Multi-Channel Serving:Provides both a Streamlit interactive chat application and a RESTful Flask API server with rate-limiting, session memory, and response caching.




Architecture Flow



User Query

   │
   ▼

Streamlit App (app.py) / Flask API (api\_server.py)

   │
   ├── In-Memory Query Cache Check (Hit -> Instant Return)
   │
   └── Query Cache Miss

        │
        ├── [Query Expansion \& Intent Parsing]
        │
        ├── [Hybrid Search Engine]
        │        ├── Vector Semantic Retrieval (ChromaDB)
        │        └── Keyword Search (BM25 + Fuzzy Match)
        │
        ▼
     Context Assembly & Grounding
        │
        ▼
     LLM Generation (Gemini Primary / Ollama Fallback)
        │
        ▼
     Verified Response

Repository Structure


RAG-Chatbot/

├── RAG-Chatbot-from-web-data/
│   ├── chatbot/
│   │   ├── api_key_manager.py     # Gemini key rotation and cooldown management
│   │   ├── api_server.py          # Flask REST API backend (/ask, /ready, /admin)
│   │   ├── app.py                 # Streamlit interactive UI application
│   │   ├── demo.ipynb             # Interactive testing and indexing notebook
│   │   ├── hybrid_retriever.py    # Vector + BM25 keyword search engine
│   │   ├── ingest.py              # Batch ingestion CLI runner
│   │   ├── prompt.py              # Grounding prompts and personality templates
│   │   ├── text_to_doc.py         # Text cleaner, chunker, and LangChain Document creator
│   │   ├── utils.py               # Core pipeline orchestrator and Chroma connection
│   │   └── web_crawler.py         # Selenium web scraping utility
│   ├── generate_pdf_manual.py     # User manual PDF generation script
│   ├── requirements.txt           # Python dependencies
│   └── SETUP_GUIDE.md             # Detailed deployment documentation
├── generate_pptx.py               # Presentation deck generator
├── Visit_Ethiopia_Agentic_RAG_Presentation.pptx
├── .gitignore
└── README.md


Setup \& Installation

1. Clone the Repository

Bash

git clone [https://github.com/Lichetsega/VISIT-ETHIOPIA-RAG-CHATBOT-.git](https://github.com/Lichetsega/VISIT-ETHIOPIA-RAG-CHATBOT-.git)

cd VISIT-ETHIOPIA-RAG-CHATBOT-

2. Configure Environment Variables

Create a .env file inside RAG-Chatbot-from-web-data/:

Code snippet

GOOGLE_API_KEY_1="your_gemini_api_key_1"

GOOGLE_API_KEY_2="your_gemini_api_key_2"

CHATBOT_API_KEY="your_optional_service_auth_key"

3\. Run the Services

From the RAG-Chatbot-from-web-data/chatbot directory:


Start the Flask REST API Server:


Bash

python api\_server.py





Start the Streamlit User Interface:



Bash

streamlit run app.py





Ingest Data Sources Manually:


Bash

python ingest.py







Step 3: Commit and Push to GitHub



Run these commands in your Command Prompt (`C:\\Users\\liche\\Desktop\\RAG-Chatbot>`):



```cmd

git add .

git status

Verify that no .env files appear in the staged list. Then commit and push:



DOS

git commit -m "docs: restructure repository layout and update comprehensive README"

git push origin main

