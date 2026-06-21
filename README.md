# Strategy-Action Alignment System

[![Python](https://img.shields.io/badge/Python-3.10+-blue)]()
[![Streamlit](https://img.shields.io/badge/Streamlit-App-red)]()
[![NLP](https://img.shields.io/badge/NLP-Semantic%20Search-green)]()
[![Vector Search](https://img.shields.io/badge/Vector%20Search-ChromaDB-purple)]()

## Overview

Strategy-Action Alignment System is an NLP and semantic search application that evaluates how well operational action plans align with long-term strategic objectives.

The system treats each strategic objective as a query and each action item as a retrievable document. It uses sentence embeddings, vector search, rule-based linkage boosting, and an interactive Streamlit dashboard to identify strong, partial, weak, and uncovered strategy-action relationships.

This project demonstrates how Information Retrieval, NLP, and lightweight RAG-style techniques can support strategic planning, governance, and decision-making.

## Live Demo

[Open the Streamlit App](https://strategy-action-mapper-comscds242p-014-hasitha.streamlit.app/)

## Problem Statement

Organizations often maintain long-term strategic plans and short-term yearly action plans as separate documents. Manually checking whether each action supports the correct strategic objective is time-consuming, subjective, and often dependent on keyword matching.

Keyword matching is limited because two related planning statements may use different words while still having the same meaning. For example, a strategy about improving research output may be supported by an action about grant application support, even if the wording is different.

This project solves the problem by converting strategy-action alignment into a semantic retrieval task.

## Key Features

- Parses strategic objectives and action plans from `.docx` documents
- Extracts explicit `Linked to:` relationships between actions and strategies
- Generates sentence embeddings using `all-MiniLM-L6-v2`
- Stores and retrieves embeddings using ChromaDB
- Applies ontology-based query expansion for domain-specific terms
- Calculates semantic similarity using cosine similarity
- Applies rule-based `linked_to` boosting for formally linked actions
- Classifies alignment into:
  - Strong
  - Partial
  - Weak
  - Not Covered (Year-1)
- Provides a Streamlit dashboard for interactive exploration
- Includes filters, similarity thresholds, drill-down views, and alignment tables
- Generates LLM-based improvement suggestions for weak or uncovered strategies
- Visualizes strategy-action-KPI relationships using a knowledge graph
- Generates stakeholder-friendly Markdown reports

## System Architecture

```text
Strategic Plan + Action Plan Documents
        ↓
Document Parsing and Structuring
        ↓
Strategy and Action Extraction
        ↓
Sentence Embedding Generation
        ↓
ChromaDB Vector Storage
        ↓
Ontology-Based Query Expansion
        ↓
Semantic Retrieval and Similarity Scoring
        ↓
Linked-to Boosting
        ↓
Alignment Classification
        ↓
Streamlit Dashboard + Knowledge Graph + Reports
```

## How It Works

### 1. Document Ingestion

The system reads strategic plan and action plan documents and extracts structured records such as:

- Strategy ID
- Strategy headline
- Full strategic objective
- Action ID
- Action title
- Action body
- Explicit `Linked to:` strategy references

The project processed 29 strategic objectives and 18 operational actions.

### 2. Semantic Embedding

Each strategy and action is converted into a dense vector representation using the `sentence-transformers/all-MiniLM-L6-v2` model.

This allows the system to compare meaning, not only exact keyword matches.

### 3. Vector Search

Embeddings are stored in ChromaDB using persistent local storage. For each strategic objective, the system retrieves the most semantically similar action items.

### 4. Ontology-Based Expansion

A lightweight ontology expands domain-specific planning terms.

Example:

```text
research output → publications, indexed journals, citations, h-index
accreditation → ABET, EUR-ACE, quality assurance
industry collaboration → partnerships, advisory boards, internships
```

This improves retrieval when strategies and actions use different vocabulary.

### 5. Linked-To Boosting

If an action is explicitly linked to a strategy in the source document, the system applies a similarity score boost.

Example:

```text
Original similarity: 0.594
Linked-to boost: +0.20
Boosted similarity: 0.794
```

This hybrid approach combines semantic similarity with formal organizational knowledge.

### 6. Alignment Classification

The final boosted similarity score is used to classify each strategy-action relationship.

```text
Strong      : >= 0.80
Partial     : >= 0.55
Weak        : < 0.55
Not Covered : No linked Year-1 action exists
```

The `Not Covered (Year-1)` label is important because some long-term strategies may not yet have supporting actions in the first-year plan.

## Dashboard

The Streamlit dashboard provides an interactive interface for exploring the alignment results.

Main dashboard capabilities include:

- Alignment summary counts
- Filterable alignment table
- Alignment label filters
- Mapping mode filters
- Similarity threshold slider
- Strategy-level drill-down
- Top-3 matching actions
- LLM-generated improvement suggestions
- KPI suggestions
- Iteration history
- Knowledge graph visualization
- Executive Markdown report generation

## LLM-Based Improvement Generator

For weak or uncovered strategies, the system uses a local LLM through Ollama to generate improvement suggestions.

The improvement workflow follows an agentic loop:

```text
Identify weak strategy
        ↓
Retrieve top matching actions
        ↓
Generate improved strategy / KPIs
        ↓
Recalculate similarity
        ↓
Refine if target score is not reached
        ↓
Return best improvement
```

This turns the system from a passive evaluator into an active decision-support tool.

The LLM output can include:

- Revised strategy wording
- Rationale for the recommendation
- Suggested KPIs
- Targets and timeframes
- Evidence based on retrieved action IDs

## Knowledge Graph View

The system includes a knowledge graph view to improve explainability.

The graph represents relationships such as:

```text
Strategy → Action → KPI
```

This helps users understand:

- Which actions support each strategy
- Which KPIs are connected to improvement suggestions
- Which strategies have weak or missing operational coverage
- How recommendations are grounded in retrieved evidence

## Automated Report Generation

The system can generate a Markdown report for stakeholder use.

The report includes:

- Executive summary
- Alignment statistics
- Key findings
- Weak or uncovered strategies
- Improvement recommendations
- Appendix-style supporting details

Markdown was used because it is easy to version-control, share, and convert to other formats.

## Tech Stack

- Python
- Streamlit
- Pandas
- Sentence Transformers
- all-MiniLM-L6-v2
- ChromaDB
- Cosine Similarity
- Graphviz
- Ollama
- qwen2.5:7b
- Markdown report generation

## Repository Structure

```text
strategy-action-alignment-system/
│
├── data/
│   ├── ACTION-PLAN-2024.docx
│   └── STRATEGIC-PLAN-2024.docx
│
├── outputs/
│   ├── alignment_table.csv
│   ├── improvements_latest.json
│   ├── improvements_history.jsonl
│   └── generated_reports.md
│
├── src/
│   ├── ingest.py
│   ├── embeddings.py
│   ├── retrieve.py
│   ├── ontology.py
│   ├── align.py
│   ├── pipeline_score.py
│   ├── improve.py
│   ├── llm_ollama.py
│   └── report.py
│
├── app.py
├── requirements.txt
├── .gitignore
└── README.md
```

## How to Run Locally

Clone the repository:

```bash
git clone https://github.com/hasithakarrunarathne-AI/strategy-action-alignment-system.git
cd strategy-action-alignment-system
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the Streamlit app:

```bash
streamlit run app.py
```

Optional: If using the local LLM improvement feature, install and run Ollama first.

```bash
ollama run qwen2.5:7b
```

## Main Modules

### `ingest.py`

Extracts structured strategies and actions from `.docx` documents using regular expression parsing.

### `embeddings.py`

Generates sentence embeddings and stores them in ChromaDB.

### `retrieve.py`

Retrieves the most relevant actions for each strategy using vector search.

### `ontology.py`

Expands domain-specific terms to improve semantic retrieval.

### `pipeline_score.py`

Calculates boosted similarity scores and generates the final alignment table.

### `improve.py`

Generates LLM-based improvement suggestions for weak or uncovered strategies.

### `llm_ollama.py`

Handles local LLM communication through Ollama.

### `report.py`

Generates Markdown reports for stakeholders.

### `app.py`

Runs the Streamlit dashboard.

## Example Alignment Logic

For each strategy, the system:

1. Retrieves top candidate actions using semantic similarity
2. Checks whether the action is explicitly linked to the strategy
3. Adds a `linked_to` boost if applicable
4. Selects the best matching action
5. Stores the top-3 matching actions for transparency
6. Classifies the alignment strength

Example output fields include:

- `strategy_id`
- `strategy_headline`
- `label`
- `mapping_mode`
- `best_action_id`
- `best_action_headline`
- `best_similarity`
- `best_boosted_similarity`
- `top1_action_id`
- `top2_action_id`
- `top3_action_id`

## Business Value

This system can help organizations:

- Check whether yearly actions support long-term strategic goals
- Detect weakly aligned or uncovered strategies
- Reduce manual review effort
- Improve transparency in strategic planning
- Support governance and planning reviews
- Generate evidence-based improvement suggestions
- Create executive-level reports for stakeholders

## Skills Demonstrated

- Information Retrieval
- NLP and semantic similarity
- Sentence embeddings
- Vector databases
- RAG-style architecture
- Python application development
- Streamlit dashboard development
- LLM integration
- Knowledge graph visualization
- Data-driven decision support
- Report automation
- Business problem framing

## Limitations

- Current parsing depends on consistent `.docx` formatting
- Ontology terms are manually curated
- LLM-generated suggestions require human review before real-world use
- The system was tested on a controlled strategic planning dataset
- Cloud deployment may require disabling or replacing local Ollama features

## Future Improvements

- Add PDF and Word upload support through the dashboard
- Improve parser robustness for different document formats
- Expand the ontology automatically
- Add user authentication
- Add role-based reporting for management users
- Support multiple organizations and planning cycles
- Replace local LLM with configurable cloud or local model options
- Add downloadable PDF reports
- Deploy with a cleaner production Streamlit URL

## Author

**Hasitha Karrunarathne**  
MSc Data Science

Focus areas: Data Science, Data Engineering, NLP, Information Retrieval, ML Systems, Analytics, and AI-enabled decision support.
