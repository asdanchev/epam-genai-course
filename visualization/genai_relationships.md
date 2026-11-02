# Generative AI — Relationships Between Terms

**Author:** Anton Danchev  
**Course:** EPAM Tech Orda — GenAI Master Program  
**Date:** November 2026

This diagram shows how the main GenAI concepts
are connected to each other.

```mermaid
flowchart TD
    AI["Artificial Intelligence"]
    ML["Machine Learning"]
    NN["Neural Networks"]
    DL["Deep Learning"]
    GenAI["Generative AI"]
    LLM["Large Language Model"]

    Tokens["Tokens"]
    Context["Context Window"]

    Prompt["Prompt"]
    PE["Prompt Engineering"]

    RAG["RAG"]
    Emb["Embeddings"]
    FT["Fine-tuning"]

    API["API"]
    Agent["AI Agent"]

    AI -->|Includes| ML
    ML -->|Uses| NN
    NN -->|Used in| DL
    DL -->|Powers| GenAI
    GenAI -->|Includes| LLM

    Tokens -->|Used by| LLM
    Context -->|Limits| LLM

    PE -->|Improves| Prompt
    Prompt -->|Sent to| LLM

    RAG -->|Can use| Emb
    RAG -->|Provides context to| LLM
    FT -->|Adapts| LLM

    API -->|Provides access to| LLM
    Agent -->|Uses| API
    Agent -->|Uses| LLM

    classDef ai fill:#1e40af,stroke:#60a5fa,color:#ffffff,stroke-width:2px
    classDef model fill:#6d28d9,stroke:#a78bfa,color:#ffffff,stroke-width:2px
    classDef technology fill:#166534,stroke:#4ade80,color:#ffffff,stroke-width:2px
    classDef interaction fill:#9a3412,stroke:#fb923c,color:#ffffff,stroke-width:2px
    classDef agent fill:#991b1b,stroke:#f87171,color:#ffffff,stroke-width:2px

    class AI,ML,NN,DL ai
    class GenAI,LLM model
    class Tokens,Context,RAG,Emb,FT technology
    class Prompt,PE,API interaction
    class Agent agent
```

## Diagram Description

The diagram shows the relationships between different
AI technologies and concepts.

Artificial Intelligence includes Machine Learning.
Machine Learning can use neural networks, and
Deep Learning uses neural networks with many layers.

Generative AI uses deep learning technologies
to create new content. Large Language Models
are one type of Generative AI.

Language models process text using tokens.
The context window limits how much information
a model can process at one time.

Prompt Engineering helps users write better prompts
to get more accurate and useful responses.

RAG can use embeddings to find relevant information
and provide additional context to language models.

Fine-tuning is used to adapt existing language models
for specific tasks.

APIs allow applications to communicate with AI models.
AI Agents can use language models and external APIs
to perform tasks automatically.