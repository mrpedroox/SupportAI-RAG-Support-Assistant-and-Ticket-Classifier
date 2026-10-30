# SupportAI

A support assistant combining retrieval-augmented generation (RAG), source citations, and human review with a supervised NLP ticket classifier.

SupportAI is a learning and portfolio project focused on **system architecture, backend engineering, model training, MLOps, and LLMOps**.

> **Status: Sprint 1 — S1.9 audit and explanation completed; sprint closure pending.** All 50 annotations have content approval: 35 development and 15 frozen test cases, evaluation `0.2.1-draft`. The user authorized the agent to review and approve the 42 remaining annotations; no independent human review is claimed. The 24 document texts remain unchanged. Corpus `0.1.6` is approved for future administrative ingestion with CC BY 4.0 licensing; ingestion has not run. The learner comprehension checkpoint remains open. See the [S1.9 audit](docs/S1_9_CHECKPOINT.md). Application code, trained models, benchmarks, CI, and deployment are not available yet.

## Support domain

The initial scenario is **NexoDesk**, a fictional SaaS product for organizing support tickets in a workspace. SupportAI will assist a Portuguese-speaking support agent in one organization with documented account and access procedures, basic use and known issues, billing guidance, and service requests. It will produce drafts for human review.

The knowledge base will use fictional Markdown procedures. It will not establish policies for real products, determine individual contractual exceptions, confirm live service status, or execute account and billing changes. A question within the domain still requires supporting evidence before it can be answered.

See [the domain and data scope](docs/DATA.md) for examples and limitations, and [the approved development plan](docs/PLAN.md) for the next steps. Classifier labels will be defined separately from these documentation areas during the supervised data pilot.

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
| Language | CPython 3.12.16 |
| API | FastAPI, Pydantic |
| Persistence | PostgreSQL, SQLAlchemy, Alembic |
| Retrieval | PostgreSQL full-text search and pgvector |
| Generation | One LLM provider and one embedding model per index version |
| Supervised learning | scikit-learn, TF-IDF, logistic regression |
| Experiment tracking and tracing | MLflow |
| Testing and code quality | pytest, Ruff |
| Dependency management | uv 0.13.6, pyproject.toml, uv.lock |
| Local environment | Docker, Docker Compose |
| Continuous integration | GitHub Actions |

Only the development tools pytest and Ruff are installed at this stage. Application libraries, model providers, and deployment infrastructure will be selected and added when needed. Fine-tuning an LLM is outside the initial scope.

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

### Minimal environment (S1.2)

Run the commands below from the repository root on Linux. Bootstrap prerequisites: an existing Python 3 with `venv`, pip, and HTTPS access to PyPI and GitHub. On the development machine, the bootstrap interpreter is the system Python 3.12.3; the project uses a separate CPython 3.12.16. The system interpreter is not replaced.

```bash
python3 -m venv .venv-tools
.venv-tools/bin/python -m pip install --index-url https://pypi.org/simple uv==0.13.6
export UV_PYTHON_INSTALL_DIR="$PWD/.cache/uv-python"
.venv-tools/bin/uv python install 3.12.16 --no-bin
.venv-tools/bin/uv sync --locked
```

The first installation downloads Python and packages. Set `UV_PYTHON_INSTALL_DIR` again when opening a new terminal; it keeps the managed interpreter inside this repository's ignored cache. `.venv-tools` isolates uv itself; `.venv` holds the project dependencies. Neither directory is committed.

Verify the installation:

```bash
export UV_PYTHON_INSTALL_DIR="$PWD/.cache/uv-python"
.venv-tools/bin/uv lock --check --offline
.venv-tools/bin/uv run --locked python --version
.venv-tools/bin/uv run --locked pytest --version
.venv-tools/bin/uv run --locked ruff --version
.venv-tools/bin/uv pip check --python .venv/bin/python
```

Expected versions for this lock: Python 3.12.16, pytest 9.2.0, Ruff 0.17.3. Lock validation and dependency compatibility checks must succeed. There is no test suite or application source yet: displaying pytest's version is an installation check, not a passing test run.

