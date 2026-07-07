# 🧠 Natural Language to Cypher Graph QA System

An AI-powered **Graph Question Answering System** developed and executed in **Google Colab** that converts natural-language questions into **Cypher queries**, runs them against a **Neo4j Aura graph database**, and returns human-readable answers.

The project combines **LangChain**, **Neo4j Aura**, **Groq**, and **Llama 3.3 70B** to create an intelligent natural-language interface for graph databases.

## 🚀 Run in Google Colab

This project is designed to run directly in Google Colab, so no local Python setup or virtual environment is required.

### Step 1 — Open the Notebook

Upload the `.ipynb` notebook to Google Colab or open it directly from the GitHub repository.

### Step 2 — Install Dependencies

Run the dependency installation cell:

```python
!pip install -qU \
    langchain \
    langchain-community \
    langchain-groq \
    langchain-experimental \
    neo4j
```

### Step 3 — Configure Credentials

Provide the required environment variables securely:

```python
import os

os.environ["NEO4J_URI"] = "your_neo4j_aura_uri"
os.environ["NEO4J_USERNAME"] = "neo4j"
os.environ["NEO4J_PASSWORD"] = "your_neo4j_password"
os.environ["GROQ_API_KEY"] = "your_groq_api_key"
```


### Step 4 — Connect to Neo4j Aura

```python
from langchain_community.graphs import Neo4jGraph

graph = Neo4jGraph(
    url=os.environ["NEO4J_URI"],
    username=os.environ["NEO4J_USERNAME"],
    password=os.environ["NEO4J_PASSWORD"]
)
```

### Step 5 — Run the Notebook

Execute the notebook cells sequentially from top to bottom.

The notebook will:

1. Install required Python packages
2. Connect to Neo4j Aura
3. Configure the Groq-powered LLM
4. Create or load graph data
5. Inspect the graph schema
6. Generate Cypher queries from natural language
7. Execute generated queries
8. Return natural-language answers
