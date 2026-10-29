# VOIDFORGE STUDIOS
## Claude AI Prompt Pack — Batch 2
### Category 02: AI Engineering & LLM Applications
**Prompt IDs:** 021–040  
**Edition:** Professional Claude-Optimized Collection

---

## Prompt 021 — Architect an LLM Application Around Real Requirements

**Category:** AI Engineering & LLM Applications

**Use case:** You have an AI product idea and need a defensible technical architecture before committing to a stack or implementation.

**Objective:** Produce a requirements-grounded architecture with component boundaries, data flows, risks, and a phased implementation plan.

**Prompt:**
```text
Act as a senior AI systems architect experienced in production LLM applications. Design an architecture for the application described below. Do not assume that adding an LLM is automatically the right solution; identify deterministic or simpler alternatives where they are more reliable.

<project_description>
[PROJECT_DESCRIPTION]
</project_description>
<users_and_workflows>
[USERS_AND_PRIMARY_WORKFLOWS]
</users_and_workflows>
<constraints>
[TECH_STACK_OR_PREFERENCES, BUDGET, LATENCY, SCALE, DATA_SENSITIVITY]
</constraints>
<existing_system>
[EXISTING_SYSTEMS_OR_NONE]
</existing_system>

Work in this order:
1. Extract functional requirements, non-functional requirements, and unknowns. Ask focused questions first if an unknown could change the architecture materially; otherwise state a provisional assumption.
2. Identify which steps require an LLM and which should remain deterministic.
3. Propose the system components, responsibilities, interfaces, and data flow from input to response.
4. Compare viable model, storage, retrieval, orchestration, and deployment options against the stated constraints. Do not invent current prices, benchmarks, or provider capabilities; mark them for verification when tools or sources are unavailable.
5. Define trust boundaries, privacy controls, failure modes, timeouts, retries, rate limits, and fallback behavior.
6. Specify observability, evaluation, and operational ownership before recommending production launch.
7. Create a phased plan that delivers a narrow end-to-end slice before optional complexity.

Output:
- Requirements and assumptions
- Architecture diagram in Mermaid, if supported by the available output format
- Component table with responsibility and interface
- Primary request/data flow
- Decision matrix with trade-offs
- Security and reliability risks
- MVP scope versus later phases
- Acceptance criteria and open questions

Do not produce implementation code unless requested. Do not claim the architecture is validated by tests or load measurements that were not run. Finish with a checklist of decisions that must be confirmed before implementation.
```

**Expected deliverable:** An architecture brief that a developer can review before implementation, including explicit trade-offs and unresolved decisions.

**Customization notes:** Replace the project, workflow, and constraint placeholders. Include real latency, budget, privacy, and scale targets if known; otherwise label them unknown.

---

## Prompt 022 — Build a Reusable Prompt Template System

**Category:** AI Engineering & LLM Applications

**Use case:** Prompt logic is scattered through application code, making it difficult to version, test, reuse, and maintain.

**Objective:** Design a prompt-template system with versioning, variable validation, safe rendering, and regression tests.

**Prompt:**
```text
Act as an LLM application engineer designing a maintainable prompt-template system for [APPLICATION_NAME], built with [LANGUAGE_AND_FRAMEWORK].

Current prompt examples or implementation:
[CURRENT_PROMPTS_OR_CODE]
Required workflows:
[WORKFLOWS]
Constraints:
[REPOSITORY_CONVENTIONS, DEPLOYMENT, PRIVACY, AND TESTING REQUIREMENTS]

First inspect the supplied implementation and conventions. If repository tools are available, inspect relevant files before proposing edits; otherwise work only from the supplied excerpts and state the limitation.

Design a system that:
1. Separates prompt content from application logic without creating unnecessary abstraction.
2. Gives each template a stable identifier, explicit version, purpose, owner or source, and expected output contract.
3. Defines required and optional variables, types, defaults, and validation errors.
4. Prevents accidental variable omission, ambiguous interpolation, and untrusted user content being confused with instructions. Use clear delimiters or structured message roles where appropriate.
5. Supports environment-specific configuration without embedding secrets in templates.
6. Allows a prompt version to be rolled back and records which version produced a result.
7. Includes representative unit tests for valid rendering, missing variables, unexpected variables, escaping/delimiters, and version selection.
8. Defines how changes are reviewed and evaluated before promotion.

Deliver:
- Proposed module and file structure
- Template schema with one realistic example
- Rendering and validation interface
- Versioning and migration rules
- Implementation or patch only if sufficient source context is available
- Tests with commands to run
- Risks and rejected alternatives
- Completion checklist

Keep the design proportional to the application's size. Do not claim tests passed unless they were actually executed. Clearly distinguish inspected files, proposed changes, executed tests, and unverified behavior.
```

**Expected deliverable:** A reusable, testable prompt-template design and an implementation plan aligned with the current stack.

**Customization notes:** Supply representative prompts and application conventions. If you want code changes, provide repository access through available tools or paste the relevant files.

---

## Prompt 023 — Enforce Reliable Structured LLM Outputs

**Category:** AI Engineering & LLM Applications

**Use case:** An application expects JSON or typed data from a model, but responses sometimes contain missing fields, invalid types, or extra prose.

**Objective:** Define a robust structured-output contract and recovery strategy.

