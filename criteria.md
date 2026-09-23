# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
The `city_guides` documents are modular town summaries, but specific facts depend on distinct section phrasing. Targeting 4 of 5 allows for one borderline retrieval where the question phrasing does not directly match the terminology used in the guide.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
Because this system acts as a factual guide, unsourced statements cannot be trusted. Since our pipeline attaches document metadata to every chunk in ChromaDB, the model has access to the filename on every retrieval, making 5 of 5 an achievable non-negotiable standard.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

**Why this target:**
Unrelated queries should land far beyond the cosine distance cutoff. Targeting 4 of 5 gives margin for at most one query that accidentally shares generic vocabulary (like "train", "hours", or "cost") with the town guide text.

---

## 4. Chunks retain sentence integrity without truncation

When inspecting 5 random chunks produced by the chunker, at least 4 of the 5 chunks end with terminal punctuation (. ! ?) rather than cutting off mid-word or mid-sentence.

**Why this target:**
Fixed character slicing (800 chars) cuts through words and sentences, stripping context from retrieval. A 4 of 5 target ensures our custom chunking strategy successfully respects paragraph or sentence boundaries for the vast majority of chunks.

---

## 5. Strict refusal when context is missing

When asked 5 questions that mention a valid town name from the corpus but ask about a detail not in the documents (such as airport shuttles or nightlife), the system explicitly refuses to answer rather than fabricating details in at least 4 of 5 attempts.

**Why this target:**
In RAG pipelines, queries with high keyword overlap are the most vulnerable to hallucination. A 4 of 5 target checks that prompt grounding prevents the LLM from filling in missing details with its pre-trained knowledge.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
