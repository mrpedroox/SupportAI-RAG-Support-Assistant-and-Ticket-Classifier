# SupportAI

A support assistant combining retrieval-augmented generation (RAG), source citations, and human review with a supervised NLP ticket classifier.

SupportAI is a learning and portfolio project focused on **system architecture, backend engineering, model training, MLOps, and LLMOps**.

> **Status: planning.** This README describes the intended system. Application code, trained models, benchmarks, CI, and deployment are not available yet. Setup instructions will be added and verified as each milestone is implemented.

## What it does

The planned application helps a support agent:

1. Ask a question about a documented support procedure.
2. Retrieve relevant passages from an approved knowledge base.
3. Generate a draft answer with references to the retrieved sources.
4. Recognize when the available evidence is insufficient.
5. Review, edit, or reject a draft while preserving the original response.
6. Classify a ticket using a trained NLP model and confirm or correct its suggested category.

For example, a question about recovering access after losing an authenticator would retrieve the documented recovery procedure and produce a draft grounded in that procedure. If the knowledge base does not cover the question, the assistant should acknowledge that limitation.

Answers remain drafts for human review. The system will not automatically contact customers or execute actions.

## Architecture

The initial design uses a modular application with PostgreSQL rather than separate microservices.

```text
Approved Markdown documents
        |
        v
Validation -> Chunking -> Embeddings -> PostgreSQL + pgvector
                                              |
Question -> FastAPI -> Retrieval -> Context -> LLM -> Output validation
                                                        |
                                                        v
                                              Answer + source references
                                                        |
                                                        v
                                              Human review + history

Labeled tickets -> Train/validation/test splits -> TF-IDF classifier
                                                        |
                                                        v
                                         Versioned artifact -> FastAPI
                                                        |
                                                        v
                                           Category + human feedback

MLflow: training experiments, RAG evaluation, and sanitized traces
```

The classifier and RAG pipeline share the backend but remain independent capabilities. A predicted ticket category will not restrict retrieval in the initial implementation.

## Planned stack

| Area | Tools |
| --- | --- |
| Language | Python |
| API | FastAPI, Pydantic |
| Persistence | PostgreSQL, SQLAlchemy, Alembic |
| Retrieval | PostgreSQL full-text search and pgvector |
| Generation | One LLM provider and one embedding model per index version |
| Supervised learning | scikit-learn, TF-IDF, logistic regression |
| Experiment tracking and tracing | MLflow |
| Testing and code quality | pytest, Ruff |
| Local environment | Docker, Docker Compose |
| Continuous integration | GitHub Actions |

Model providers, dependency versions, and deployment infrastructure will be selected during implementation. Fine-tuning an LLM is outside the initial scope.

## Learning objectives

- **System architecture:** responsibilities, API contracts, transactions, external dependencies, failure handling, and observability.
- **RAG:** document ingestion, chunking, embeddings, retrieval, context budgets, citations, and abstention.
- **Model training:** labeling, leakage prevention, baselines, hyperparameter selection, and per-class evaluation.
- **MLOps:** reproducible training, model artifacts, manual promotion, serving, feedback, controlled retraining, monitoring, and rollback.
- **LLMOps:** versioned prompts and collections, traces, response evaluation, token usage, latency, and cost estimation.

## Delivery roadmap

All eight milestones are required for the complete project. The first RAG deployment is an intermediate delivery.

- [ ] **1. Knowledge and evaluation foundation:** define the domain, curate documents, and annotate evaluation questions and evidence.
- [ ] **2. Ingestion and retrieval:** implement versioned ingestion and compare lexical and vector search.
- [ ] **3. RAG API:** generate source-grounded drafts, abstain when appropriate, and persist query history.
- [ ] **4. Human review and evaluation:** add review history, safety tests, response evaluation, tracing, and cost measurements.
- [ ] **5. Reproducible delivery:** add Docker, CI, integration tests, and backup/restore verification.
- [ ] **6. RAG release:** evaluate the candidate on reserved questions and deploy a verified demonstration.
- [ ] **7. Supervised training:** prepare labeled tickets, train baselines, evaluate the classifier, and record artifacts in MLflow.
- [ ] **8. Complete MLOps release:** serve the classifier, collect corrections, compare a retraining run, monitor behavior, and verify rollback.

## Evaluation approach

**Retrieval:** compare lexical and vector search using Hit@k, Recall@k, and complementary ranking metrics on a fixed corpus snapshot.

**Answers:** review correctness, support from cited passages, citation relevance, and appropriate abstention. Valid source IDs alone do not prove that a claim is supported.

**Classification:** compare a trivial baseline against a TF-IDF and logistic regression pipeline using macro-F1 and precision, recall, F1, and support per class.

**Operations:** measure latency, failures, token usage, estimated cost, and model/collection versions under documented conditions.

Development and reserved test sets will be separated. Related examples and duplicates must stay within the same supervised data partition. Evaluation questions and reference answers must never be indexed into the RAG knowledge base.

No performance results are claimed yet. Metrics and limitations will be published after experiments are run.

## Data, privacy, and safety

- Start with public, appropriately licensed or explicitly fictional documents and tickets.
- Record dataset origin, labeling rules, versions, and limitations.
- Keep credentials, private data, local tracking stores, and generated artifacts out of Git.
- Treat retrieved documents and user input as untrusted content; they cannot authorize actions or override system rules.
- Avoid storing full prompts, questions, answers, or retrieved context in logs and traces by default.
- Require authentication for queries, history, and reviews in the deployed demonstration.
- Use bounded context, output limits, timeouts, request limits, and an explicit provider budget.

Prompt-injection controls and human review reduce risk but do not guarantee correctness. Synthetic-only evaluations will be identified as such and will not be presented as evidence of real-world performance.

## Getting started

Implementation has not started. Installation, environment variables, database migrations, ingestion, training, testing, and run commands will be documented here as they become available and are verified.

The initial task is to define a narrow support domain, prepare a small document-and-question pilot, and record the first sprint plan. Do not expect a running application from this repository yet.

## Planned documentation

Documentation will be added incrementally:

- `docs/PLAN.md`: milestones and scope decisions.
- `docs/DATA.md`: sources, licenses, annotations, partitions, and retention.
- `docs/SYSTEM_CARD.md`: intended use, RAG configuration, evaluation, and limitations.
- `docs/MODEL_CARD.md`: classifier data, training, evaluation, and deployment version.
- `docs/PROJECT_LOG.md`: learning notes, decisions, and verification evidence.

## License

A project license has not been selected yet. A `LICENSE` file will be added once that decision is made. Third-party documents, datasets, and models remain subject to their own licenses.
