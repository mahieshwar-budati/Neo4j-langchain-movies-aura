# Neo4j Aura Graph Visualization with PyVis (Google Colab)

A minimal, reproducible notebook that connects to a **Neo4j AuraDB** instance from **Google Colab**, pulls a subgraph via Cypher, and renders it as an interactive HTML graph using **PyVis**.

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Neo4j](https://img.shields.io/badge/Neo4j-AuraDB-008CC1)
![PyVis](https://img.shields.io/badge/PyVis-Network%20Viz-orange)
![Platform](https://img.shields.io/badge/Runs%20on-Google%20Colab-yellow)

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Connecting to Neo4j Aura](#connecting-to-neo4j-aura)
- [Fetching & Visualizing the Graph](#fetching--visualizing-the-graph)
- [Full Code](#full-code)
- [Output](#output)
- [Known Warnings](#known-warnings)
- [Security Notes](#security-notes)
- [Next Steps](#next-steps)

---

## Overview

This notebook demonstrates an end-to-end workflow for:

1. Installing the required Python packages in Colab.
2. Authenticating against a **Neo4j AuraDB** cloud instance.
3. Running a Cypher query to retrieve nodes and relationships.
4. Converting the result set into a **PyVis** network graph.
5. Rendering the graph inline as an interactive HTML visualization.

---

## Prerequisites

| Requirement | Details |
|---|---|
| Neo4j AuraDB instance | A running Aura Free/Pro instance with a valid connection URI |
| Credentials | Username (default `neo4j`) and password generated when the instance was created |
| Environment | Google Colab (or any Jupyter-compatible environment) |
| Python | 3.x (Colab default runtime) |

---

## Installation

Install PyVis for graph rendering:

```python
!pip install -q pyvis
```

Install LangChain, LangChain-Community, LangChain-Groq, and the Neo4j Python driver:

```python
!pip install --upgrade --quiet langchain langchain-community langchain-groq neo4j
```

Verify PyVis is available:

```python
from pyvis.network import Network
from IPython.display import HTML, display

print("✅ PyVis installed successfully!")
```

---

## Connecting to Neo4j Aura

```python
from neo4j import GraphDatabase

URI = "neo4j+s://<your-instance-id>.databases.neo4j.io"
USERNAME = "neo4j"
PASSWORD = "<your-password>"

driver = GraphDatabase.driver(
    URI,
    auth=(USERNAME, PASSWORD)
)

driver.verify_connectivity()
print("✅ Connected successfully!")
```

> **Tip:** Never hardcode credentials directly in a notebook you intend to share. See [Security Notes](#security-notes) below.

---

## Fetching & Visualizing the Graph

The workflow below runs a Cypher query, maps each returned node/relationship into a PyVis-compatible structure, and renders the result as an interactive HTML canvas.

**Cypher query used:**

```cypher
MATCH (n)-[r]->(m)
RETURN n, r, m
LIMIT 50
```

**High-level steps performed in code:**

1. Initialize a PyVis `Network` object (directed, notebook-rendered, inline CDN resources).
2. Open a Neo4j session and execute the query.
3. For each `(n)-[r]->(m)` record:
   - Extract unique Neo4j `element_id`s for both nodes.
   - Convert node properties to Python dictionaries.
   - Resolve each node's label (falling back to `"Node"` if unlabeled).
   - Build a human-readable display string from `name` → `title` → label type.
   - Add both nodes and the connecting edge to the PyVis graph, using the label as the group (for color-coding).
4. Enable the physics control panel via `show_buttons`.
5. Save and display the graph as an inline HTML file.

---

## Full Code

```python
from pyvis.network import Network
from IPython.display import HTML, display

query = """
MATCH (n)-[r]->(m)
RETURN n, r, m
LIMIT 50
"""

net = Network(
    height="850px",
    width="100%",
    directed=True,
    notebook=True,
    cdn_resources="in_line"
)

with driver.session(database="neo4j") as session:
    result = session.run(query)

    for record in result:
        n = record["n"]
        r = record["r"]
        m = record["m"]

        # Unique Neo4j node IDs
        n_id = n.element_id
        m_id = m.element_id

        # Convert properties to dictionaries
        n_props = dict(n)
        m_props = dict(m)

        # Get node labels
        n_label_type = next(iter(n.labels), "Node")
        m_label_type = next(iter(m.labels), "Node")

        # Choose readable display text
        n_text = (
            n_props.get("name")
            or n_props.get("title")
            or n_label_type
        )

        m_text = (
            m_props.get("name")
            or m_props.get("title")
            or m_label_type
        )

        # Add first node
        net.add_node(
            n_id,
            label=str(n_text),
            title=f"{n_label_type}<br>{n_props}",
            group=n_label_type
        )

        # Add second node
        net.add_node(
            m_id,
            label=str(m_text),
            title=f"{m_label_type}<br>{m_props}",
            group=m_label_type
        )

        # Add relationship
        net.add_edge(
            n_id,
            m_id,
            label=r.type,
            title=r.type
        )

net.show_buttons(filter_=["physics"])

html_file = "/content/neo4j_graph.html"
net.save_graph(html_file)

display(HTML(filename=html_file))
```

---

## Output

Running the notebook renders an interactive force-directed graph inline in Colab, with nodes color-coded by label group and edges labeled by relationship type. A physics control panel is available at the bottom of the canvas to fine-tune layout behavior in real time.

The rendered HTML file is also saved locally at:

```
/content/neo4j_graph.html
```

---

## Known Warnings

During installation, pip may report a dependency resolver conflict:

```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed.
google-colab 1.0.0 requires requests==2.32.4, but you have requests 2.34.2 which is incompatible.
```

This is a **non-blocking warning** specific to Colab's pre-installed `google-colab` package and does not affect the Neo4j/PyVis workflow. It can generally be safely ignored unless you rely on `google-colab`-specific functionality elsewhere in the same runtime.

---

## Security Notes

- The AuraDB **URI** and **password** shown in the source screenshots have been redacted/masked in this documentation.
- Avoid committing real credentials to notebooks, version control, or shared documentation.
- Recommended: store credentials as Colab secrets or environment variables instead of inline strings:

```python
import os
URI = os.environ["NEO4J_URI"]
USERNAME = os.environ["NEO4J_USERNAME"]
PASSWORD = os.environ["NEO4J_PASSWORD"]
```

---

## Next Steps

- Parameterize the Cypher query (node labels, relationship types, `LIMIT`) via notebook widgets.
- Add legend/color-key generation based on distinct `group` values.
- Extend the pipeline with `langchain-groq` to enable natural-language-to-Cypher querying.
- Export the visualization as a standalone shareable HTML report.

---

*Documentation generated from a Google Colab notebook practicing Neo4j Aura connectivity and PyVis graph visualization.*