**Prompt:**
```text
Act as a senior engineer responsible for reliable structured outputs from an LLM. Design or repair the output pipeline for [USE_CASE] using [LANGUAGE, FRAMEWORK, AND MODEL/API IF KNOWN].

Desired schema:
[JSON_SCHEMA_OR_TYPE_DEFINITIONS]
Representative model outputs, including failures:
[EXAMPLE_OUTPUTS]
Downstream behavior:
[WHAT_CONSUMES_THE_OUTPUT]

Analyze the task as a data-contract problem, not merely a prompt-wording problem.

1. Review the schema for ambiguity, overly broad fields, invalid states, and missing constraints.
2. Recommend native constrained/structured output features if the selected provider supports them; do not assume support without evidence. Provide a portable fallback if needed.
3. Separate generation, parsing, schema validation, semantic validation, and business-rule validation.
4. Define behavior for truncated output, refusal, empty values, unknown enum values, malformed JSON, and valid syntax with invalid meaning.
5. Specify bounded repair or retry behavior. Avoid infinite retries and avoid silently inventing missing facts.
6. Preserve provenance for extracted values and represent uncertainty or unavailable data explicitly when the schema allows it.
7. Create adversarial and boundary test cases, including Unicode, escaping, long fields, nulls, and unexpected extra keys.
8. Explain when to reject, ask for clarification, use a fallback, or route to human review.

Return:
- Revised schema and rationale
- Example valid output
- Validation pipeline or pseudocode/code suitable for the stated stack
- Error taxonomy and recovery policy
- Test matrix with expected results
- Logging and privacy recommendations
- Acceptance checklist

Never treat JSON parse success as proof of factual correctness. Do not claim provider-specific capabilities or successful tests without verification. Identify any assumptions that affect the design.
```

**Expected deliverable:** A structured-output contract that is syntactically valid, semantically checked, observable, and safe for downstream use.

**Customization notes:** Replace the schema and include actual failure examples. Tell Claude whether the application can reject results, retry, or require human review.

---

## Prompt 024 — Design a Retrieval-Augmented Generation Pipeline

**Category:** AI Engineering & LLM Applications

**Use case:** The application must answer questions from a private or specialized knowledge base with evidence rather than relying only on model memory.

**Objective:** Design a RAG pipeline from source ingestion through retrieval, grounded generation, evaluation, and operations.

**Prompt:**
```text
Act as a retrieval-augmented generation (RAG) architect. Design a pipeline for answering [QUESTION_TYPES] from [DOCUMENT_COLLECTION]. Optimize for grounded answers, traceable sources, maintainability, and the constraints below—not for architectural novelty.

Corpus details:
[FILE_TYPES, LANGUAGES, SIZE, UPDATE_FREQUENCY, ACCESS_RULES]
Application constraints:
[STACK, LATENCY_TARGET, COST_LIMIT, SECURITY, SCALE]
Known failure examples:
[OPTIONAL_FAILURES]

Work through these stages:
1. Identify source quality, document ownership, freshness, access-control, and deletion requirements.
2. Define ingestion: parsing, normalization, metadata extraction, duplicate handling, version tracking, and failed-document quarantine.
3. Compare chunking approaches against the document structure and question types. Explain trade-offs; do not assume one chunk size fits all.
4. Select retrieval options: keyword, vector, hybrid, metadata filters, reranking, or query rewriting as justified by evidence and constraints.
5. Design authorization-aware retrieval so a user cannot retrieve documents they are not permitted to access.
6. Specify how the model receives retrieved evidence and how it must cite or identify supporting passages. Define behavior when evidence is weak, contradictory, stale, or absent.
7. Add caching, timeouts, retries, telemetry, index updates, deletion propagation, and a rollback plan.
8. Propose offline and online evaluations for retrieval quality, answer groundedness, citation correctness, and refusal when unsupported.

Deliver a data-flow diagram, component choices with alternatives, metadata schema, retrieval and generation contracts, evaluation dataset plan, failure-handling table, and phased implementation plan. State which recommendations require empirical testing on the real corpus. Do not fabricate evaluation scores or claim that the design has been benchmarked.
```

**Expected deliverable:** An end-to-end RAG architecture with access controls, evidence handling, evaluation, and operational safeguards.

**Customization notes:** Describe your corpus and user permissions. Include real example questions and source documents if possible; RAG quality depends heavily on them.

---

## Prompt 025 — Create an Embedding and Vector Indexing Pipeline

**Category:** AI Engineering & LLM Applications

**Use case:** You need to transform text or other supported content into embeddings and maintain a searchable index as source data changes.

**Objective:** Specify an ingestion and indexing pipeline with reproducibility, idempotency, versioning, and recovery.

**Prompt:**
```text
Act as an ML infrastructure engineer designing an embedding and indexing pipeline for [DATASET_OR_PRODUCT]. The pipeline must be reproducible, observable, and safe to rerun.

Source data:
[FORMAT, SIZE, LANGUAGE, IDENTIFIERS, UPDATE_PATTERN]
Embedding and storage candidates:
[KNOWN_MODELS_OR_SERVICES, OR “NOT DECIDED”]
Operational requirements:
[THROUGHPUT, FRESHNESS, COST, PRIVACY, RETENTION]

1. Define the record identity and canonical text representation used to create an embedding.
2. Design normalization, filtering, metadata extraction, language handling, and chunk boundaries where applicable.
3. Explain how the embedding model, dimensions, preprocessing, and index configuration are versioned together. Do not mix incompatible vector dimensions or silently compare vectors from incompatible model spaces.
4. Define incremental updates, content hashes, idempotent writes, duplicate handling, deletion, and stale-record cleanup.
5. Estimate work and storage only from stated inputs. If inputs are missing, provide formulas and identify the measurements needed rather than inventing totals.
6. Define batching, rate limiting, retries with backoff, checkpointing, dead-letter handling, and safe restart after interruption.
7. Include validation for malformed records, empty content, extreme length, duplicate IDs, vector dimension mismatch, and partial indexing.
8. Propose monitoring for throughput, error rate, lag, cost, index completeness, and embedding-version distribution.

Deliver: architecture, record schema, pipeline stages, idempotency strategy, pseudocode or code for [STACK] if enough context is supplied, recovery procedure, test cases, and launch checklist. Separate tested behavior from design proposals. Never claim a pipeline ran or an index is complete without execution evidence.
```

**Expected deliverable:** An implementable indexing specification that can be safely rerun and updated without silent corruption or uncontrolled cost.