`pyproject.toml` declares direct dependencies and the supported Python series (`>=3.12,<3.13`). `.python-version` selects the exact interpreter. `uv.lock` is the single dependency lockfile, recording exact direct/transitive versions, package sources, hashes and platform markers. `uv sync --locked` installs those versions and refuses an outdated lock instead of silently updating it. See the [uv locking documentation](https://docs.astral.sh/uv/concepts/projects/sync/).

Python 3.12 is a supported series in its security-maintenance phase; see the [Python version status](https://devguide.python.org/versions/). The patch pin is an explicit reproducibility choice and should be reviewed when updating Python. It does not guarantee compatibility with application libraries that have not been added yet.

The project is configured with `package = false`: it manages an environment without building an empty application package. pytest will support behavioral tests as code arrives; Ruff will check and format Python. No artificial test or empty module is needed to demonstrate that the tools are installed.

### Knowledge source contract (S1.3)

[The data documentation](docs/DATA.md) defines document IDs, versions, sections, checksums, provenance, license and human review, along with retention and deletion policies. Metadata lives in JSON; procedures live in Markdown. A checksum detects content changes; it does not establish correctness or permission to use a source.

The [corpus manifest](configs/corpus_manifest.json) contains 24 documents in snapshot `0.1.4-draft`, with actual SHA-256 checksums and `status: draft`. Three pilot documents have user approval; 21 expansion documents await human review, and all licenses remain pending; it authorizes no ingestion. A future approved snapshot must have an approved license and recorded human review for every listed source. The ingestion validator will be implemented in Sprint 2; these contracts are not an existing security mechanism.

### Fictional document pilot (S1.4)

- [Access recovery after losing an authenticator](knowledge/nd-access-001.md): request steps, prerequisites and exceptions; no recovery guarantee or public status lookup.
- [Workspace invoice lookup](knowledge/nd-billing-001.md): administrator access, lookup, download and discrepancies; no refund or discount rules.
- [Tracking your own support tickets](knowledge/nd-tickets-001.md): authenticated lookup, four documented statuses and missing-ticket exceptions; no completion-time promises.

The three pilot documents are in Portuguese; their original draft notices describe their preparation. The manifest records the user approval from S1.6. Their 11 sections have stable IDs.

### Development question pilot (S1.5)

[The draft evaluation](evaluation/rag_dev_pilot.json) contains eight Portuguese questions: six answerable and two unanswerable, across seven groups. It includes a paraphrase pair with abbreviations, an ambiguous outcome explained by the corpus, and a missing-context question requiring clarification. All entries stay in development; no reserved test set has been created or used.

Answers reference literal evidence spans with document versions, checksums, section IDs and Unicode character offsets. Unanswerable cases have no fabricated evidence, only an abstention reason and expected response. The evaluation remains linked to the three-document snapshot 0.1.2-draft in commit 7acb503 by its exact manifest checksum and must never be indexed or supplied as generation context. These LLM-assisted annotations have explicit user approval recorded in S1.6; they are not model performance results or classifier training data.

### Human pilot review (S1.6 — content approved)

The [review guide](docs/PILOT_REVIEW.md) brings together the three source documents, all eight questions, expected answers, evidence passages, snapshot checksums and a decision checklist. The agent's editorial check identified no contradiction requiring a change before human review. This is not human approval or a model evaluation.

The user explicitly approved the three pilot documents and eight annotations and authorized S1.7 on 2026-10-30. The decision is recorded as project-user; no second independent human reviewer is claimed. Procedure bodies, questions and evidence remain unchanged. Licensing remains pending and keeps the manifest in draft. S1.7 will expand in small reviewable batches.

### Corpus expansion (S1.7 — editorial work complete)

The corpus now has 24 fictional Portuguese procedures and 95 sections: six access, four ticket, four billing, five service-request and five interface troubleshooting documents. The [complete review index](docs/S1_7_REVIEW.md) links all seven expansion batches and records editorial decisions, consistency checks and limits. The six previously existing procedures remain unchanged.

The expansion target is met. Human acceptance of the 21 expansion documents is still pending; the user's instruction to complete S1.7 is not recorded as approval of unseen content. All licenses remain pending and the manifest stays in draft. Eight development annotations remain bound to their original three-document snapshot; S1.8 will revalidate evidence and expand the question set. No model evaluation has been run.

Database migrations, ingestion, training, and application commands will be documented when implemented and verified. Do not expect a running application yet.

## Planned documentation

Documentation will be added incrementally:

- `docs/PLAN.md`: milestones and scope decisions.
- `docs/DATA.md`: sources, licenses, annotations, partitions, and retention.
- `docs/SYSTEM_CARD.md`: intended use, RAG configuration, evaluation, and limitations.
- `docs/MODEL_CARD.md`: classifier data, training, evaluation, and deployment version.
- `docs/PROJECT_LOG.md`: learning notes, decisions, and verification evidence.

## License

Project source code and operational configuration are licensed under [MIT](LICENSE). Documentation, fictional knowledge documents, evaluation annotations, and corpus metadata are licensed under [CC BY 4.0](LICENSE-CONTENT.md); embedded code examples use MIT. See the content notice for scope, attribution, and links to the legal terms. Third-party materials retain their own licenses.

SupportAI is an educational and portfolio project, experimental and without a claim of production readiness. This purpose adds no noncommercial restriction to the licenses.

The user approved this choice on 2026-10-30. Corpus `0.1.6` is approved in the [current manifest](configs/corpus_manifest.json). The [data release binding](configs/rag_data_release.json) links it to the unchanged evaluation and its historical [source corpus snapshot](configs/corpus_snapshots/corpus-0.1.5-draft.json). Licensing approval does not execute ingestion or publish the project.

### S1.8 approved annotations and frozen test

The [review guide](docs/S1_8_REVIEW.md) presents 42 new annotations in five batches with reference answers, evidence, and abstention reasons. The [split manifest](evaluation/rag_split_manifest.json) identifies the corpus snapshot, files, checksums, question IDs, and groups. Development contains 28 answerable and 7 unanswerable questions; test contains 11 and 4. Groups do not overlap. The original eight-question pilot remains unchanged and all its cases stay in development. All annotations have content approval; the 42 new cases were reviewed by the agent under explicit user authorization. The manifest records this decision, its timestamp, prior test exposure, and the frozen test checksum. No independent human review or retrieval/generation results are claimed. Use development for system choices and reserve test for the final candidate. The `draft` suffix is historical: licensing was later approved through the data release binding without rewriting the frozen evaluation snapshot.

### S1.9 foundation checkpoint

The [audit and learning checkpoint](docs/S1_9_CHECKPOINT.md) records verified counts, snapshot checksums, environment checks, remaining acceptance criteria, and a manual exercise using development cases only. The audit confirmed 24 documents, 95 sections, 44 internal references, 50 annotations, 73 literal evidence spans, and 33 groups with no overlap between partitions. Data snapshots and the frozen test remain unchanged.

The local environment matches the pinned versions; offline lock and installed-dependency checks passed. No application tests or model benchmarks were run. The Sprint 1 budget for APIs and hosting is R$0; no paid API calls or hosting purchases were made in the recorded steps. The licensing decision and manifest authorization are now recorded. Sprint 1 remains open for the learner comprehension checkpoint. Sprint 2 has not started.
