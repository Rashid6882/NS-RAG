# NS-RAG: Neuro-Symbolic Retrieval-Augmented Generation for Interactive Narrative Generation

A Neuro-Symbolic Retrieval-Augmented Generation (NS-RAG) framework for interactive storytelling that combines Large Language Models (LLMs), Knowledge Graphs, Hybrid Retrieval, and Rule-Based Verification to generate coherent and logically consistent narratives.

---

## Overview

Traditional LLM-based storytelling systems often suffer from:

- Character inconsistency (dead characters acting again)
- Inventory contradictions
- Location inconsistencies
- Timeline violations
- Hallucinated entities
- Loss of long-term memory

NS-RAG addresses these challenges through a hybrid architecture that combines semantic retrieval, symbolic reasoning, and automated self-correction.

---

## Key Features

### Hybrid Retrieval
- FAISS-based semantic retrieval for narrative memory
- Knowledge Graph retrieval for structured world-state facts
- Character state retrieval
- World-state snapshots
- Generation constraints

### Dynamic Knowledge Graph
Maintains structured facts about:
- Characters
- Locations
- Inventory
- Relationships
- Events
- World State

Example:

```text
Arthur → isAlive → True
Arthur → owns → Sword
Arthur → locatedIn → Forest
Dragon → isAlive → False
```

### Fact Extraction

Converts generated narrative text into structured triples:

```text
Arthur killed the dragon.
```

↓

```text
(Arthur, killed, Dragon)
```

### Neuro-Symbolic Verification

Verifies generated facts against:

- Knowledge Graph state
- Temporal rules
- Character rules
- Inventory rules
- Location rules

Example:

```text
Arthur → isAlive → False
```

Generated:

```text
Arthur attacks the dragon.
```

↓

```text
Violation Detected:
Dead character cannot act.
```

### Self-Correction Loop

```text
Generate
   ↓
Verify
   ↓
Violation
   ↓
Rewrite
   ↓
Verify Again
```

The LLM automatically regenerates a corrected story beat.

### Interactive Visualization

Real-time visualization of the evolving Knowledge Graph using:

- NetworkX
- Matplotlib
- Streamlit

---

# System Architecture

```text
                User Input
                     │
                     ▼
         ┌─────────────────────┐
         │ Hybrid Retriever    │
         └─────────────────────┘
              ▲           ▲
              │           │
      ┌────────────┐  ┌────────────┐
      │   FAISS    │  │ Knowledge  │
      │ Semantic   │  │   Graph    │
      │  Memory    │  │ Symbolic   │
      └────────────┘  └────────────┘
              │
              ▼
      ┌──────────────────┐
      │ LLM Generator    │
      └──────────────────┘
              │
              ▼
      Candidate Story Beat
              │
              ▼
      ┌──────────────────┐
      │ Fact Extractor   │
      └──────────────────┘
              │
              ▼
      ┌──────────────────┐
      │ Consistency      │
      │ Verifier         │
      └──────────────────┘
          │         │
      Pass│         │Fail
          ▼         ▼
    Final Story   Self-Correction
        Beat          Loop
          │
          ▼
     Update KG
     Update FAISS
```

---

## Project Structure

```text
NS-RAG/
│
├── app.py
├── story_engine.py
├── llm_manager.py
├── kg_manager.py
├── hybrid_retriever.py
├── verifier.py
├── evaluator.py
├── schema.py
│
├── tests/
│   └── test_consistency.py
│
├── data/
├── figures/
└── requirements.txt
```

---

## Technology Stack

| Component | Technology |
|------------|------------|
| Frontend | Streamlit |
| LLM Backend | Ollama |
| Vector Database | FAISS |
| Embeddings | all-MiniLM-L6-v2 |
| Knowledge Graph | NetworkX |
| Validation | Pydantic |
| Visualization | Matplotlib |
| Language | Python |

---

# Hybrid Retrieval Pipeline

The retriever combines semantic and symbolic memory.

### Semantic Retrieval (FAISS)