**Customization notes:** Provide data volume, source format, update frequency, and candidate embedding model. Treat performance estimates as estimates until measured.

---

## Prompt 026 — Build Semantic Search That Matches User Intent

**Category:** AI Engineering & LLM Applications

**Use case:** Keyword search misses conceptually related results, but naive vector search returns irrelevant or overly broad matches.

**Objective:** Design a semantic search experience and evaluation plan tuned to real user queries.

**Prompt:**
```text
Act as a search relevance engineer. Improve search for [PRODUCT_OR_DATASET] so results match user intent while remaining explainable and measurable.

Search context:
[DOCUMENT_TYPES, USERS, QUERY_PATTERNS, CURRENT_SEARCH_METHOD]
Examples:
[QUERIES_WITH_RELEVANT_AND_IRRELEVANT_RESULTS]
Constraints:
[STACK, LATENCY, COST, ACCESS_CONTROL]

1. Classify query types: exact identifiers, keyword lookups, conceptual questions, filters, multilingual queries, misspellings, and ambiguous requests.
2. Determine which query types should use lexical search, vector search, hybrid retrieval, metadata filters, or specialized handling.
3. Propose query normalization and rewriting rules that preserve key terms, negation, IDs, and user constraints.
4. Explain candidate generation, score combination, reranking, tie handling, and result diversity. Avoid treating similarity scores as calibrated probabilities unless calibration is measured.
5. Design a relevance dataset with representative queries, judged results, hard negatives, and clear grading guidelines.
6. Select metrics appropriate to the interface, such as Recall@k, Precision@k, nDCG, MRR, zero-result rate, and latency. Define how each will be computed.
7. Include safeguards for access permissions, stale records, adversarial queries, and query logging privacy.
8. Recommend a small baseline before introducing more complex ranking components, then define controlled experiments.

Output: search pipeline diagram, ranking strategy, example query handling, relevance-labeling guide, offline evaluation plan, launch metrics, failure modes, and a prioritized experiment backlog. Do not claim relevance has improved until results from a comparable evaluation support that conclusion.
```

**Expected deliverable:** A search relevance plan with clear ranking logic and measurable criteria for improvement.

**Customization notes:** Provide actual queries and judged results if available. If no relevance data exists, the first deliverable should be a dataset-building plan.

---

## Prompt 027 — Process Documents into Reliable, Traceable Data

**Category:** AI Engineering & LLM Applications

**Use case:** PDFs, scans, emails, spreadsheets, or office files must be converted into structured data without losing source traceability.

**Objective:** Design a document-processing pipeline that handles varied formats, extraction errors, and human review.

**Prompt:**
```text
Act as a document AI engineer designing a reliable extraction pipeline for [BUSINESS_PROCESS]. Convert [INPUT_DOCUMENT_TYPES] into [TARGET_FIELDS_OR_SCHEMA] while preserving evidence and flagging uncertainty.

Sample documents or field definitions:
[DOCUMENT_EXAMPLES_AND_SCHEMA]
Volume and constraints:
[VOLUME, LANGUAGES, PRIVACY, RETENTION, REVIEW_CAPACITY]

1. Map each input type to an appropriate parser, OCR method, table extractor, or multimodal model, based on actual tool availability.
2. Define preprocessing for rotated pages, low-quality scans, tables, headers, footers, multi-column layouts, and mixed languages.
3. Specify a schema for extracted values, source location, extraction method, confidence or review status, and document/version identifier.
4. Distinguish verbatim extraction from inferred normalization. Never silently infer a missing value or turn an ambiguous value into a fact.
5. Define field-level validation, cross-field checks, date/currency normalization, duplicate detection, and handling of conflicting documents.
6. Route low-confidence, high-impact, or contradictory fields to human review. Explain what evidence the reviewer sees.
7. Design safe retries, page-level recovery, audit logs, data retention, and deletion behavior.
8. Build a representative evaluation set covering clean documents, scans, tables, missing fields, and difficult edge cases.

Deliver: pipeline diagram, extraction schema, field mapping, validation rules, review policy, sample output, error taxonomy, evaluation metrics, and implementation checklist. If sample files are unavailable, label the output as a design rather than a validated extractor. Do not claim extraction accuracy without measured results.
```

**Expected deliverable:** A traceable document-to-data workflow that exposes uncertainty and supports targeted human review.

**Customization notes:** Specify document types and fields. Redact personal or confidential data before sharing samples unless an approved secure workflow is available.

---

## Prompt 028 — Create an Evaluation Dataset for an LLM Feature

**Category:** AI Engineering & LLM Applications

**Use case:** A team wants to compare prompts, models, or pipeline changes but has no consistent test set.

**Objective:** Build an evaluation dataset and scoring protocol that reflects real use, edge cases, and failure severity.

**Prompt:**
```text
Act as an LLM evaluation lead. Create a reproducible evaluation dataset for [FEATURE_OR_WORKFLOW]. The goal is to detect meaningful regressions and compare candidate implementations fairly.

Task definition and intended users:
[TASK_AND_USERS]
Expected behavior or rubric:
[ACCEPTANCE_CRITERIA]
Available examples and constraints:
[EXISTING_DATA, PRIVACY, LANGUAGE, LABELING_BUDGET]

1. Define the unit of evaluation, input schema, expected output format, and what counts as success or failure.
2. Segment the dataset by common cases, boundary cases, rare but important cases, ambiguous inputs, adversarial inputs, and known production failures.
3. Avoid leakage between prompt-development examples and the held-out evaluation set. Document any synthetic examples and their limitations.
4. Write labeling instructions with positive examples, negative examples, tie-breaking rules, and an uncertainty label.
5. Choose metrics tied to user impact. Separate exact-match or schema validity from factual correctness, groundedness, usefulness, safety, and latency/cost where relevant.
6. Define human review, inter-rater agreement checks, adjudication, and periodic refresh from real failures without exposing sensitive data.
7. Specify how to compare versions using identical inputs, controlled settings where possible, repeated runs for nondeterministic behavior, and uncertainty intervals when sample size supports them.
8. Create a dataset manifest containing version, provenance, intended use, exclusions, known blind spots, and access restrictions.

Return a starter dataset schema, 10–15 illustrative cases based only on the stated task, a scoring rubric, evaluation procedure, release criteria, and maintenance plan. Clearly label invented examples as illustrative rather than real customer data. Do not manufacture benchmark scores or claim statistical significance without calculations and adequate evidence.
```

