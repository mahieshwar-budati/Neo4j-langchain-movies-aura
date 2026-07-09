# Neo4j Graph Visualization using Python

## Overview

This project demonstrates how to connect to a Neo4j graph database, retrieve nodes and relationships using Cypher queries, and visualize the graph using Python.

The application connects to a Neo4j database, extracts graph data, converts it into a NetworkX graph, and generates clear visualizations using Matplotlib. It also includes an enhanced visualization with color-coded node labels for improved readability.

---

## Features

- Connect to a Neo4j database using the official Python driver
- Retrieve graph data using Cypher queries
- Convert Neo4j relationships into a NetworkX directed graph
- Visualize graph structures with Matplotlib
- Display relationship types as edge labels
- Generate improved visualizations with color-coded node categories

---

## Technologies Used

- **Python 3.x**
- **Neo4j**
- **Neo4j Python Driver**
- **NetworkX**
- **Matplotlib**

---

## Installation

Install the required Python packages before running the project.

```bash
pip install neo4j networkx matplotlib
```

---

## Project Workflow

### 1. Connect to Neo4j

The application establishes a connection to the Neo4j database using the database URI, username, and password.

```python
GraphDatabase.driver(uri, auth=(username, password))
```

After a successful connection, the application confirms the connection with a status message.

---

### 2. Fetch Graph Data

Graph data is retrieved using the following Cypher query:

```cypher
MATCH (n)-[r]->(m)
RETURN
    id(n) AS source_id,
    labels(n)[0] AS source_label,
    type(r) AS relationship,
    id(m) AS target_id,
    labels(m)[0] AS target_label
```

This query returns:

- Source node ID
- Source node label
- Relationship type
- Target node ID
- Target node label

The retrieved records are processed by the `fetch_graph_data()` function.

---

### 3. Build the Graph

A directed graph is created using NetworkX.

```python
G = nx.DiGraph()
```

For each record:

- A source node is added
- A target node is added
- A directed edge is created
- The relationship type is stored as the edge label

This converts the Neo4j graph into a Python graph structure.

---

### 4. Basic Graph Visualization

The graph is visualized using Matplotlib.

Features include:

- Nodes representing graph entities
- Directed edges representing relationships
- Relationship names displayed as edge labels
- Automatic node positioning using `spring_layout`

---

### 5. Enhanced Graph Visualization

The improved visualization provides better readability by:

- Assigning unique colors to different node labels
- Increasing node size
- Improving node spacing
- Displaying relationship labels more clearly
- Automatically generating colors for each node category

This makes larger graph structures easier to interpret.

---

## Project Structure

```
project/
│
├── graph_visualization.py
├── README.md
└── requirements.txt
```

---

## Output

The program generates two graph visualizations:

### Basic Visualization

- Displays all nodes and relationships
- Uses a standard layout for quick inspection

### Enhanced Visualization

- Color-coded node categories
- Larger nodes for better visibility
- Improved spacing
- Clearly labeled relationships

---

## Requirements

- Python 3.8 or later
- Neo4j Database (Local or Neo4j Aura)
- Internet connection (for Neo4j Aura)

---

## How to Run

1. Clone the repository.

```bash
git clone <repository-url>
```

2. Navigate to the project directory.

```bash
cd <repository-name>
```

3. Install the required dependencies.

```bash
pip install -r requirements.txt
```

4. Update the Neo4j connection credentials in the Python file.

```python
URI = "your_neo4j_uri"
USERNAME = "your_username"
PASSWORD = "your_password"
```

5. Run the application.

```bash
python graph_visualization.py
```

---

## Results

This project demonstrates how to:

- Connect to a Neo4j graph database
- Execute Cypher queries
- Retrieve nodes and relationships
- Convert graph data into a NetworkX graph
- Visualize graph structures using Matplotlib
- Create enhanced visualizations with categorized node coloring

---

## Future Improvements

- Interactive graph visualization using Plotly or PyVis
- Filter graphs based on node labels or relationship types
- Export graph visualizations as PNG or PDF
- Add support for large graph datasets
- Integrate graph analytics (centrality, clustering, shortest path)

---

## License

This project is intended for educational and learning purposes.
