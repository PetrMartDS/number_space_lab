📘 Number Space Lab — Project Notes & Planning
🧠 Core Research Goal
To empirically investigate whether large language models exhibit any consistent internal representation of numbers — a number space — and to compare how different models encode numerical relationships.
We begin with no strong philosophical assumptions. Instead, we test three possible outcomes:

🔬 Hypothesis Space
H0 — No Number Space (Null Hypothesis)

There is no meaningful global structure among number embeddings in LLMs.
Embeddings of "1", "2", "100", "999" show no systematic ordering; any patterns are incidental (e.g., tokenization artifacts).

H1 — Weak or Global Number Structure
LLMs exhibit some approximate number structure:

Rough ordinality
Partial correlation between numeric distance and embedding distance
Occasional arithmetic-like translation patterns

…but the structure is noisy, inconsistent, and not universal.
H2 — Context-Dependent Number Spaces
Numbers do not have a single unified embedding structure. Instead, they form multiple local number spaces depending on context, such as:

Money ("I paid 37 dollars")
Measurement ("37 cm long")
Age ("She is 37 years old")
Probability ("37% chance")

Different contexts create different geometries, and the identity of a number may fragment across contexts.
Model Comparison Sub-Hypothesis
Regardless of H0/H1/H2, different models (GPT, LLaMA, Mistral, DeepSeek, etc.) may show:

Similar structures → hinting at universality
Completely different structures → model-specific emergence

Our experiments must measure both existence and consistency of number structures.

📁 Repository Structure (Planned)
number-space-lab/
├─ README.md
├─ PROJECT_NOTES.md          # This file
├─ EXPERIMENT_LOG.md
├─ requirements.txt
│
├─ notebooks/
│   ├─ 01_static_number_embeddings.ipynb
│   ├─ 02_context_number_spaces.ipynb
│   ├─ 03_cross_model_alignment.ipynb
│   ├─ 04_tokenization_artifacts.ipynb
│   └─ 05_arithmetic_geometry.ipynb
│
├─ numberspace/
│   ├─ __init__.py
│   ├─ config.py
│   ├─ embeddings.py
│   ├─ geometry.py
│   ├─ context.py
│   └─ plotting.py
│
└─ results/
    ├─ static_embeddings/
    ├─ context_spaces/
    ├─ cross_model/
    └─ figures/

🧱 Module Overview
numberspace/config.py

List of models used
Default ranges (1–100, 1–1000)
Context templates
Paths to results directories

numberspace/embeddings.py
Functions for retrieving embeddings:

get_token_embedding(model, text)
get_embeddings_for_range(model, start, end)
get_contextual_embeddings(model, number, template)
Optional: batch APIs for speed

Supports both:

OpenAI API embeddings
Local HuggingFace models (LLaMA/Mistral/etc.)

numberspace/geometry.py
Core analysis utilities:

Distance matrices
Correlations: corr(|i-j|, dist(i,j))
Nearest-neighbor extraction
Procrustes alignment between models
Similarity metrics between number spaces
Dimensionality estimation (PCA eigenvalues)

numberspace/context.py
Handles generation of contextual sentences:

"I paid {n} dollars"
"The table is {n} centimeters long"
"She is {n} years old"
"There is a {n}% chance"

And functions to extract per-context embedding sets.
numberspace/plotting.py
Visualization helpers:

PCA scatter for numbers
UMAP / t-SNE projections
Heatmaps of distance matrices
Cluster visualization across contexts
Cross-model overlay plots


🧪 Notebook Structure (Experiments)
Notebook 01 — Static Number Embeddings (H0 vs H1)
Goal: Test whether numbers 1–100 (or 1–1000) have any global structure.
Steps:

Load embeddings for plain tokens: "1", "2", ..., "100"
Compute distance matrix
Correlate distance with numeric difference
Nearest neighbor analysis
Visualize with PCA/UMAP

Interpret pattern:

No pattern → H0 support
Weak line/curve → H1 support


