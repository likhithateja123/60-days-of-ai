# Implementation Blueprint (Days 2–10)
## PaperLens Lite

> Day 1 was discovery, validation, and planning (this document set). Building begins Day 2.
> Each day builds directly on the previous day's output — no day should be skipped or reordered.

---

## Day 2 — PDF Ingestion & Text Extraction

**Objective:** Get raw text reliably out of an uploaded PDF.

**Learning Goals:**
- Understand PDF text extraction challenges (columns, headers/footers, tables)
- Learn basic file handling for uploads

**Features:**
- Single PDF upload
- Raw text extraction from PDF

**Implementation Steps:**
1. Set up project folder structure
2. Implement file upload handling
3. Integrate a PDF text extraction library
4. Extract and print raw text to verify quality
5. Handle basic edge cases (empty file, corrupted PDF, non-PDF upload)

**Files/Folders:**
- `/app` (main application folder)
- `/app/uploads` (temporary storage for uploaded PDFs)
- `/app/ingestion` (extraction logic)

**Tools/APIs:**
- Open-source PDF text extraction library (evaluate options during this day)

**Testing:**
- Upload 3–5 real research papers with varying layouts
- Confirm extracted text is readable and roughly matches PDF content

**Debugging Tips:**
- If text is garbled, check if PDF is scanned (image-based) vs. text-based
- Strip excessive whitespace/line breaks before moving to chunking

**Checklist:**
- [ ] Upload mechanism works
- [ ] Text extracted from at least 3 sample papers
- [ ] Edge cases handled without crashing

**Expected Screenshots:**
- Upload interface (basic/unstyled)
- Console/log output showing extracted raw text

**Handoff Notes:**
- Extracted text (per uploaded paper) is the input for Day 3's chunking step. Keep extraction output in a simple, consistent format (e.g., plain string or list of pages).

---

## Day 3 — Text Chunking & Preprocessing

**Objective:** Convert raw extracted text into clean, retrievable chunks.

**Learning Goals:**
- Understand chunking strategies (fixed-size vs. semantic/paragraph-based)
- Learn why chunk size affects retrieval quality

**Features:**
- Text cleaning (remove noise, normalize whitespace)
- Chunking logic with overlap

**Implementation Steps:**
1. Clean raw text from Day 2 (remove headers/footers noise if feasible)
2. Split text into chunks (e.g., paragraph or fixed-token-based with overlap)
3. Attach metadata to each chunk (e.g., approximate section/page reference)
4. Store chunks in a simple structured format (list of dicts/objects)

**Files/Folders:**
- `/app/processing` (chunking logic)

**Tools/APIs:**
- Text splitting utility (open-source library or simple custom logic)

**Testing:**
- Verify chunk count is reasonable for paper length (not too fragmented, not too large)
- Spot-check that chunk metadata (section/page) is accurate

**Debugging Tips:**
- If chunks cut off mid-sentence, adjust overlap or splitting logic
- Keep chunk size moderate — too large hurts retrieval precision, too small loses context

**Checklist:**
- [ ] Chunking function implemented and tested
- [ ] Chunks include metadata for citation purposes later
- [ ] Chunk quality verified on all Day 2 sample papers

**Expected Screenshots:**
- Sample printed output of chunked text with metadata

**Handoff Notes:**
- Chunk objects (text + metadata) are the direct input for Day 4's embedding step. Keep the chunk schema stable — later days depend on it.

---

## Day 4 — Embeddings & Vector Store Setup

**Objective:** Convert chunks into embeddings and store them for retrieval.

**Learning Goals:**
- Understand what embeddings represent and how similarity search works
- Learn basic vector database operations (insert, query)

**Features:**
- Embedding generation for each chunk
- Vector store insertion and similarity search

**Implementation Steps:**
1. Choose an open-source embedding model
2. Generate embeddings for all chunks from Day 3
3. Set up a local/open-source vector store
4. Insert chunk embeddings + metadata into the vector store
5. Run a manual test query to confirm similarity search returns relevant chunks

**Files/Folders:**
- `/app/embeddings`
- `/app/vectorstore`

**Tools/APIs:**
- Open-source embedding model
- Open-source vector database (local-friendly)

**Testing:**
- Run 3–5 manual test queries against a sample paper
- Confirm top retrieved chunks are topically relevant

**Debugging Tips:**
- If retrieval feels off, double-check embeddings are generated consistently (same model for storage and query)
- Normalize/preprocess query text similarly to chunk text

**Checklist:**
- [ ] Embeddings generated for all chunks
- [ ] Vector store stores and retrieves chunks correctly
- [ ] Manual similarity search test passes

**Expected Screenshots:**
- Terminal output showing a query and its top-matching chunks

