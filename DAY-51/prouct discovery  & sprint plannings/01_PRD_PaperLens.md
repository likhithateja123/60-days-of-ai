# Product Requirements Document (PRD)
## PaperLens Lite

---

## 1. Executive Summary

PaperLens Lite is a lightweight AI-powered web application that helps researchers and grad students quickly understand dense academic papers. A user uploads a single PDF, receives an instant AI-generated summary, and can ask follow-up questions in a chat interface — with every answer grounded in and cited back to the exact passage in the source document.

The goal of the 10-day capstone is to ship a working, deployed, end-to-end demo — not a production system.

---

## 2. Problem Statement

Reading and digesting academic papers is slow and cognitively expensive. Researchers and grad students conducting literature reviews often need to:
- Quickly assess whether a paper is relevant
- Extract key findings without reading the full text
- Verify specific claims by tracing them back to source passages

Existing tools (plain LLM chat, PDF readers) either lack grounding (hallucinate answers) or lack citation traceability, forcing users to manually re-search the PDF to verify AI-generated answers.

---

## 3. Target Users

| User Type | Description | Primary Need |
|---|---|---|
| Grad students | Conducting literature reviews for thesis/coursework | Fast comprehension + citation trust |
| Researchers | Screening papers for relevance before deep reading | Quick, trustworthy summaries |

---

## 4. Objectives

1. Enable a user to upload one PDF and receive a clear, accurate summary.
2. Allow the user to ask natural-language questions about the paper.
3. Ground every answer in retrieved source text using embeddings + vector search.
4. Provide inline citations so users can verify AI answers against the original document.
5. Deploy a working, publicly accessible demo by Day 10.

---

## 5. User Stories

| ID | As a... | I want to... | So that... |
|---|---|---|---|
| US-1 | Grad student | Upload a research paper as a PDF | I can start analyzing it immediately |
| US-2 | Grad student | See an AI-generated summary right after upload | I can quickly judge the paper's relevance |
| US-3 | Researcher | Ask specific questions about the paper's content | I don't have to read the whole document |
| US-4 | Researcher | See which section/passage an answer came from | I can trust and verify the AI's response |
| US-5 | Any user | Use the tool without creating an account | I can start immediately with no friction |

---

## 6. Functional Requirements

| ID | Requirement | Priority |
|---|---|---|
| FR-1 | System shall accept a single PDF upload | Must |
| FR-2 | System shall extract text from the uploaded PDF | Must |
| FR-3 | System shall chunk extracted text into retrievable segments | Must |
| FR-4 | System shall generate embeddings for each chunk | Must |
| FR-5 | System shall store embeddings in a vector store (local/open-source) | Must |
| FR-6 | System shall generate a summary of the full paper | Must |
| FR-7 | System shall accept user questions via a chat interface | Must |
| FR-8 | System shall retrieve relevant chunks for each question | Must |
| FR-9 | System shall generate an answer grounded in retrieved chunks | Must |
| FR-10 | System shall display a citation (e.g., section/snippet reference) with each answer | Must |
| FR-11 | System shall handle basic errors (e.g., unsupported file, empty PDF) gracefully | Should |

---

## 7. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-1 | Summary generation should complete within ~10–15 seconds for an average paper |
| NFR-2 | Q&A response should complete within ~5–10 seconds per query |
| NFR-3 | Application must run using free/open-source tools only (no paid API dependency required) |
| NFR-4 | Application must be deployable as a simple web app with no authentication |
| NFR-5 | System should handle papers up to a reasonable length (e.g., ~20–30 pages) without failure |
| NFR-6 | UI should be simple and functional; polish is not a priority for v1 |

---

## 8. Success Metrics

| Metric | Target |
|---|---|
| End-to-end flow completion | Upload → Summary → 3+ Q&A exchanges without crash |
| Citation accuracy | Answers correctly reference the passage they were grounded in |
| Deployment status | Live, publicly accessible link functional on Day 10 |
| Demo reliability | No crashes during a live walkthrough of the core flow |

---

## 9. Risks

| Risk | Impact | Mitigation |
|---|---|---|
| PDF parsing fails on complex layouts (tables, columns) | Medium | Test early with real papers; pick robust parsing library |
| Embedding/vector search setup takes longer than expected | High | Use well-documented open-source libraries; timebox setup to Day 2–3 |
| LLM hallucinates despite retrieval grounding | Medium | Use strict prompting to answer only from retrieved context |
| Scope creep toward multi-doc or advanced features | High | Explicitly enforce "Out of Scope" list throughout build |
| Free-tier API/rate limits slow development | Medium | Cache embeddings; avoid redundant calls during testing |

---

## 10. Future Scope (Post Day-10)

- Multi-document upload and cross-paper comparison
- User accounts and saved session history
- Citation confidence scoring
- Re-ranking models for improved retrieval accuracy
- Mobile-responsive polished UI
- Export summary/Q&A as a report

---

## ✅ Review Checklist

- [ ] Problem statement is specific and validated against target users
- [ ] All functional requirements are testable
- [ ] Non-functional requirements are realistic for a solo, 3–4 hr/day, 10-day build
- [ ] Success metrics are measurable on Day 10
- [ ] Risks have clear mitigations
- [ ] Out-of-scope items are explicitly excluded from requirements above
