# 🧠 Natural Language to Cypher Graph QA System

An AI-powered **Graph Question Answering System** that converts natural-language questions into **Cypher queries**, executes them against a **Neo4j graph database**, and returns human-readable answers.

This project integrates **Neo4j**, **LangChain**, **Groq LLM**, and **Llama 3.3 70B** to demonstrate how Large Language Models can interact with structured graph data.

---

## 📌 Project Overview

Traditional graph databases require users to understand and write Cypher queries manually.

This project enables users to ask questions in plain English, such as:

```text
Who was the director of the movie GoldenEye?
```

The system automatically:

1. Accepts the natural-language question
2. Understands the graph database schema
3. Generates a Cypher query using an LLM
4. Executes the query against Neo4j
5. Retrieves relevant graph data
6. Returns a natural-language answer

### Example Flow

```text
User Question
      ↓
LangChain GraphCypherQAChain
      ↓
Groq LLM — Llama 3.3 70B
      ↓
Generated Cypher Query
      ↓
Neo4j Graph Database
      ↓
Query Results
      ↓
Natural-Language Answer
```

---

## ✨ Key Features

* 🗣️ Natural Language to Cypher query generation
* 🧠 LLM-powered graph reasoning
* 🕸️ Neo4j graph database integration
* ⚡ Fast LLM inference through Groq
* 🎬 Movie knowledge graph creation
* 📄 Text-to-graph transformation
* 🔗 Automatic entity and relationship extraction
* ❓ Natural-language question answering
* 📊 Graph schema inspection
* 🧪 Example Cypher query patterns

---

## 🛠️ Technology Stack

| Technology                      | Purpose                                          |
| ------------------------------- | ------------------------------------------------ |
| Python                          | Core programming language                        |
| Jupyter Notebook / Google Colab | Interactive development environment              |
| Neo4j Aura                      | Cloud graph database                             |
| LangChain                       | LLM application framework                        |
| LangChain Community             | Neo4j graph integration                          |
| LangChain Experimental          | LLM graph transformation                         |
| LangChain Groq                  | Groq LLM integration                             |
| Groq API                        | High-speed LLM inference                         |
| Llama 3.3 70B                   | Natural-language reasoning and Cypher generation |
| Cypher                          | Neo4j graph query language                       |

---

## 🏗️ System Architecture

```text
┌──────────────────────────────┐
│          User Question       │
│      Natural Language        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      GraphCypherQAChain      │
│          LangChain           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│          Groq LLM            │
│    Llama 3.3 70B Versatile   │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│    Generated Cypher Query    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Neo4j Graph Database    │
│  Movies • People • Genres    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       Retrieved Context      │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│    Natural-Language Answer   │
└──────────────────────────────┘
```

---

## 📂 Recommended Project Structure

```text
natural-language-to-cypher/
│
├── notebooks/
│   └── neo4j_graph_qa.ipynb
│
├── .env.example
├── .gitignore
├── requirements.txt
├── README.md
└── LICENSE
```

---

## 🕸️ Graph Data Model

The movie knowledge graph contains three primary node types:

```text
(:Movie)
(:Person)
(:Genre)
```

### Relationships

```text
(:Person)-[:DIRECTED]->(:Movie)

(:Person)-[:ACTED_IN]->(:Movie)

(:Movie)-[:IN_GENRE]->(:Genre)
```

### Visual Representation

```text
                    ┌─────────────┐
                    │   Person    │
                    └──────┬──────┘
                           │
              ┌────────────┴────────────┐
              │                         │
         :DIRECTED                 :ACTED_IN
              │                         │
              ▼                         ▼
         ┌───────────────────────────────┐
         │             Movie             │
         └──────────────┬────────────────┘
                        │
                    :IN_GENRE
                        │
                        ▼
                 ┌─────────────┐
                 │    Genre    │
                 └─────────────┘
```

---

## 📊 Movie Node Properties

Each movie contains properties such as:

```text
Movie
├── id
├── title
├── released
└── imdbRating
```

Example:

```cypher
(:Movie {
    id: "1",
    title: "GoldenEye",
    released: date("1995-11-17"),
    imdbRating: 7.2
})
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd natural-language-to-cypher
```

---

### 2. Create a Virtual Environment

#### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

#### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Alternatively:

```bash
pip install langchain \
            langchain-community \
            langchain-groq \
            langchain-experimental \
            neo4j \
            groq \
            python-dotenv
```

---

## 📦 requirements.txt

Create a `requirements.txt` file:

```text
langchain
langchain-community
langchain-groq
langchain-experimental
neo4j
groq
python-dotenv
jupyter
```

For production projects, pin tested dependency versions before release.

---

## 🔐 Environment Configuration

Create a `.env` file in the project root:

```env
NEO4J_URI=neo4j+s://your-instance.databases.neo4j.io
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_neo4j_password

GROQ_API_KEY=your_groq_api_key
```

Load the variables in Python:

```python
import os
from dotenv import load_dotenv

load_dotenv()

NEO4J_URI = os.getenv("NEO4J_URI")
NEO4J_USERNAME = os.getenv("NEO4J_USERNAME")
NEO4J_PASSWORD = os.getenv("NEO4J_PASSWORD")
GROQ_API_KEY = os.getenv("GROQ_API_KEY")
```

> ⚠️ Never commit `.env`, API keys, database passwords, or other credentials to GitHub.

---

## 🛡️ Recommended .gitignore

Create a `.gitignore` file:

```gitignore
# Environment variables
.env
.env.*

# Python
__pycache__/
*.py[cod]
*.pyo
*.pyd

# Virtual environments
venv/
.venv/
env/

# Jupyter
.ipynb_checkpoints/

# IDE
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db
```

---

## 🔌 Connecting to Neo4j

The project connects to Neo4j using LangChain's graph integration:

```python
from langchain_community.graphs import Neo4jGraph

graph = Neo4jGraph(
    url=NEO4J_URI,
    username=NEO4J_USERNAME,
    password=NEO4J_PASSWORD
)
```

---

## 🤖 Configuring the LLM

The project uses Groq with the Llama 3.3 70B model:

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    groq_api_key=GROQ_API_KEY,
    model_name="llama-3.3-70b-versatile"
)
```

---

## 🧩 Text-to-Graph Transformation

The project also demonstrates automatic knowledge graph extraction from unstructured text.

```python
from langchain_core.documents import Document

documents = [
    Document(
        page_content="Your source text here..."
    )
]
```

The LLM Graph Transformer converts the text into graph structures:

```python
from langchain_experimental.graph_transformers import LLMGraphTransformer

llm_transformer = LLMGraphTransformer(llm=llm)

graph_documents = (
    llm_transformer
    .convert_to_graph_documents(documents)
)
```

The generated graph documents can contain:

* Nodes
* Entities
* Relationships
* Semantic connections

---

## 🎬 Loading the Movie Dataset

The project imports a public movie dataset directly into Neo4j:

```cypher
LOAD CSV WITH HEADERS FROM
'https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/movies/movies_small.csv'
AS row

MERGE (m:Movie {id: row.movieId})

SET m.released = date(row.released),
    m.title = row.title,
    m.imdbRating = toFloat(row.imdbRating)

FOREACH (director IN split(row.director, '|') |
    MERGE (p:Person {name: trim(director)})
    MERGE (p)-[:DIRECTED]->(m)
)

FOREACH (actor IN split(row.actors, '|') |
    MERGE (p:Person {name: trim(actor)})
    MERGE (p)-[:ACTED_IN]->(m)
)

FOREACH (genre IN split(row.genres, '|') |
    MERGE (g:Genre {name: trim(genre)})
    MERGE (m)-[:IN_GENRE]->(g)
)
```

This creates a connected movie knowledge graph containing:

* Movies
* Actors
* Directors
* Genres
* IMDb ratings
* Release dates

---

## 🔄 Refreshing the Graph Schema

After loading data, refresh the schema:

```python
graph.refresh_schema()

print(graph.schema)
```

This allows the LLM to understand:

* Available node labels
* Relationship types
* Node properties
* Graph structure

---

## 💬 Creating the Graph QA Chain

```python
from langchain_community.chains.graph_qa.cypher import GraphCypherQAChain

chain = GraphCypherQAChain.from_llm(
    llm=llm,
    graph=graph,
    verbose=True,
    allow_dangerous_requests=True
)
```

> ⚠️ `allow_dangerous_requests=True` should only be used in trusted environments. LLM-generated database queries must be handled carefully in production.

---

## 🧪 Example Questions

### Find a Movie Director

```python
response = chain.invoke({
    "query": "Who was the director of the movie GoldenEye?"
})

print(response)
```

---

### Find Movie Genres

```python
response = chain.invoke({
    "query": "Tell me the genre of the movie GoldenEye."
})

print(response)
```

---

### Find Genres of Braveheart

```python
response = chain.invoke({
    "query": "What are the genres of the movie Braveheart?"
})

print(response)
```

---

### Find the Director of Casino

```python
response = chain.invoke({
    "query": "Who was the director of the movie Casino?"
})

print(response)
```

---

### Find Movies Released in 2008

```python
response = chain.invoke({
    "query": "Which movies were released in 2008?"
})

print(response)
```

---

### Find Highly Rated Movies

```python
response = chain.invoke({
    "query": "Give me the list of movies having an IMDb rating greater than 8."
})