Uses sentence embeddings and cosine similarity to retrieve relevant story history.

Example:

```text
Query:
Arthur enters forest again
```

Retrieved Memory:

```text
Arthur fought a dragon in the forest.
```

### Symbolic Retrieval (Knowledge Graph)

Performs depth-limited graph traversal to retrieve relevant facts.

Example:

```text
Arthur → owns → Sword
Arthur → locatedIn → Forest
Dragon → isAlive → False
```

### Combined Context

```text
Relevant Story Segments
+
Knowledge Graph Facts
+
Character State
+
World State Snapshot
+
Generation Constraints
```

↓

```text
LLM Prompt Context
```

---

# Consistency Verification

The verifier validates generated facts using symbolic rules.

### Character Rules
- Dead characters acting
- Illegal resurrection
- Relationship contradictions

### Inventory Rules
- Duplicate ownership
- Invalid item transfers
- Using unowned items

### Location Rules
- Teleportation
- Invalid movement

### Temporal Rules
- Actions after death
- Actions before arrival
- Invalid event ordering

---

# Evaluation Framework

NS-RAG introduces a consistency-focused evaluation framework.

## Consistency Score (CS)

Measures overall narrative consistency.

```math
CS = (Consistent Steps / Total Steps) × 100
```

## Knowledge Graph Consistency Score (KGCS)

Measures consistency of Knowledge Graph facts.

Detects conflicts in:
- Character state
- Location
- Health
- Faction

## Temporal Consistency Score (TCS)

Measures timeline correctness.

Detects:
- Actions after death
- Actions before arrival
- Invalid event ordering

## Character Consistency Score (CCS)

Measures character behaviour consistency.

Detects:
- Dead characters acting
- Illegal resurrection
- Relationship contradictions

## Inventory Consistency Score (ICS)

Measures item ownership consistency.

Detects:
- Duplicate ownership
- Invalid transfers
- Using unowned items

## Hallucination Rate (HR)

Measures how often generated facts reference unknown entities.

```math
HR = Unknown Entity Facts / Total Facts
```

Lower is better.

## Rewrite Frequency (RF)

Measures how often the verifier forces regeneration.

```math
RF = Total Rewrites / Total Steps
```

Lower is better.

---

# Benchmark Scenarios

The framework evaluates consistency using predefined narrative stress tests.

### Death Barrier

```text
Arthur dies.
```

Challenge:

```text
Arthur attacks again.
```

Tests:
- CCS
- TCS

---

### Item Transfer

```text
Arthur owns Sword.
```

Challenge:

```text
Merlin uses Sword.
```

without transfer.

Tests:
- ICS

---

### Travel Event

```text
Arthur locatedIn Castle.
```

Challenge:

```text
Arthur fights in Forest.
```

without travelling.

Tests:
- TCS

---

### Relationship Change

```text
Arthur enemyOf Merlin.
```

Challenge:

```text
Arthur and Merlin become allies.
```

without reconciliation.

Tests:
- CCS
- KGCS

---

# Running the Project

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Start Ollama

```bash
ollama serve
```

Pull a model if needed:

```bash
ollama pull llama3.2
```

## Launch the Application

```bash
streamlit run app.py
```

---

# Research Contributions

- Neuro-Symbolic RAG architecture for interactive storytelling
- Dynamic Knowledge Graph world-state tracking
- Rule-based consistency verification engine
- Automated self-correction loop
- Hybrid semantic-symbolic retrieval
- Consistency-focused evaluation framework
- Interactive Knowledge Graph visualization

---

# Future Work

- Multi-agent character reasoning
- Automatic graph summarization for large narratives
- Advanced relationship and emotion modeling
- Hierarchical memory management
- Graph Neural Network (GNN) integration
- Larger benchmark datasets and human evaluation studies

---

# License

This project is intended for academic and research purposes.

---

## Author

**Rashid Rafeek**

Final Year Project – Neuro-Symbolic AI, Knowledge Graphs, and Retrieval-Augmented Generation for Interactive Narrative Generation.