**Handoff Notes:**
- This retrieval pipeline (query → embed → search → top chunks) is the core dependency for Day 5's summary generation and Day 6's Q&A logic.

---

## Day 5 — Summary Generation

**Objective:** Generate a coherent AI summary of the full paper.

**Learning Goals:**
- Learn prompt design for summarization tasks
- Understand handling long documents within LLM context limits

**Features:**
- Full-paper summary generation after upload

**Implementation Steps:**
1. Decide summarization strategy (e.g., summarize chunks then combine, or summarize top representative sections)
2. Write and test summarization prompt
3. Connect summarization step to the ingestion pipeline (Day 2–4 output)
4. Display generated summary in a basic output area

**Files/Folders:**
- `/app/summarization`

**Tools/APIs:**
- LLM API/model used for generation

**Testing:**
- Generate summaries for all sample papers
- Manually review summaries for coherence and accuracy against the source

**Debugging Tips:**
- If summary is too generic, refine prompt to ask for key findings, methods, and conclusions explicitly
- If context limits are hit, summarize in stages (chunk-level summaries → final combination)

**Checklist:**
- [ ] Summarization pipeline connected end-to-end (upload → summary)
- [ ] Summaries reviewed for quality on all sample papers
- [ ] Output displayed to user in basic UI

**Expected Screenshots:**
- Basic UI showing uploaded paper title + generated summary

**Handoff Notes:**
- Confirm the ingestion → retrieval pipeline works reliably before moving to Q&A, since Day 6 reuses this exact retrieval mechanism.

---

## Day 6 — Q&A Core Logic (Retrieval-Augmented Answering)

**Objective:** Allow users to ask questions and get answers grounded in retrieved chunks.

**Learning Goals:**
- Understand the full RAG loop: query → retrieve → generate
- Learn prompt design for grounded, non-hallucinated answers

**Features:**
- Question input
- Retrieval of relevant chunks per question
- LLM-generated answer using only retrieved context

**Implementation Steps:**
1. Build a function that takes a user question and retrieves top-N relevant chunks
2. Design a prompt that instructs the LLM to answer **only** using provided chunks
3. Connect retrieval output to the answer-generation prompt
4. Test with sample questions per paper

**Files/Folders:**
- `/app/qa`

**Tools/APIs:**
- Same LLM API as Day 5
- Same vector store as Day 4

**Testing:**
- Ask 5+ questions per sample paper
- Verify answers are relevant and grounded (not hallucinated)

**Debugging Tips:**
- If answers hallucinate, strengthen prompt constraints ("If the answer is not in the context, say so")
- If retrieval misses relevant chunks, revisit chunk size/overlap from Day 3

**Checklist:**
- [ ] Q&A function implemented and connected to retrieval
- [ ] Answers grounded in retrieved chunks, not general knowledge
- [ ] Tested across all sample papers with varied questions

**Expected Screenshots:**
- Terminal/basic UI showing a question and its grounded answer

**Handoff Notes:**
- The answer generation function must also return which chunk(s) were used — this is required for Day 7's citation feature.

---

## Day 7 — Citation Linking

**Objective:** Attach visible citations to each AI answer, pointing back to source text.

**Learning Goals:**
- Learn how to trace generated output back to source metadata
- Understand UX patterns for displaying citations

**Features:**
- Inline citation shown alongside each Q&A answer (e.g., section/page reference)

**Implementation Steps:**
1. Modify Day 6's Q&A function to return chunk metadata alongside the answer
2. Format citation display (e.g., "Source: Section 3.2" or page number)
3. Update UI to show citation next to each answer
4. Test citation accuracy against actual source chunks

**Files/Folders:**
- `/app/qa` (extend existing logic)
- `/app/ui` (citation display components)

**Tools/APIs:**
- No new tools — extends Day 6 pipeline

**Testing:**
- For each test question, manually verify the cited section matches where the answer info actually appears in the PDF

**Debugging Tips:**
- If citation metadata is missing/inaccurate, revisit Day 3's chunk metadata tagging
- Keep citation format simple and consistent (e.g., always "Section X" or "Page X")

**Checklist:**
- [ ] Citations displayed for every answer
- [ ] Citation accuracy manually verified on sample papers
- [ ] UI clearly separates answer text from citation reference

**Expected Screenshots:**
- UI showing a Q&A exchange with citation clearly displayed

**Handoff Notes:**
- Core RAG + citation pipeline is now feature-complete. Days 8–10 shift focus to integration, testing, and deployment — no new core features should be added (scope lock).

---

## Day 8 — End-to-End Integration & UI Assembly

**Objective:** Connect upload → summary → Q&A → citations into one seamless flow.