print(response)
```

---

## 🧠 Example Cypher Queries

### Count Unique Actors

```cypher
MATCH (a:Person)-[:ACTED_IN]->(:Movie)
RETURN count(DISTINCT a)
```

### Find Actors in Casino

```cypher
MATCH (m:Movie {title: 'Casino'})<-[:ACTED_IN]-(a:Person)
RETURN a.name
```

### Count Tom Hanks Movies

```cypher
MATCH (a:Person {name: 'Tom Hanks'})-[:ACTED_IN]->(m:Movie)
RETURN count(m)
```

### Find Genres of Schindler's List

```cypher
MATCH (m:Movie {title: "Schindler's List"})-[:IN_GENRE]->(g:Genre)
RETURN g.name
```

### Find Actors in Both Comedy and Action Movies

```cypher
MATCH (a:Person)-[:ACTED_IN]->(:Movie)-[:IN_GENRE]->(g1:Genre),
      (a)-[:ACTED_IN]->(:Movie)-[:IN_GENRE]->(g2:Genre)
WHERE g1.name = 'Comedy'
  AND g2.name = 'Action'
RETURN DISTINCT a.name
```

### Find Directors Who Also Acted in Their Movies

```cypher
MATCH (p:Person)-[:DIRECTED]->(m:Movie),
      (p)-[:ACTED_IN]->(m)
RETURN m.title, p.name
```

### Find the Most Prolific Actor

```cypher
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
RETURN a.name, COUNT(m) AS movieCount
ORDER BY movieCount DESC
LIMIT 1
```

---

## 🔍 How It Works

### Step 1 — User Asks a Question

```text
Who was the director of the movie GoldenEye?
```

### Step 2 — LLM Understands the Request

The model identifies:

```text
Entity: GoldenEye
Target: Director
```

### Step 3 — Cypher Query Is Generated

Conceptually:

```cypher
MATCH (p:Person)-[:DIRECTED]->(m:Movie)
WHERE m.title = 'GoldenEye'
RETURN p.name
```

### Step 4 — Neo4j Executes the Query

The graph database retrieves the matching connected node.

### Step 5 — The System Returns an Answer

```text
The director of GoldenEye was Martin Campbell.
```

---

## 🔒 Security Best Practices

Before publishing this project:

1. Never hardcode API keys in notebooks
2. Never commit Neo4j passwords
3. Store secrets in `.env`
4. Add `.env` to `.gitignore`
5. Rotate any credentials that were previously exposed
6. Clear notebook outputs before committing
7. Use a read-only Neo4j account where possible
8. Validate LLM-generated Cypher queries in production
9. Avoid unrestricted write permissions for AI-generated queries

---

## ⚠️ Known Limitations

* Generated Cypher queries may occasionally be incorrect
* Results depend on the graph schema and available data
* Natural-language ambiguity can affect query generation
* LLM responses may require validation
* Production deployments need stronger query restrictions
* Schema changes require refreshing the graph schema

---

## 🔮 Future Enhancements

* [ ] Add Streamlit web interface
* [ ] Add FastAPI backend
* [ ] Implement conversation memory
* [ ] Add few-shot Cypher examples
* [ ] Add query validation layer
* [ ] Add read-only database access
* [ ] Add Docker support
* [ ] Add automated testing
* [ ] Add graph visualization
* [ ] Add query history
* [ ] Add structured logging
* [ ] Support multiple graph datasets
* [ ] Add production-ready authentication
* [ ] Implement advanced Graph RAG retrieval

---

## 🎯 Use Cases

This architecture can be adapted for:

* 🎬 Movie recommendation systems
* 🏦 Banking knowledge graphs
* 🛒 E-commerce product graphs
* 🏥 Healthcare knowledge systems
* 📚 Research knowledge graphs
* 👥 Social network analysis
* 🏢 Enterprise knowledge management
* 🔐 Fraud detection systems
* 📖 Document relationship extraction

---

## 🤝 Contributing

Contributions are welcome.

To contribute:

```bash
git checkout -b feature/your-feature-name
```

Make your changes, commit them, and open a pull request.

---

## 📄 License

This project is intended for educational and research purposes.

Consider adding an MIT License if you plan to distribute or reuse the project publicly.

---

## 👨‍💻 Author

**Vishnu N**

B.Tech Information Technology Graduate
AI • Graph Databases • LangChain • Generative AI

---

## ⭐ Support

If you find this project useful:

* ⭐ Star the repository
* 🍴 Fork the project
* 🐛 Report issues
* 🤝 Contribute improvements

---

## 📌 Project Summary

This project demonstrates how **Large Language Models**, **graph databases**, and **natural-language interfaces** can work together to make structured knowledge accessible without requiring users to manually write Cypher queries.

By combining Neo4j, LangChain, Groq, and Llama 3.3 70B, the system provides an intelligent interface for exploring connected graph data through simple natural-language questions.
