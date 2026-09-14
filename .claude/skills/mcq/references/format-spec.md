# MCQ Bank — Exact Format Spec

Reproduce these templates literally: heading levels, bold markers, the 📘 emoji, blockquote markers,
table column shapes, and the horizontal rule between questions. `<angle brackets>` mark the only parts to
fill in.

## Per-question block

```
**Q<n>.** <question stem, key terms in **bold**> `[<Tag?>]` `[<Difficulty>]` `[Asked: <exam/source>]?`

- **A)** <option>
- **B)** <option>
- **C)** <option>
- **D)** <option>

**Answer: <Letter>) <full correct option text repeated>**

**Trace / Why:** <2-4 sentences of reasoning that justifies the correct answer from first principles,
bolding the key terms>.

```
<optional fenced code block with worked arithmetic, when the question is numeric — plain steps, no
narration, e.g.:>
/12  → block size in the 2nd octet = 256 − 240 = 16
start 172.16.0.0  →  covers second octets 16 through 16 + 16 − 1 = 31
end   172.31.255.255
```

**📘 CONCEPT — C<n> · <one-line concept title>**
> <1-4 sentences, or a small markdown table, explaining the transferable rule>
>
> **Applies when** <the stem pattern/cue that signals this concept is being tested>.
>
> **Boundary:** <the edge case, common confusion, or exception that separates this concept from a
> lookalike>.

**Wrong traces:** A = <specific reason, only for the letters that are NOT the answer> · B = <...> ·
C = <...> · D = <...>.

---
```

Notes:

- Difficulty tag is exactly one of `` `[Core]` ``, `` `[Applied]` ``, `` `[Trap]` ``, always present.
- The optional leading `` `[<Tag>]` `` is a domain/language tag when the subject has more than one
  (e.g. OOP's `` `[C++]` ``, `` `[Java]` ``, `` `[Both]` ``). Omit entirely for single-domain subjects.
- `` `[Asked: <exam/source>]` `` is optional and rare (~5-10% of questions) — only for a question whose
  pattern is a real, frequently-repeated one from that exam/source family. Examples seen in practice:
  `[Asked: GATE-style]`, `[Asked: BCS / Bank IT]`, `[Asked: BCS / Bank IT — repeatedly]`,
  `[Asked: CCNA / bank IT]`, `[Asked: ExamVeda-style]`, `[Asked: Sanfoundry]`, `[Asked: examveda]`,
  `[Asked: interviewkickstart]`, `[Asked: bank job / university MCQ bank]`. Name a source *type*/family,
  never a fabricated specific URL or article.
- **Trace / Why** bolds the key terms it relies on and stays to first-principles reasoning, not a repeat
  of the concept box.
- The fenced-code worked-calculation block is inline inside Trace/Why (no separate "Calculation:" label)
  — used only when the question is numeric.
- **Wrong traces** gives every non-answer letter its own clause naming the *specific* misconception or
  mismatch it represents (a different concept, a common swap, a too-narrow reading, an off-by-one, etc.),
  never a generic "incorrect."
- When a concept id repeats in a later question, don't restate the full block — use the terse form:
  `**📘 CONCEPT — C<n> (see Concept Index)** › <one added remark specific to this question, e.g. the
  classic exam pairing or an extra example>.`

## Bank-level header (top of the first file only)

```
# <Subject> — MCQ Question Bank
**Subject:** <broad field> · **Target:** <exam list, e.g. BCS Preliminary / Bank IT Officer / NTRCA / GATE / IBPS SO IT>
**Questions:** <total> (Q1–Q<total>[, across N files]) · **Concepts covered:** <k>/<k> · **Complete in one run**

> How to use: attempt the question first, then read the explanation. The wrong
> options matter more than the right one — that's what the examiner is testing.

<2-4 sentence paragraph: what this bank covers end-to-end, explicitly stating nothing is deferred to a
later batch — name the major sub-topics it spans>

[optional paragraph, bold-led "**Cross-reference, not a gap.**" or similar: names a sibling chapter file
that owns overlapping material, and states exactly what this bank does NOT re-cover as a result, with a
markdown link to the sibling]

**Where the marks are.** <3-6 sentence research-backed section-ordering rationale: which exam types weight
which sub-topics highest, citing source families (GATE PYQ collections, IBPS SO IT / bank IT sets,
BCS/NTRCA compilations, CCNA banks, IndiaBIX/ExamVeda-style sets), and how that maps to the section order
chosen>.

**<k> of the <total> questions carry a worked calculation**<, and <m> more are `[Applied]` reasoning
questions, if relevant>. <1-2 sentences on the verification method and why the numeric share is what it
is for this subject>.

---
```

## Bank-level footer (end of the last file only)

```
## Concept Index

The transferable content, one line per rule. Revise from this, not from the questions.

| id | Concept | Rule in one line | Drilled by |
|---|---|---|---|
| C1 | <short name> | <the transferable rule, terse> | Q1, Q5 |
| C2 | <short name> | <rule> | Q2 |
...one row per concept id, strictly in id order, "Drilled by" lists every question that cites it...

---

## Status

**The <Subject> chapter is covered in full: <k> inventory items, <k> covered, across <total> questions in
this single run.** <1-3 sentence recap of what was covered, explicitly nothing held back>

[optional cross-reference recap: what a sibling chapter covers instead, and that together they cover the
whole syllabus]

Available as reformatting of the same material:

- `/mcq <Subject> --hard` → the trap-tier and harder applied questions only, for final revision.
- `/mcq <Subject> --practice` → all <total> questions first, explanations moved to the end, for timed
  self-testing under exam conditions.

*Research note: <2-4 sentences restating the section-ordering research rationale, AND naming one specific
real, widely-repeated error found in source material that was deliberately turned into a trap question,
citing its Q number>. Every question was written fresh and every numerical figure independently computed;
nothing is reproduced from any source.*
```

Continuation files (not first, not last) skip the bank-level header and footer entirely — they open
directly with the next `## Section N — Title` and continue question numbering, and the middle-of-run
Concept Index rows for concepts introduced in that file are still tracked (for the final file's full
index) even though they aren't printed until the last file.

## Chapter README.md (one per chapter, alongside the mcq_*.md file(s))

```
# <Subject> — MCQ Bank
**Subject:** <broad field> · **Files:** <n> · **Total questions:** <total>
**Target:** <exam list>

| File | Type | Questions | Added |
|---|---|---|---|
| [<filename>](<filename>) | <e.g. "Complete bank, part 1 (Q1–Q70)"> | <n> | <date> |
...one row per file...

<1-2 sentence note on why it's split this way (file-size decision, not a deferral) if more than one file>

**Verified on delivery.** Answer letters A/B/C/D = **<a>/<b>/<c>/<d>** (<uniform or near-uniform,
state the spread>); <note on how any drift was fixed — by reordering options, not changing answers>.
Difficulty: **Core <n> · Applied <n> · Trap <n>**. Worked calculations: **<n> of <total>**. Inventory
coverage **<k>/<k>, 0 empty**. Concept ids C1–C<k>. Structure complete on all <total>; zero "All of the
above" options.

[optional cross-reference paragraph, matching the one in the bank file's header]

## Concepts covered

<one paragraph per section, bold-led "**Section N — Title (Q<a>–Q<b>):**" followed by a middle-dot-joined
list of the concepts/topics that section drills, matching the section's actual question content>

## Available as reformatting

- `/mcq <Subject> --hard` → trap-tier and harder applied questions only.
- `/mcq <Subject> --practice` → all <total> questions first, explanations at the end.
```
