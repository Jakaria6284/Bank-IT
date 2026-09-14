---
name: mcq
description: Generate a large, exam-focused multiple-choice question bank for a given subject (e.g. Computer Networks, Subnetting, OOP), targeted at competitive exams (BCS, Bank IT Officer, NTRCA, GATE, IBPS SO IT, CCNA-style). Use when the user runs `/mcq <Subject>` to build a full question bank, `/mcq <Subject> --hard` to filter an existing bank to trap/hard-applied questions only, or `/mcq <Subject> --practice` to reformat an existing bank into an exam-simulation layout (questions first, explanations at the end).
---

# MCQ Question Bank Generator

Builds exam-prep MCQ banks as graded study material, not quiz filler: every wrong option gets a specific
explanation, every recurring idea is distilled into a cross-referenced "concept," and every numeric answer
is independently verified before it's written down. See `references/format-spec.md` for the exact
templates — follow them literally, character for character (headers, bold markers, the 📘 emoji, table
shapes). Do not improvise a different structure.

Reference examples of a fully-formed bank from a prior run of this skill live at (if present in the
current project) `mcq_data-communication-and-networking*.md`, `mcq_subnetting*.md`, and `OOP.md`, with an
index at `README.md`. Read one before a first run if you want to recalibrate against real output.

## Modes

### 1. `/mcq <Subject>` — generate a new bank

1. **Inventory first.** Enumerate every distinct testable concept in the subject before writing a single
   question — this becomes the coverage checklist. A full chapter typically has 100-170 concepts. Do not
   start drafting questions until the inventory exists.
2. **Research the mark distribution.** For the named exam types (GATE PYQ collections, IBPS SO IT / bank
   IT professional-knowledge sets, BCS/NTRCA compilations, CCNA practice banks, IndiaBIX/ExamVeda-style
   sets, standard MCQ banks), work out where marks actually concentrate for this subject, and order
   sections so the cheap/universally-asked material comes first and calculation-heavy/advanced material
   comes later. This becomes the "Where the marks are" paragraph in the header — cite source *types*, never
   fabricate specific URLs.
3. **Section the bank.** Organize into `## Section N — Title` blocks, each a coherent sub-topic. Number
   questions Q1..Qn continuously across the whole bank, including across split files.
4. **Verify before writing.** For every numeric answer, compute and cross-check it first (state the method
   in-line, e.g. "cross-checked with Python's `ipaddress` module") — never transcribe a figure from a
   source without recomputing it. Show nontrivial arithmetic as a fenced code block inside the "Trace /
   Why" explanation (see format spec).
5. **Balance answer letters.** Track A/B/C/D distribution toward uniform across the whole bank. Fix drift
   by reordering options, never by changing which fact is correct.
6. **Budget difficulty.** Every question gets exactly one difficulty tag: `` `[Core]` `` (recall/definitional),
   `` `[Applied]` `` (reasoning/multi-step), or `` `[Trap]` `` (a commonly-made real error, deliberately
   tested). Track running counts and report the final split.
7. **Never** use "All of the above" or "None of the above" as an option, on any question.
8. **Cross-reference sibling material explicitly.** If another chapter in the same project already owns
   some overlapping material (e.g. a Subnetting chapter owns IP-addressing arithmetic, so a Networking
   chapter's network-layer section should skip it), say so as a stated cross-reference with a link, not a
   silent gap. Check the project directory for sibling `mcq_*.md` files and their `README.md` before
   deciding scope.
9. **Assign stable concept ids.** The first time a concept is introduced, give it an id (C1, C2, C3, ...
   continuing across split files). Every question's concept box cites its id. When a concept repeats,
   don't re-explain it in full — reference it tersely (see format spec).
10. **Split only on size, not scope.** If the bank is large, split into multiple files at roughly every
    60-70 questions. Only the first file carries the bank-level header; only the last file carries the
    Concept Index and Status sections. Never defer coverage to "a later batch" — the whole subject is
    covered in one run, across as many files as size requires.
11. **Tag exam provenance sparingly, when true.** For a question whose pattern mirrors a real,
    frequently-repeated exam question, add an extra `` `[Asked: <exam/source>]` `` tag (e.g. `[Asked:
    GATE-style]`, `[Asked: BCS / Bank IT]`, `[Asked: CCNA / bank IT — repeatedly]`) — used for maybe 5-10%
    of questions, never fabricated for questions that aren't actually that recognizable.
12. **Write/update the chapter README.md** per `references/format-spec.md`'s README template: file table,
    "Verified on delivery" stats line, and one paragraph per section summarizing scope.

Before declaring the bank done, verify against the inventory and re-derive the stats you're about to
report (don't guess them): concept coverage (k/k, 0 empty), answer-letter tally, difficulty tally,
worked-calculation count, and zero "all of the above" options.

### 2. `/mcq <Subject> --hard` — filter to hard revision set

Take the existing bank for `<Subject>` (find its `mcq_*.md` file(s) in the project) and produce a filtered
view containing only `` `[Trap]` `` questions and the harder `` `[Applied]` `` questions, in original
question-number order, full format preserved (options, answer, trace, concept box, wrong traces). Frame it
as a final-revision pass, not a new bank — don't renumber questions or reassign concept ids.

### 3. `/mcq <Subject> --practice` — exam-simulation layout

Take the existing bank for `<Subject>` and reformat it into two parts in one document: **Part 1** is every
question with its options only — no answer, no trace, no concept box — in original order, for timed
self-testing. **Part 2** is the answer key and full explanations (answer, trace, concept box, wrong
traces) in the same order, so the reader can self-grade after finishing Part 1 cold.

## Common mistakes to avoid

- Writing questions before finishing the concept inventory — coverage gaps creep in and get discovered
  too late to fix cleanly.
- Padding "Wrong traces" with generic filler ("this is incorrect") instead of a specific reason tied to a
  real misconception.
- Repeating a full concept block verbatim every time it recurs, instead of the terse cross-reference form.
- Reporting stats (coverage, balance, difficulty counts) without actually tallying them from the finished
  file.