**Expected deliverable:** A reusable evaluation specification and starter test set with documented coverage and limitations.

**Customization notes:** Supply real anonymized examples and explicit success criteria. Replace illustrative examples with reviewed cases before using the set as a release gate.

---

## Prompt 029 — Compare LLM Models Against Your Actual Workload

**Category:** AI Engineering & LLM Applications

**Use case:** You need to choose a model for a specific product task and want to avoid selecting based only on reputation or public benchmark headlines.

**Objective:** Create a fair, task-specific model comparison protocol covering quality, latency, cost, reliability, and constraints.

**Prompt:**
```text
Act as an independent LLM evaluation engineer. Help select a model for [APPLICATION_TASK] using evidence from our workload rather than brand reputation or unsupported claims.

Candidate models/providers:
[CANDIDATES]
Representative inputs and expected outcomes:
[EVALUATION_CASES]
Operational constraints:
[QUALITY_FLOOR, P95_LATENCY, COST_BUDGET, REGION, PRIVACY, TOOL_SUPPORT]

1. Identify non-negotiable requirements and eliminate candidates that cannot meet them, while marking any capability that needs current documentation verification.
2. Design a fair test harness with identical task instructions, input data, output schema, and comparable settings where supported.
3. Include normal cases, difficult cases, long-context cases if relevant, refusal or safety cases, and failure recovery cases.
4. Measure task-specific quality, schema validity, groundedness, latency distributions, timeout/error rates, and estimated cost per successful task. Define the measurement method for each.
5. Account for nondeterminism, rate limits, retries, caching, tokenization differences, and provider-specific features that may affect comparability.
6. Separate public benchmark evidence from our own measured evidence. Record test date, model/version identifier, configuration, and sample size.
7. Recommend a primary model, fallback model if justified, and conditions that would trigger reevaluation.

Deliver a test protocol, results table template, scoring rubric, cost formula, decision matrix, limitations, and next-step checklist. If no model tests have been run, provide the plan and leave result cells explicitly unmeasured. Do not invent prices, performance numbers, or rankings.
```

**Expected deliverable:** A defensible model-selection process tied to the real workload and measurable operating constraints.

**Customization notes:** List candidate model versions and include real examples. Current provider capabilities and prices must be verified from up-to-date sources before purchase or launch decisions.

---

## Prompt 030 — Reduce Unsupported Claims and Hallucinations

**Category:** AI Engineering & LLM Applications

**Use case:** An LLM sometimes invents details, answers beyond available evidence, or presents guesses with unjustified certainty.

**Objective:** Diagnose the failure sources and design layered mitigations with measurable tests.

**Prompt:**
```text
Act as an AI reliability engineer investigating unsupported claims in [APPLICATION]. Do not assume the prompt alone is the cause; examine the full system and the task's information limits.

System behavior and examples:
[FAILURE_EXAMPLES_WITH_EXPECTED_TRUTH_IF_KNOWN]
Available evidence sources:
[DOCUMENTS, DATABASES, TOOLS, OR NONE]
Risk level and constraints:
[IMPACT, HUMAN_REVIEW, LATENCY, COST]

1. Classify each failure: missing evidence, retrieval miss, source contradiction, stale data, instruction ambiguity, faulty tool result, invalid inference, fabricated citation, or output-validation gap.
2. Trace where the unsupported claim entered the pipeline and identify what evidence would be needed to verify it.
3. Propose layered controls: retrieval improvements, source-grounded response rules, explicit unknown states, tool verification, constrained output, claim-level evidence mapping, and human escalation where appropriate.
4. Define when the system must abstain, ask a clarifying question, state uncertainty, or provide a partial answer.
5. Prevent superficial fixes such as adding “never hallucinate” without changing evidence access or evaluation.
6. Create regression tests for known failures, missing sources, conflicting evidence, prompt injection in retrieved content, and misleading but plausible inputs.
7. Define metrics for unsupported-claim rate, citation correctness, answer coverage, abstention quality, and user-impact severity.
8. Identify trade-offs: stronger abstention can reduce false claims but may also reduce useful coverage.

Deliver a root-cause table, prioritized mitigation plan, revised response contract, test cases, monitoring plan, and release gate. Separate observed facts from hypotheses. Do not claim hallucinations have been eliminated; state what residual risks remain and what evidence would demonstrate improvement.
```

**Expected deliverable:** A testable hallucination-reduction plan that targets system causes and defines acceptable residual risk.

**Customization notes:** Provide examples with verified ground truth. If the true answer is unknown, mark it unknown rather than asking Claude to guess the correct answer.

---

## Prompt 031 — Manage Context Windows and Conversation State

**Category:** AI Engineering & LLM Applications

**Use case:** Long conversations or multi-step tasks lose important details, exceed context limits, or repeatedly send irrelevant history to the model.

**Objective:** Design a context-management strategy that preserves essential state while controlling token use and privacy risk.

