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