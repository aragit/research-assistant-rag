# 🔬 Research Assistant RAG

This project is a fully local, chat-based Research Assistant that uses Retrieval Augmented Generation (RAG) to answer questions about scientific papers and other documents in PDF format. It leverages a local LLM and a vector database, ensuring your data remains private.

![Screenshot of the Research Assistant UI](https://media.licdn.com/dms/image/v2/D4D22AQEuR6oSoEUedA/feedshare-shrink_1280/B4DZfNzyaXH4Ao-/0/1751504595899?e=1755734400&v=beta&t=GcJOC2x73wGgTmQ7M9pwgIjedT8Kl2rtIrkrjQVzRwE)

 We used Naive RAG for this project . Try to research other pattern too. 
---

#### Features

-   **📄 PDF Document Processing**: Ingests and processes multiple PDF files from a local directory.
-   **🧠 Local LLM**: Uses the CPU-optimized `Gemma-2-2B-IT` model in GGUF format, running entirely on your machine. No API keys needed.
-   **💾 Efficient Indexing**: Creates and caches a [FAISS](https://faiss.ai/) vector index for fast and efficient document retrieval.
-   **💬 Interactive UI**: A simple and clean chat interface built with [Gradio](https://www.gradio.app/).
-   **✅ Source Citations**: Each answer includes citations pointing to the source document and page number.
-   **⚙️ Cross-Platform**: All file paths are relative, making the project portable across different operating systems.

---

#### How It Works

The application follows a standard RAG pipeline, divided into two main stages:

**1. Ingestion & Indexing (Offline)**
This happens the first time you run the app.
-   **Load**: PDF documents in the `/docs` folder are loaded using `PyPDFLoader`.
-   **Clean**: Text is cleaned to remove non-ASCII characters and extra whitespace.
-   **Split**: The documents are split into smaller, overlapping chunks using `RecursiveCharacterTextSplitter`.
-   **Embed & Store**: Each chunk is converted into a vector embedding using `all-MiniLM-L6-v2` and stored in a FAISS vector index. This index is saved to the `/faiss_index` directory so it doesn't need to be recreated on subsequent runs.

**2. Retrieval & Generation (Online)**
This happens every time you ask a question.
-   **Retrieve**: Your question is embedded, and the FAISS index is searched to find the most relevant document chunks (the "context").
-   **Augment**: The retrieved context and your original question are inserted into a prompt template.
-   **Generate**: The complete prompt is sent to the local Gemma-2 LLM, which generates a concise answer based *only* on the provided context.

---

### Setup and Usage

Follow these steps to get the application running on your local machine.

**Prerequisites:**
-   Python 3.10 or newer
-   Git

**1. Clone the Repository**
```bash
git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)
cd YOUR_REPOSITORY_NAME
```

**2. Set Up Project Structure**
Create the necessary folders for your documents and the LLM model.
```bash
mkdir docs
mkdir models
```

**3. Add Your Files**
-   Place your research papers (`.pdf` files) inside the newly created `docs` folder.
-   Download the [Gemma-2-2B-IT GGUF model](https://huggingface.co/lmstudio-ai/gemma-2-2b-it-GGUF) (we recommend the `Q4_K_M` version for a good balance of quality and performance) and place the `.gguf` file inside the `models` folder.

**4. Install Dependencies**
It is highly recommended to use a virtual environment.
```bash
# Create a virtual environment
python -m venv venv

# Activate it
# On Windows: venv\Scripts\activate
# On macOS/Linux: source venv/bin/activate

# Install all required packages
pip install -r requirements.txt
```

**5. Run the Application**
```bash
python app.py
```
The script will first create the FAISS index (this may take a few minutes depending on the number of documents). Once complete, it will launch a Gradio web server. Open the local URL provided in your terminal (e.g., `http://127.0.0.1:7860`) to start chatting with your documents!

---

#### Technology Stack
##### Naive RAG:

-   **Backend Framework**: [LangChain](https://www.langchain.com/)
-   **LLM**: [Google Gemma-2](https://huggingface.co/google/gemma-2-2b-it) (via [Llama.cpp](https://github.com/ggerganov/llama.cpp))
-   **Embedding Model**: [Sentence-Transformers](https://www.sbert.net/)
-   **Vector Store**: [Facebook AI Similarity Search (FAISS)](https://faiss.ai/)
-   **UI**: [Gradio](https://www.gradio.app/)