**Prompt:**
```text
Act as an LLM platform engineer designing context management for [APPLICATION_OR_AGENT]. Optimize for task continuity, correctness, cost, and privacy—not simply for fitting the maximum possible history into each request.

Conversation/task patterns:
[EXAMPLES_OF_LONG_SESSIONS_OR_MULTISTEP_TASKS]
Model/API constraints:
[CONTEXT_LIMITS_IF_VERIFIED, TOKEN_BUDGET, LATENCY, STORAGE_RULES]
Critical facts and failure cases:
[WHAT_MUST_BE_PRESERVED, WHAT_WAS_LOST]

1. Classify information into current instructions, user-provided facts, task state, retrieved evidence, tool results, historical context, and disposable chatter.
2. Define a strategy for truncation, summarization, retrieval from history, and structured state storage. Explain which data should not be summarized because exact wording or provenance matters.
3. Design a state schema with source, timestamp/version, confidence or verification status, expiration, and deletion behavior where appropriate.
4. Prevent summaries from turning uncertain inferences into facts or allowing old instructions to override current instructions.
5. Define context assembly order, token budgeting, prioritization, and behavior when the required context will not fit.
6. Include privacy and consent rules for persistent memory, secrets, sensitive information, and cross-user isolation.
7. Create tests for conflicting facts, stale state, lost constraints, context overflow, summary distortion, and session restart.
8. Measure task success and state-recall accuracy alongside token usage and latency.

Return a context lifecycle diagram, state schema, selection algorithm or pseudocode, expiration/deletion policy, test suite, monitoring metrics, and implementation steps. Do not assume a provider's exact context limit unless verified. Do not store secrets in summaries or logs.
```

**Expected deliverable:** A context lifecycle design that preserves task-critical information and makes state selection and expiration explicit.

**Customization notes:** Include examples of facts that must persist, facts that must expire, and errors caused by lost context. Verify provider limits against current documentation.

---

## Prompt 032 — Test an LLM Application End to End

**Category:** AI Engineering & LLM Applications

**Use case:** Unit tests pass, but the full AI workflow still fails because retrieval, prompt assembly, model responses, tools, and downstream code interact unpredictably.

**Objective:** Build an end-to-end test strategy with deterministic fixtures where possible and controlled tests for nondeterministic model behavior.

**Prompt:**
```text
Act as a test architect for [LLM_APPLICATION]. Design an end-to-end testing strategy that catches failures across the complete request path without making the test suite prohibitively slow or expensive.

Architecture and stack:
[COMPONENTS, FRAMEWORKS, PROVIDERS]
Critical user journeys:
[WORKFLOWS]
Known failures and release constraints:
[BUGS, QUALITY BAR, CI TIME, BUDGET]

1. Map the full path from user input through validation, retrieval, prompt assembly, model call, tool execution, post-processing, persistence, and response rendering as applicable.
2. Divide tests into deterministic unit tests, integration tests with controlled dependencies, provider contract tests, evaluation tests, and a small set of live end-to-end smoke tests.
3. Use recorded fixtures or stubs where appropriate, but do not mistake a mock test for proof that the real provider behaves identically.
4. Define stable assertions for nondeterministic outputs: schema validity, required facts, forbidden claims, citations, tool-call constraints, and rubric-based checks instead of brittle full-string equality where unsuitable.
5. Cover timeouts, rate limits, malformed responses, refusals, empty retrieval, provider outage, duplicate requests, and partial failure.
6. Control cost and flakiness with bounded retries, explicit test budgets, isolated test data, and recorded model/configuration versions.
7. Define CI gates, quarantining rules for flaky tests, failure artifacts, and a process for adding production incidents to regression tests.

Deliver: test pyramid, test matrix, representative test cases, fixture policy, CI plan, cost controls, failure-report format, and release criteria. If repository access is available, inspect the existing tests before recommending changes. Report exactly what was executed; never say a test passed unless its result was observed.
```

**Expected deliverable:** A practical end-to-end testing plan that distinguishes deterministic correctness checks from model-quality evaluations and live integration risks.

**Customization notes:** Supply the architecture and high-value user journeys. Identify which tests may call paid APIs and what budget is acceptable in CI.

---

## Prompt 033 — Implement Streaming Responses with Correct Lifecycle Handling

**Category:** AI Engineering & LLM Applications

**Use case:** The interface should show tokens as they arrive, but streaming introduces cancellation, partial output, errors, and cleanup concerns.

**Objective:** Design or implement a streaming response path with explicit lifecycle states and safe failure behavior.

**Prompt:**
```text
Act as a full-stack engineer implementing streaming LLM responses for [APPLICATION] using [FRONTEND, BACKEND, PROVIDER/API].

Current implementation or repository context:
[CODE_OR_RELEVANT_FILES]
Expected user experience:
[STREAMING_BEHAVIOR]
Constraints:
[AUTH, TIMEOUTS, NETWORK, ACCESSIBILITY, DEPLOYMENT]

1. Confirm the provider's actual streaming protocol and the application's transport choice (for example, server-sent events or another supported mechanism). Do not assume API event formats.
2. Define lifecycle states: idle, connecting, streaming, completed, cancelled, failed, and optionally retrying. Specify allowed transitions.
3. Design incremental decoding and rendering so partial chunks, split UTF-8 characters, metadata events, tool events, and terminal events are handled correctly for the chosen protocol.
4. Define cancellation propagation from UI to server/provider, connection cleanup, request timeouts, backpressure, and behavior when the client disconnects.
5. Distinguish partial output from a completed answer. Never present a failed or truncated stream as a successful final result.
6. Handle authentication, authorization, rate limiting, and safe logging without exposing API keys or private prompt content.
7. Include accessibility considerations such as status announcements, keyboard use, and avoiding excessive screen-reader updates.
8. Add tests for chunk boundaries, empty streams, abrupt disconnects, provider errors, cancellation, duplicate submissions, and completion events.

Deliver a lifecycle diagram, protocol assumptions, implementation or patch if source context is available, error-handling table, tests, and manual verification steps. State which tests ran and which were only proposed. Do not invent provider-specific event names.
```

**Expected deliverable:** A streaming design or implementation that handles completion, cancellation, partial output, and failure transparently.

**Customization notes:** Provide your provider, framework, and relevant code. Verify current streaming protocol documentation before implementing event parsing.

---