**Learning Goals:**
- Learn to integrate independently built modules into a single application flow
- Practice basic UI assembly for functional (not polished) interfaces

**Features:**
- Full user flow in one interface: upload → summary display → chat Q&A with citations

**Implementation Steps:**
1. Assemble all previous modules into a single application flow
2. Build minimal UI screens: upload screen, summary + chat screen
3. Wire up state so uploaded paper persists through the summary and Q&A steps
4. Do a full manual walkthrough as a real user would

**Files/Folders:**
- `/app/ui` (main assembly)
- `/app/main` (entry point wiring modules together)

**Tools/APIs:**
- No new tools — integration only

**Testing:**
- Full walkthrough with 3+ different sample papers, start to finish
- Confirm no broken links between modules (e.g., chunking data reaching embeddings correctly)

**Debugging Tips:**
- Integration bugs are often data-format mismatches between modules — check inputs/outputs at each handoff point
- Keep a simple log/print statement at each stage to trace data flow

**Checklist:**
- [ ] Full flow works without manual intervention between steps
- [ ] UI, while basic, is functional and non-confusing
- [ ] No crashes across 3+ full walkthroughs

**Expected Screenshots:**
- Full-flow screenshots: upload screen → summary view → chat with citations

**Handoff Notes:**
- Flag any remaining bugs clearly for Day 9's dedicated bug-fixing and testing session.

---

## Day 9 — Testing, Bug Fixing & Edge Cases

**Objective:** Stabilize the application ahead of deployment.

**Learning Goals:**
- Practice systematic testing and edge-case thinking
- Learn to prioritize bug fixes under time constraints

**Features:**
- No new features — stabilization only

**Implementation Steps:**
1. Re-test full flow with edge cases: very short papers, long papers, unusual formatting
2. Fix any crashes or broken outputs found
3. Add basic error handling/messages for failure cases (e.g., "Couldn't extract text from this PDF")
4. Re-verify citation accuracy after any pipeline fixes

**Files/Folders:**
- Touches all `/app` modules as needed for fixes

**Tools/APIs:**
- No new tools

**Testing:**
- Structured test pass covering: normal case, edge case (very short/long paper), failure case (bad file)
- Re-run all previous day checklists to confirm nothing broke

**Debugging Tips:**
- Fix only what threatens the Day-10 demo flow — resist adding new features or polish (scope lock still active)
- Keep a running list of "known issues, not fixing" if time-constrained

**Checklist:**
- [ ] Full flow re-tested and stable across edge cases
- [ ] Error handling in place for common failure modes
- [ ] Citation accuracy re-confirmed after fixes

**Expected Screenshots:**
- Screenshot of an error case handled gracefully (e.g., friendly error message)

**Handoff Notes:**
- Application should be feature-complete and stable entering Day 10. Day 10 is deployment + demo prep only.

---

## Day 10 — Deployment & Demo Preparation

**Objective:** Deploy the application and prepare a clean, reliable live demo.

**Learning Goals:**
- Learn basic deployment steps for a simple web app
- Practice presenting a technical project clearly and concisely

**Features:**
- No new features — deployment and demo polish only

**Implementation Steps:**
1. Deploy the application to a simple hosting environment
2. Test the live deployed link end-to-end (not just local)
3. Prepare a short demo script (upload paper → show summary → ask 2–3 questions → show citations)
4. Do a final full run-through as if presenting live

**Files/Folders:**
- Deployment config files as needed (no new app logic)

**Tools/APIs:**
- Simple hosting platform (free-tier compatible)

**Testing:**
- Full demo run-through on the live deployed link, at least twice
- Confirm no differences in behavior between local and deployed versions

**Debugging Tips:**
- If deployment behaves differently than local, check environment/dependency differences first
- Keep the demo script short and focused — stick to the core flow, don't improvise new features live

**Checklist:**
- [ ] Application successfully deployed and publicly accessible
- [ ] Live link tested end-to-end without errors
- [ ] Demo script prepared and rehearsed
- [ ] Success criteria from PRD (Section 8) confirmed as met

**Expected Screenshots:**
- Live deployed app URL with full flow screenshot (upload → summary → cited Q&A)

**Handoff Notes:**
- Capstone complete. Document any "Future Scope" ideas encountered during the build for potential post-Day-10 continuation.

---

## ✅ Review Checklist (Full Blueprint)

- [ ] Each day's objective builds directly on the previous day's output
- [ ] No day introduces scope beyond the approved v1 feature list
- [ ] Core RAG pipeline (chunk → embed → retrieve → generate → cite) is complete by Day 7
- [ ] Days 8–10 contain no new features, only integration/testing/deployment
- [ ] Day 10 success criteria matches PRD Section 8 exactly