Notebook 02 — Context-Based Number Spaces (Testing H2)
Goal: Determine if context produces distinct geometries.
Contexts:

Money
Length
Age
Probability
Optional domain-specific (engineering, rating, etc.)

Steps:

Generate contextual embeddings
Analyze each context's number geometry separately
Compare across contexts
Test if the same number embeds closer to itself or to neighbors within each context

Interpret:

Strong differences → H2 support
Similar structures → weak H1


Notebook 03 — Cross-Model Universality
Goal: Check whether number space (global or local) is shared across models.
Steps:

Repeat static/context experiments for multiple models
Align number spaces using Procrustes
Compare distance matrices
Look for universal patterns

Interpret:

High similarity → universality evidence
Low similarity → model-specific structure


Notebook 04 — Tokenization & Artifact Analysis
Goal: Identify effects of tokenizer segmentation on number embeddings.
Steps:

Check how models tokenize "10", "100", "1000"
Compare embeddings of numbers with similar character structure
Distinguish true number structure vs string-based clustering


Notebook 05 — Arithmetic Geometry (Optional, Later)
Goal: Test whether arithmetic operations appear as approximate transformations.
Experiments:

Check linearity of addition: v(3) - v(2) ≈ v(7) - v(6)
Look for translation directions
Multiplicative distortions
Failure modes

(Useful later once basic structure is mapped.)

🗂️ Experiment Logging
All experiments recorded in: EXPERIMENT_LOG.md
Each entry includes:

Experiment name
Date
Model(s)
Number range
Context (if any)
Methods
Key results
Next steps

Example snippet:
markdown## EXP-001 – Static embeddings 1–100 (GPT-4o-mini)
- Date: 2025-12-09
- Result: Weak curved structure, moderate neighbor alignment.
- H0/H1 Status: Weak evidence toward H1.
- Next: Test larger range + LLaMA comparison.

🎯 Research Philosophy
This project is hypothesis-driven but empirically grounded. We:

Start with multiple possible outcomes (H0, H1, H2)
Let data determine which hypothesis has support
Remain open to unexpected patterns
Compare across models to test universality
Control for artifacts (tokenization, context effects)

We do not assume LLMs "understand" numbers in any deep sense. We simply ask: Is there structure? If so, what kind?

📊 Key Success Metrics
Evidence for H0 (No Structure)

Distance correlation near 0
Random nearest neighbors
No consistent PCA patterns

Evidence for H1 (Weak Global Structure)

Moderate distance correlation (0.3-0.7)
Better-than-random neighbor alignment
Visible but noisy PCA structure

Evidence for H2 (Context-Dependent)

Strong within-context structure
Weak cross-context alignment
Numbers cluster by context, not value

Evidence for Universality

High Procrustes alignment (>0.7)
Similar distance correlations across models
Consistent PCA patterns


🚀 Next Steps (Development Roadmap)
Phase 1: Setup

 Create repository structure
 Write project documentation
 Complete requirements.txt
 Implement numberspace/ modules
 Create notebook templates

Phase 2: Initial Experiments

 EXP-001: Static embeddings (single model)
 EXP-002: Context variations (single model)
 Document initial findings

Phase 3: Cross-Model Analysis

 EXP-003: Multi-model static comparison
 EXP-004: Tokenization analysis
 Statistical significance testing

Phase 4: Advanced Analysis

 EXP-005: Arithmetic geometry
 Write research summary
 Create visualizations for presentation


💡 Open Questions

Granularity: Should we test 1-100, 1-1000, or both?
Context variety: How many contexts are needed to test H2 thoroughly?
Model selection: Which models are most interesting to compare?
Embedding method: Token embeddings vs sentence embeddings?
Statistical testing: What baseline should we compare against?


📚 Related Work (To Explore)

Studies on number representation in neural networks
Word2vec arithmetic properties
Geometric properties of language model embeddings
Tokenization effects on semantic similarity
Cross-lingual number alignment studies