## Prompt 034 — Design Safe and Reliable LLM Tool Calling

**Category:** AI Engineering & LLM Applications

**Use case:** A model can request functions or tools, but calls must be validated, authorized, and handled safely.

**Objective:** Specify a tool-calling layer that separates model intent from application permission and execution.

**Prompt:**
```text
Act as an AI application security engineer designing tool calling for [APPLICATION]. The model may propose tool calls, but the application—not the model—must enforce permissions and validate side effects.

Available tools and schemas:
[TOOL_NAMES, INPUTS, OUTPUTS, SIDE_EFFECTS]
User roles and authorization rules:
[ROLES_AND_PERMISSIONS]
Execution environment:
[STACK, TIMEOUTS, AUDIT, APPROVAL_REQUIREMENTS]

1. Review each tool's purpose, minimum required inputs, side effects, data exposure, and failure modes.
2. Define strict argument schemas and semantic validation, including bounds, identifiers, ownership checks, and rejection of unknown fields where appropriate.
3. Enforce user authorization and policy checks in application code before execution. Never rely on the model's statement that a user is authorized.
4. Classify tools by risk. Require explicit confirmation for consequential or irreversible actions, and use dry-run/preview behavior where practical.
5. Treat tool output and retrieved content as untrusted data. Prevent tool results from injecting new instructions or bypassing authorization.
6. Define idempotency keys, duplicate-call handling, timeouts, retries, compensation, and partial-failure reporting.
7. Minimize returned data and redact secrets or sensitive values from logs. Record who requested the action, validated arguments, outcome, and approval reference as appropriate.
8. Test invalid arguments, unauthorized resources, repeated calls, malicious tool output, unavailable services, and ambiguous confirmation.

Deliver a tool registry schema, call lifecycle, risk matrix, validation pseudocode, approval policy, audit event schema, test cases, and rollout checklist. Do not execute real side effects while merely designing the system. Do not claim security is guaranteed; identify residual risks and required review.
```

**Expected deliverable:** A permission-enforced tool-calling design with clear approval boundaries, validation, idempotency, and auditability.

**Customization notes:** List every tool and mark which ones can change external state. Specify actions that always require human confirmation.

---

## Prompt 035 — Create a Model-Provider Abstraction Layer

**Category:** AI Engineering & LLM Applications

**Use case:** An application needs to support more than one model provider without scattering provider-specific logic across the codebase.

**Objective:** Design an abstraction that centralizes common behavior while preserving meaningful provider-specific differences.

**Prompt:**
```text
Act as a software architect designing a model-provider abstraction for [APPLICATION] in [LANGUAGE/FRAMEWORK]. The goal is to reduce coupling without pretending all providers have identical capabilities.

Current provider code or interfaces:
[CODE_OR_INTERFACE_DESCRIPTIONS]
Required features:
[TEXT, STREAMING, STRUCTURED_OUTPUTS, TOOLS, MULTIMODAL, ETC.]
Operational constraints:
[FAILOVER, LOGGING, COST, PRIVACY, DEPLOYMENT]

1. Identify the stable application-level contract and the provider-specific features that cannot be safely normalized away.
2. Propose interfaces for request, response, streaming events, usage metadata, tool calls, errors, cancellation, and model capabilities.
3. Define a capability registry so unsupported features fail clearly or use an explicit fallback instead of silently degrading.
4. Normalize common errors while preserving provider code, retryability, request ID, and diagnostic context where safe.
5. Design configuration, secret injection, model/version selection, timeout settings, and per-provider limits.
6. Define retry and failover rules. Do not retry non-idempotent operations blindly or route data to another provider if policy or residency constraints prohibit it.
7. Build contract tests that each provider adapter must pass, plus provider-specific tests for unique features.
8. Explain how to add a new provider without changing unrelated business logic and how to avoid an abstraction that becomes a lowest-common-denominator bottleneck.

Deliver interface definitions or pseudocode, adapter structure, capability matrix, error taxonomy, configuration example without real secrets, contract-test plan, migration steps, and trade-offs. If code is requested, use the existing repository conventions. Never claim compatibility with a provider feature that has not been verified.
```

**Expected deliverable:** A maintainable provider boundary with explicit capabilities, error handling, and safe fallback behavior.

**Customization notes:** Share current provider integrations and required features. Identify any data that must never be sent to a particular provider.

---

## Prompt 036 — Optimize LLM Cost and Latency Without Breaking Quality

**Category:** AI Engineering & LLM Applications

**Use case:** Model usage is expensive or slow, and the team needs to find the largest savings without damaging user outcomes.

**Objective:** Create a measurement-led optimization plan for tokens, calls, retrieval, caching, routing, and response latency.

**Prompt:**
```text
Act as an LLM performance and cost engineer. Diagnose and optimize [APPLICATION] using measured workload data. Do not recommend cost cuts that undermine the product's minimum quality or safety requirements.

Usage and traces:
[REQUEST_VOLUME, INPUT/OUTPUT TOKENS, LATENCY DISTRIBUTIONS, ERRORS, COST DATA]
Quality baseline:
[EVALUATION_RESULTS_OR_ACCEPTANCE_CRITERIA]
Constraints:
[MONTHLY_BUDGET, P95_LATENCY, PRIVACY, FEATURE_REQUIREMENTS]

1. Establish a baseline by workflow, model, request size, retries, cache hit rate, and failure rate. Flag missing measurements before estimating savings.
2. Decompose end-to-end latency into queueing, retrieval, prompt assembly, model time-to-first-token, generation, tools, and post-processing where telemetry permits.
3. Evaluate high-impact options: shorter context, prompt deduplication, retrieval filtering, caching, batching, smaller-model routing, output limits, parallel calls, and reduced unnecessary retries.
4. For each option, estimate the mechanism of savings, likely quality risks, implementation effort, and how to test the effect. Use formulas and ranges if exact data is missing.
5. Define cache keys and invalidation rules that prevent cross-user leakage and stale answers. Do not cache sensitive outputs without an approved policy.
6. Compare cost per successful task—not only cost per request—and include retries, fallbacks, and failed outputs.
7. Run controlled experiments against the same evaluation set. Establish guardrails for quality, refusal correctness, latency percentiles, and error rate.
8. Prioritize changes by expected impact and confidence; distinguish quick wins from changes requiring architectural work.

Return a baseline table, cost/latency decomposition, prioritized experiment backlog, formulas, instrumentation gaps, rollout plan, and rollback triggers. Do not invent savings percentages or state that an optimization worked until measured.
```

