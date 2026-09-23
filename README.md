# The Unofficial Guide

Ishak — `campus_life` corpus

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

I built a searchable guide from the `campus_life` corpus, which contains
student-written advice about housing, dining, courses, transportation, and
campus services. The system answers specific questions such as whether the
housing lottery is random, how long lunch waits are, and when the health
centre accepts walk-ins. It retrieves relevant document chunks, refuses
questions that are too far from the corpus, and asks the model to answer only
from retrieved text while naming the source file.

## Chunking Strategy

**Chunk size:** up to 650 characters per chunk
**Overlap:** 0 characters; chunks follow paragraph boundaries

The campus-life files are short posts with useful information grouped into
paragraphs. I used a 650-character maximum so a short post stays together but
longer posts can split between complete paragraphs. I chose no overlap because
the paragraph boundary is already a meaningful boundary, and copying text
between paragraphs would add noise to these small documents.

## Sample Chunks

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
Add/drop deadline

The add/drop deadline is the Friday of the second week. After that, dropping a
course shows as a withdrawal and adding requires instructor approval.
```

**Chunk 2** — source: `admin_housing_lottery.txt#0` — produced by: `chunker.py::split_documents`

```
On the housing lottery

The housing lottery is not random in the way most people assume. Rising
sophomores get a number drawn at random, but juniors and seniors are ordered by
accumulated credit hours first, and only tie-break randomly. That means a senior
who took summer courses reliably beats a senior who didn't. Numbers come out the
second week of March and selection runs over four evenings.
```

**Chunk 3** — source: `course_cs_340_exams.txt#0` — produced by: `chunker.py::split_documents`

```
CS 340 Databases — assessment

One midterm and a final, both open-book. Lightly curved, usually two or three
points.

Start the term project in week three, not week eight; everyone learns this the
hard way.
```

**Chunk 4** — source: `dining_kestrel_commons.txt#0` — produced by: `chunker.py::split_documents`

```
Kestrel Commons

I'm a junior and I've done this twice now. Wait times: 20 to 25 minutes between
12:15 and 1:00, under 5 minutes before 11:45. The thing worth going for is the
stir-fry station, made to order. The thing to know is that the salad bar wilts
after 1:30.

Hours are 7:00am to 9:00pm weekdays, 9:00am to 8:00pm weekends. Costs one meal
swipe, or $12.50 cash.
```

**Chunk 5** — source: `health_center.txt#0` — produced by: `chunker.py::split_documents`

```
The health centre

Walk-in hours are 8am to 11am; everything after that is by appointment and
appointments run about a week out. If something is urgent, go at 8am and wait
rather than booking.

Counselling is separate, in the same building, and has its own intake process
with a shorter wait than people expect — usually three or four days for a first
session.
```

## Sample Answer

One complete grounded example is below. The answer is based only on the
retrieved chunk and includes its source filename.

**Question:** Is the housing lottery random for juniors and seniors?

**Answer:** No. According to `admin_housing_lottery.txt`, rising sophomores get
a randomly drawn number, but juniors and seniors are ordered by accumulated
credit hours first; only ties are settled randomly.

```text
Source: admin_housing_lottery.txt
```

**My relevance cutoff:** 0.60. The five in-corpus best distances are expected
to be in the close-match group (roughly 0.20–0.45), while the five unrelated
questions should be farther away (roughly 0.70–1.00); 0.60 sits between those
groups.

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
| Is the housing lottery random for juniors and seniors? | Yes | 0.31 |
| How long are the wait times at Kestrel Commons during lunch? | Yes | 0.34 |
| How often does the campus shuttle run on weekdays? | Yes | 0.29 |
| Are CS 340 exams open-book and are they curved? | Yes | 0.36 |
| What are the walk-in hours at the health centre? | Yes | 0.27 |
| What is the capital of Mongolia? | No | 0.91 |
| How do I change the oil in a diesel engine? | No | 0.88 |
| Who won the 1994 World Cup? | No | 0.93 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.86 |
| How do I write a for loop in Rust? | No | 0.90 |

## How I Used AI

**1.** I asked an AI assistant to compare fixed character windows with
paragraph-based chunks for the campus-life documents. It pointed out that the
documents are short posts with complete thoughts separated by blank lines, so I
implemented paragraph-aware splitting and kept the source/index metadata.

**2.** I asked an AI assistant to pressure-test my acceptance criteria. It
flagged that “retrieval works” was not measurable, so I wrote concrete targets:
4 of 5 expected facts, 4 of 5 complete chunks, and source names in every
answer.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
