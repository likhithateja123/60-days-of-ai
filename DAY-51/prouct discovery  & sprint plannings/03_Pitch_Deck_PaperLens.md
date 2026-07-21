# PaperLens Lite
### Understand any research paper in minutes — not hours.

---

## Slide 1 — Title

# PaperLens Lite
**AI-powered research paper comprehension, with trustworthy citations.**

*10-Day Capstone Project*

---

## Slide 2 — The Problem

Reading academic papers is slow, dense, and cognitively expensive.

- Researchers and grad students spend hours per paper during literature reviews
- Key findings are buried in jargon-heavy sections
- Verifying specific claims means manually re-scanning the entire PDF
- Time pressure often forces skimming — risking missed insights or misunderstood claims

**Core pain point:** *Understanding takes too long, and verifying AI or human summaries is tedious.*

---

## Slide 3 — Existing Solutions

| Solution | Limitation |
|---|---|
| Reading the paper manually | Time-consuming, doesn't scale across many papers |
| Generic LLM chat (e.g., pasting text into ChatGPT) | No grounding — prone to hallucinated or generic answers |
| PDF readers with search | Keyword-based only, no semantic understanding or Q&A |
| Existing "chat with PDF" tools | Often lack transparent citations, hard to verify trustworthiness |

**Gap:** No simple tool combines AI comprehension **with** transparent, verifiable citations back to source text.

---

## Slide 4 — Proposed Solution

# PaperLens Lite

Upload one research paper → get an instant AI summary → ask follow-up questions → receive answers **grounded in and cited to the exact source passage.**

**Key differentiator:** Every AI answer is traceable back to where it came from in the document — building trust, not just convenience.

---

## Slide 5 — Target Users

| User | Need |
|---|---|
| **Grad students** | Fast literature review comprehension |
| **Researchers** | Quick relevance screening before deep reading |

**Primary use case:** Literature review — screening and understanding papers faster, with confidence in the answers provided.

---

## Slide 6 — Features (v1)

✅ Included in this build:
- Single PDF upload
- AI-generated full-paper summary
- Conversational Q&A about the paper's content
- Inline citations linking every answer to its source passage
- Simple, no-login web interface

❌ Explicitly out of scope for v1:
- Multi-document support
- User accounts / saved history
- Advanced re-ranking or confidence scoring
- Polished responsive design

---

## Slide 7 — High-Level Architecture

**Flow:** PDF Upload → Text Extraction → Chunking → Embeddings → Vector Store → Retrieval → LLM Generation (Summary / Q&A) → Cited Output

| Stage | Purpose |
|---|---|
| Ingestion | Extract readable text from PDF |
| Chunking | Break text into retrievable, citable segments |
| Embedding + Vector Store | Enable semantic search over the document |
| Retrieval | Pull the most relevant chunks per query |
| Generation | Produce grounded summaries and answers |
| Citation Layer | Attach source references to every output |

*(Built entirely with free/open-source tools, deployed as a simple no-login web app.)*

---

## Slide 8 — Expected Impact

- Cuts initial paper comprehension time significantly for researchers and students
- Increases trust in AI-assisted reading through visible, verifiable citations
- Demonstrates a practical, scoped application of Retrieval-Augmented Generation (RAG) — a core, in-demand AI engineering pattern

---

## Slide 9 — Future Scope

- Multi-document upload and cross-paper comparative Q&A
- User accounts with saved reading/question history
- Citation confidence scoring
- Improved retrieval via re-ranking models
- Export summaries and Q&A sessions as shareable reports
- Mobile-responsive, polished UI

---

## Slide 10 — Vision

**PaperLens aims to become the trusted first stop for anyone facing a dense document** — starting with one paper and one researcher, and growing toward a full research companion that makes rigorous understanding faster and more transparent for everyone.

---

## ✅ Review Checklist

- [ ] Problem and solution are clearly and concisely stated
- [ ] Features slide matches the approved v1 scope exactly
- [ ] Architecture slide contains no implementation-level detail (high-level only)
- [ ] Deck tells a coherent story: problem → gap → solution → impact → vision
- [ ] Ready for live presentation (no filler slides, no code, no mockups)