**Expected deliverable:** A prioritized, evidence-based optimization plan with quality guardrails and measurable success criteria.

**Customization notes:** Supply real usage logs or aggregate metrics, excluding secrets and unnecessary personal data. If no baseline exists, begin with an instrumentation plan.

---

## Prompt 037 — Design Error Handling for Unreliable Model APIs

**Category:** AI Engineering & LLM Applications

**Use case:** Provider timeouts, rate limits, malformed outputs, and intermittent failures cause unpredictable behavior in an AI feature.

**Objective:** Define a resilient error-handling policy that distinguishes retryable failures from permanent or unsafe ones.

**Prompt:**
```text
Act as a reliability engineer for the model API integration in [APPLICATION]. Design error handling for [PROVIDER/SDK IF KNOWN] that protects users and prevents retry storms or duplicate side effects.

Current request flow and errors:
[CODE, LOGS WITH SECRETS REMOVED, OR FAILURE DESCRIPTIONS]
Service objectives:
[AVAILABILITY, LATENCY, COST, USER EXPERIENCE]
Side effects and fallback options:
[TOOLS, DATABASE WRITES, ALTERNATE MODELS, MANUAL PATH]

1. Classify failures into validation errors, authentication/configuration errors, rate limits, timeouts, transient server errors, content/refusal outcomes, malformed responses, cancellation, and unknown failures.
2. For each class, specify whether to retry, fail fast, request user input, use a fallback, or escalate.
3. Define bounded exponential backoff with jitter where appropriate, honoring provider retry guidance when verified. Add a retry budget and overall deadline.
4. Prevent duplicate side effects by separating model generation from execution and using idempotency controls where applicable.
5. Define circuit-breaker or load-shedding behavior if useful for the service's scale. Avoid adding resilience mechanisms without a clear failure mode they address.
6. Specify user-facing errors that are honest and actionable without exposing internals, prompts, or secrets.
7. Add structured telemetry for error class, provider, model/version, request correlation ID, attempt count, latency, and outcome while minimizing sensitive content.
8. Test timeout, rate-limit, malformed response, authentication failure, provider outage, cancellation, retry exhaustion, and fallback failure.

Deliver an error policy matrix, pseudocode or implementation plan, telemetry schema, test cases, operational alerts, and recovery runbook. Verify provider-specific error codes against current documentation. Report what was actually tested and what remains unverified.
```

**Expected deliverable:** A bounded, observable failure policy that improves resilience without hiding errors or repeating unsafe operations.

**Customization notes:** Include sanitized error logs and service-level objectives. Specify whether fallback models or manual workflows are permitted.

---

## Prompt 038 — Add Observability to an AI Application

**Category:** AI Engineering & LLM Applications

**Use case:** Users report poor answers, but the team cannot tell whether the cause is retrieval, prompt construction, model behavior, tool execution, or downstream processing.

**Objective:** Design traces, metrics, and logs that make AI failures diagnosable without exposing sensitive data.

**Prompt:**
```text
Act as an observability architect for [LLM_APPLICATION]. Design instrumentation that lets engineers trace a user request across the AI pipeline and diagnose quality, reliability, latency, and cost issues while respecting privacy.

System components:
[COMPONENTS_AND_DEPENDENCIES]
Operational questions:
[QUESTIONS_THE_TEAM_CANNOT_CURRENTLY_ANSWER]
Privacy and compliance constraints:
[DATA_CLASSIFICATION, RETENTION, ACCESS, REDACTION]

1. Define a request correlation model spanning ingress, retrieval, prompt assembly, model calls, tool calls, validation, persistence, and final response.
2. Propose metrics for request volume, error classes, latency percentiles, time-to-first-token, token usage, cost estimates, retries, timeouts, cache behavior, retrieval outcomes, schema failures, and user-visible completion.
3. Define trace spans and safe attributes. Avoid placing raw prompts, full documents, credentials, or personal data in telemetry by default.
4. Specify what content may be sampled or redacted for debugging, who can access it, how long it is retained, and how deletion requests propagate.
5. Separate operational observability from quality evaluation. Define how a privacy-approved sample can be reviewed for groundedness or usefulness without treating telemetry metrics as proof of answer quality.
6. Create dashboards and alerts tied to actionable thresholds; explain how thresholds should be set from a baseline rather than arbitrary universal values.
7. Add a request-level diagnostic record that captures model/version, prompt/template version, retrieval/index version, tool outcomes, validation result, and final status where available.
8. Define incident triage steps and tests that verify sensitive values are not logged.

Deliver an instrumentation map, metric dictionary, trace schema, dashboard and alert plan, privacy controls, incident checklist, and rollout phases. Mark thresholds that need baseline data. Do not claim observability is complete without verifying emitted telemetry.
```

**Expected deliverable:** A practical observability blueprint that connects user-visible failures to pipeline stages while limiting data exposure.

**Customization notes:** List current monitoring tools and what engineers need to diagnose. Have security or privacy owners review the logging policy before production use.

---

## Prompt 039 — Build a Multimodal LLM Workflow

**Category:** AI Engineering & LLM Applications

**Use case:** An application needs to interpret combinations of text, images, audio, video frames, or documents, and must handle modality-specific limitations.

**Objective:** Design a multimodal pipeline with explicit input validation, modality handling, evidence traceability, and evaluation.

