# EMUAdvisor - Local Regulation RAG Assistant

## Overview

EMUAdvisor is a local-only RAG assistant for answering Eastern Mediterranean University regulation questions from official cited sources.

The system is designed for staff-facing regulation lookup, not as a general university chatbot. Its purpose is to retrieve official evidence, answer in plain language, cite sources, and refuse or clarify when the available evidence is weak or out of scope.

## Problem

University regulation questions are high-risk for a generic chatbot. A useful assistant cannot simply generate a plausible answer. It needs to know:

- which sources are allowed,
- which language corpus should be searched,
- whether the retrieved evidence is strong enough,
- when to answer,
- when to ask for clarification,
- when to refuse,
- how to cite the answer,
- how to keep every answer traceable to source material.

The main engineering challenge is not connecting documents to an LLM. The real challenge is building the boundaries around the answer.

## Goal

Build a local-only assistant that answers EMU staff questions about official rules and regulations using cited evidence from official HTML pages and PDFs.

The assistant should:

- answer in detailed plain language,
- quote or summarize relevant regulation text,
- provide citations,
- preserve English/Turkish source boundaries,
- avoid unsupported policy interpretation,
- refuse out-of-scope questions,
- ask clarification questions when needed.

## Current Status

EMUAdvisor is a demo-stage local prototype.

The current demo corpus includes:

- 8,714 active chunks,
- 119 official sources,
- 22 PDFs,
- English and Turkish corpora,
- structured table evidence,
- row-level table chunks,
- derived facts for selected regulation comparisons,
- assistant-curated evaluation sets pending human review.

The extractive answer path is the validated continuity path. Local generated answers are implemented through Ollama, but the system is designed to fall back safely to extractive/cited answers when local generation is unavailable or too slow.

## Product Boundary

### In Scope

- EMU rules and regulations
- official `mevzuat.emu.edu.tr` HTML pages
- official PDFs linked from or belonging to the regulation source set
- English and Turkish regulation corpora
- cited answers
- clarification and refusal behavior
- source traceability
- local-only demo deployment

### Out of Scope for V1

- general university chatbot behavior
- student or staff advising
- program/course/event questions
- private student or staff records
- external APIs
- final legal or administrative decisions
- silent mixing of English and Turkish regulatory sources

## Architecture

```text
Official sources
  ├─ mevzuat HTML
  └─ official PDFs
       ↓
Ingestion
  ├─ crawler
  ├─ extractor
  ├─ PDF handling
  ├─ metadata extraction
  ├─ deduplication
  └─ version hashing
       ↓
Document/chunk layer
  ├─ language metadata
  ├─ source metadata
  ├─ citation metadata
  └─ crawl/version metadata
       ↓
Retrieval
  ├─ language routing
  ├─ corpus selection
  ├─ hybrid retrieval
  ├─ Qdrant/FAISS indexing direction
  └─ confidence checks
       ↓
Answerability gate
  ├─ strong evidence → answer
  ├─ medium evidence → clarify or answer with uncertainty
  ├─ weak evidence → refuse or redirect
  └─ conflict → show conflict
       ↓
Answer layer
  ├─ extractive answer
  ├─ optional local LLM generation
  ├─ fallback behavior
  └─ bottom citations
```

## Main Features

- FastAPI backend
- local web UI
- local-only LLM/Ollama workflow
- hybrid retrieval
- Qdrant/FAISS indexing direction
- language routing
- citation-aware answers
- retrieval thresholds
- clarification/refusal flow
- conflict display behavior
- source/version traceability
- local evaluation scripts
- demo-readiness reporting

## Design Decisions

### Evidence First

The system treats retrieved evidence as the primary object. The LLM is not allowed to behave like an unconstrained answer generator. The assistant should answer only when the retrieved sources support the answer.

### Separate English and Turkish Corpora

English and Turkish EMU regulations are not assumed to be exact translations. The assistant routes by query language and avoids silently combining legal/regulatory sources across languages.

### Controlled Failure

The assistant is expected to fail safely. If the query is out of scope or evidence is insufficient, it should clarify, refuse, or redirect instead of inventing an answer.

### Traceability

Every substantive answer should be traceable to source document, source URL/path, source version/hash, crawl timestamp, and retrieved chunks.

### Demo-Readiness vs. Production-Readiness

The local demo is presentable, but production deployment requires human-reviewed evaluation, validated deployment hardware, service-backed vector infrastructure, source-diff review, and administrative update controls.

## Evaluation

The evaluation surface tracks:

- retrieval top-1 / top-3 / top-5,
- response accuracy,
- rejection accuracy,
- clarification behavior,
- citation coverage,
- extractive latency,
- failed cases,
- human-review status.

The current evaluation sets are useful for development and demo validation. They are not presented as final production-grade gold labels until manually reviewed.

## What I Learned

The difficult part of RAG is not retrieval alone and not generation alone. The difficult part is the system contract: source scope, evidence quality, answerability, refusal behavior, citation discipline, and traceability.

EMUAdvisor taught me to think about AI systems as workflows with boundaries, not just interfaces that produce text.
