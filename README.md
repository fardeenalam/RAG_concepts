# 🧠 Adaptive RAG (Retrieval-Augmented Generation)

This repository implements an **Adaptive RAG system** — a self-correcting, intelligent retrieval pipeline that goes beyond traditional RAG.
Unlike static pipelines, it validates, reroutes, and rewrites at every stage, ensuring **relevance, factuality, and completeness** in its answers.

---

## 🚀 Overview

Traditional RAG pipelines often stop at retrieval and generation — if the context is off, hallucinations or irrelevant answers follow.
**Adaptive RAG**, built with **LangGraph**, **Gemini**, and **Hugging Face**, adds intelligence between each step:

* Routes questions intelligently (Web Search vs. RAG)
* Grades document relevance
* Rewrites poor queries
* Detects hallucinations
* Validates answer sufficiency

It’s not just a retrieval pipeline — it’s a **reasoning loop**.

---

## ⚙️ Tech Stack

| Component      | Technology                                       |
| -------------- | ------------------------------------------------ |
| **LLMs**       | Google Gemini 2.5 Flash                          |
| **Vector DB**  | ChromaDB                                         |
| **Embeddings** | sentence-transformers/all-MiniLM-L6-v2           |
| **Frameworks** | LangChain, LangGraph                             |
| **Web Search** | Tavily API                                       |
| **Language**   | Python 3.13                                      |

---

## 📦 Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/fardeenalam/RAG_concepts.git
cd RAG_concepts
```

### 2. Install Dependencies

```bash
pip install -U langchain_community tiktoken langchain-google-genai langchain-huggingface langchainhub chromadb langchain langgraph tavily-python sentence-transformers
```

### 3. Set Environment Variables

Add your API keys before running:

```bash
export GEMINI_API_KEY="your_gemini_api_key"
export SECOND_GEMINI_API_KEY="your_backup_gemini_key"
export TAVILY_API_KEY="your_tavily_key"
```

Or set them interactively:

```python
import os
os.environ["GEMINI_API_KEY"] = "..."
os.environ["SECOND_GEMINI_API_KEY"] = "..."
os.environ["TAVILY_API_KEY"] = "..."
```

---

## 🧩 Architecture

### **System Flow**

```
START
  ↓
[Route Question] → Web Search OR Vectorstore
  ↓
[Grade Documents] → Are docs relevant?
  ↓
  NO → [Transform Query] → Retry Retrieval
  YES → [Generate Answer]
  ↓
[Hallucination Check] → Is answer grounded?
  ↓
  NO → Regenerate
  YES → [Answer Grader] → Does it resolve the question?
  ↓
  NO → Transform Query & Retry
  YES → END (Return Answer)
```

---

## 🔍 Core Components

### **1. Query Router**

Uses a Gemini-powered structured output model to decide whether to use:

* **Vectorstore Retrieval** (for topics like agents, prompt engineering, adversarial attacks)
* **Web Search** (for general or current topics)

### **2. Retrieval Grader**

Ensures retrieved documents are semantically relevant to the question.

### **3. Generation Module**

Creates grounded answers using the retrieved context.
Instructs the model to say *“I don’t know”* when context is insufficient.

### **4. Hallucination Grader**

Checks if the generated answer aligns with factual context from retrieved documents.

### **5. Answer Grader**

Determines if the generated answer truly resolves the user’s question.

### **6. Question Rewriter**

Rewrites vague or poorly phrased queries for better retrieval precision.

### **7. Web Search Tool**

Uses the **Tavily API** (1000 free credits) for real-time online information retrieval.

---

## 🧠 Graph Construction (LangGraph)

LangGraph’s **StateGraph** defines the pipeline’s logic and branching.

| Node              | Function                              |
| ----------------- | ------------------------------------- |
| `route_question`  | Chooses between RAG or Web Search     |
| `retrieve`        | Fetches documents from Chroma         |
| `grade_documents` | Validates retrieved docs              |
| `generate`        | Produces the response                 |
| `transform_query` | Rewrites question if needed           |
| `rate_answer`     | Evaluates hallucinations & usefulness |

Each node passes state variables (`question`, `generation`, `documents`, `retries`) dynamically.

---

## 🧪 Example Queries

### **1. Web Search Example**

**Input:**

> “Summarize Lionel Messi’s achievements during the 2022 FIFA World Cup.”

**Output:**

* Routes to **Web Search**
* Retrieves news sources via Tavily
* Generates verified summary

---

### **2. RAG Example**

**Input:**

> “What are the types of agent memory?”

**Output:**

* Routes to **Vectorstore**
* Retrieves relevant documents on agent memory
* Produces grounded technical explanation

---

### **3. Query Rewrite Example**

**Input:**

> “Can prompt engineering prevent hallucination?”

**Output:**

* Initial retrieval fails relevance
* Query rewritten to improve semantic matching
* Retrieves correct documents and generates validated answer

---

## 📁 Repository Structure

```
adaptive_rag/
│
├── adaptive_rag.ipynb        # Main implementation notebook
├── README.md                 # Documentation (this file)
├── requirements.txt          # Optional dependency list
└── data/                     # (Optional) Additional resources
```

---

## 🧰 Troubleshooting

**Common Issues**

| Error                                                            | Cause                       | Fix                                     |
| ---------------------------------------------------------------- | --------------------------- | --------------------------------------- |
| `HTTPError: 422`                                                 | Tavily API misconfigured    | Check API key and query format          |
| `RateLimitError`                                                 | Gemini API quota reached    | Use SECOND_GEMINI_API_KEY               |
| `AttributeError: 'tuple' object has no attribute 'page_content'` | Incorrect doc type returned | Ensure documents are `Document` objects |

---

## 📊 Future Enhancements

* [ ] Add retry limit to prevent infinite query rewrite loops
* [ ] Integrate cost tracking for LLM calls
* [ ] Replace Gemini with open models (e.g., Mistral or Llama 3) for full local execution
* [ ] Add memory persistence layer

---

## 📎 References

* [LangGraph Documentation](https://python.langchain.com/docs/langgraph/)
* [Tavily Search API](https://tavily.com)
* [LangChain Hub](https://smith.langchain.com/)
* [Sentence Transformers](https://www.sbert.net/)

---

## 💬 Author

**Fardeen Alam**
Exploring GenAI, Agentic Systems, and Python-driven Intelligent Backends.
🔗 [GitHub](https://github.com/fardeenalam) | [LinkedIn](https://linkedin.com/in/fardeen-alam)