**Prompt:**
```text
Act as a multimodal AI systems engineer. Design a workflow for [USER_TASK] using these input modalities: [TEXT, IMAGE, AUDIO, VIDEO, DOCUMENT, OR OTHER SUPPORTED TYPES]. Do not assume the chosen model supports every modality or every operation.

Inputs and expected outputs:
[INPUT_EXAMPLES_AND_OUTPUT_SCHEMA]
Candidate tools/models:
[CANDIDATES_OR_UNDECIDED]
Constraints:
[FILE_SIZE, LATENCY, COST, PRIVACY, ACCESSIBILITY, STORAGE]

1. Confirm the actual task for each modality and distinguish direct model interpretation from preprocessing such as OCR, transcription, frame extraction, or metadata parsing.
2. Identify supported formats, size/duration limits, sampling needs, resolution or audio-quality concerns, and provider constraints that require current verification.
3. Design validation for file type, size, corruption, orientation, timestamps, missing tracks, empty content, and malicious uploads.
4. Define synchronization and provenance when information comes from multiple modalities. Preserve page numbers, timestamps, frame references, or text spans when available.
5. Specify how the system should report uncertainty, missing modality data, contradictory signals, and unsupported requests.
6. Consider privacy, consent, retention, access control, and whether data is sent to external providers.
7. Create evaluation cases for normal inputs, noisy inputs, edge cases, cross-modal contradictions, and adversarial content. Evaluate each modality and the combined task.
8. Define graceful degradation when one modality fails, plus cost and latency instrumentation for each processing stage.

Deliver a pipeline diagram, modality-support matrix, input contract, processing sequence, evidence/provenance strategy, failure policy, evaluation plan, and implementation checklist. Do not claim the selected model supports a feature until verified. Do not infer facts that the input does not support.
```

**Expected deliverable:** A modality-aware design that preserves traceability and defines safe behavior when inputs are noisy, incomplete, or unsupported.

**Customization notes:** State the actual modalities and user task. Verify model capabilities, limits, and data handling terms before selecting a provider.

---

## Prompt 040 — Run an LLM Application Production-Readiness Review

**Category:** AI Engineering & LLM Applications

**Use case:** A prototype works in a demo, but the team needs a structured review before exposing it to real users or sensitive data.

**Objective:** Identify launch blockers and create evidence-based release gates across quality, security, reliability, cost, and operations.

**Prompt:**
```text
Act as an independent production-readiness reviewer for [LLM_APPLICATION]. Review the supplied design, repository, tests, configuration, and operational plan. Be skeptical: a successful demo is not evidence of production readiness.

Materials available:
[ARCHITECTURE, REPOSITORY/FILES, TEST RESULTS, EVALUATION RESULTS, RUNBOOKS]
Expected users and risk level:
[USERS, DATA SENSITIVITY, CONSEQUENCES OF FAILURE]
Launch constraints:
[DATE, BUDGET, SERVICE TARGETS, SUPPORT CAPACITY]

Inspect or request evidence for:
1. Product scope, known limitations, and user-facing disclosure.
2. Model/provider versioning, prompt version control, evaluation coverage, and regression gates.
3. Grounding, retrieval quality, citation integrity, structured-output validation, refusal behavior, and handling of uncertainty where relevant.
4. Authentication, authorization, tenant isolation, prompt injection, tool permissions, secrets, data retention, and logging privacy.
5. Timeouts, retries, rate limits, cancellation, provider outage, backup/restore, and dependency failures.
6. Cost controls, usage quotas, abuse prevention, and monitoring for unexpected spend.
7. Deployment repeatability, configuration separation, rollback, incident ownership, support procedures, and alert routing.
8. Accessibility, user feedback, data deletion, and any applicable compliance obligations identified by the organization.

Classify findings as launch blocker, high, medium, or low, with evidence, user impact, remediation, owner role, and a concrete verification test. Distinguish verified findings from suspected risks and missing evidence. Do not invent test results or imply legal certification. End with a go/no-go recommendation conditional on explicit gates, residual risks, and rollback criteria.

Output: executive decision, scope reviewed, evidence inventory, risk register, required fixes, release gates, post-launch monitoring plan, and sign-off checklist. If critical evidence is unavailable, state that readiness cannot be confirmed.
```

**Expected deliverable:** A production-readiness report with prioritized findings, verifiable release gates, and an evidence-based go/no-go recommendation.

**Customization notes:** Provide the most current architecture, evaluation reports, test output, and deployment procedures. Redact secrets. A missing artifact should be recorded as missing, not assumed to pass.

---

# Batch 2 Completion Manifest

- **Completed:** Prompts 021–040
- **Category:** AI Engineering & LLM Applications
- **Count:** 20 prompts
- **Unfinished entries in this batch:** None
- **Next batch:** Batch 3 — Prompts 041–060, AI Agents & Automation

## Batch 2 Scope Audit

| Prompt IDs | Focus |
|---|---|
| 021 | LLM application architecture |
| 022 | Prompt template systems |
| 023 | Structured outputs and validation |
| 024 | Retrieval-augmented generation |
| 025 | Embedding and indexing pipelines |
| 026 | Semantic search relevance |
| 027 | Document extraction pipelines |
| 028 | Evaluation dataset design |
| 029 | Workload-based model comparison |
| 030 | Unsupported claims and hallucination mitigation |
| 031 | Context and conversation-state management |
| 032 | End-to-end AI testing |
| 033 | Streaming response lifecycle |
| 034 | Safe tool calling |
| 035 | Provider abstraction |
| 036 | Cost and latency optimization |
| 037 | API error handling and resilience |
| 038 | AI observability |
| 039 | Multimodal workflows |
| 040 | Production-readiness review |

**Quality note:** This batch is drafted to the master brief. A final cross-batch duplication audit and hands-on validation of any implementation examples should be completed before the full 200-prompt collection is marketed as fully audited.
