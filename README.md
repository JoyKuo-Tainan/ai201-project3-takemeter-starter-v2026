# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

TakeMeter reads posts and comments from r/ArtificialInteligence and sorts each
one by how its claim about AI is made. A `grounded` post backs its claim with
something a reader could check: a number, a first-hand test, a named source, or
a concrete detail from the poster's own work. A `hot_take` asserts a confident
claim with no support, and a `reaction` is a personal emotional response to
something that just happened to the poster. The classifier judges the form of
the argument, not whether the claim is true.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

**Scope:** I only label posts from r/ArtificialInteligence that make a claim
about AI. Pure help requests ("which tool should I use?"), bare news links with
no commentary, and image/meme posts with only a title are left out when I
collect, because none of them is a take.

### `grounded`

**Definition:** The post makes a claim and supports it with something specific
and checkable: a number, a first-hand test, a named source, or a concrete detail
from the poster's own work.

**Example 1:** *"How I use AI to choose better hotels for less $$$"*
> I pulled the rates for 43 hotels in Rome, Paris and Barcelona for the same
> Friday in November and the free cancellation version of the same room cost on
> average 11% more than the non refundable one

**Example 2:** *"Zalando, H&M and About You took 3 different bets on AI fashion imagery"*
> I work on ecommerce content at a fashion retailer, and most of the AI imagery
> debate I read assumes the job at risk is the campaign photographer... But for
> us the campaign is a small part of the budget, most of the money goes on
> product photos, and every item on the site needs new ones each season.

### `hot_take`

**Definition:** The post makes a confident claim about AI and offers no support.
It might be true, but it's asserted, not argued.

**Example 1:** *"About AI/human misalignment"*
> Misalignment won't look like Terminator. It'll look more like an optimization
> run that treats human survival as distraction to their goal completion.

**Example 2:** *"On Excel there is still all the world finance"*
> The whole world is worried about the consequences of AI but if it were to
> collapse Excel collapses the entire world economy in a few minutes

### `reaction`

**Definition:** The post is a personal emotional response to something that just
happened to the poster or near them. It's mostly feeling, not argument.

**Example 1:** *"Hopeless in Art"*
> I just heard that my art mentor, who had been working at this company for 8
> years, got fired. They're only keeping one artist for testing who can use AI,
> and its cheaper that way. I think Art is screwed by AI, I regretted choosing
> this path.

**Example 2:** *"AI gone wrong"*
> Now, college freshman at private college in the Northeast. His first paper was
> graded as 0. His professor told him it was totally AI generated and he used
> "big words" he probably couldn't define.

### The hardest boundary

**Which two labels:** `grounded` vs `hot_take`

The post that showed me this boundary is *"AI detectors simply don't work"*:
> I put a test agaisnt multiple software programs that detect AAI-generatedcontent
> and text... Some of my data showed the detectors flagging my own essays as AI
> but not the actual AI one

The poster ran a test, which sounds `grounded`. But they never say how many
essays, which detectors, or what the results were, and the title claims far more
than the test shows.

**The decision rule I used every time:** Could a reader check the specific
detail, meaning find it, count it, or repeat it? If yes, and removing that
detail would leave the claim unsupported, the post is `grounded`. If there is
no such detail, it's a `hot_take`. Length and tone don't count: a long,
thoughtful-sounding essay with no checkable detail is still a `hot_take`. By
this rule, "AI detectors simply don't work" is a `hot_take`.

**Second rule, `reaction` vs `hot_take`:** A post is a `reaction` only if a
specific recent event set it off ("I just heard...", "his first paper was
graded..."). An emotional post about AI in general, with no triggering event,
is a `hot_take`.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** All 150 rows are public posts and comments from
r/ArtificialInteligence, kept whole (line breaks flattened to spaces).

- **32 rows** I copied by hand while reading the subreddit.
- **118 rows** came from [Arctic Shift](https://arctic-shift.photon-reddit.com),
  a public Reddit archive, because Reddit blocks automated access. I pulled the
  newest 100 posts and newest 100 comments (all from 25 October 2026). Posts
  are stored as `Title: body`, or just the title for link posts. Out of 143
  usable candidates I dropped 25: mod-removed posts and removal notices, one
  duplicate of a row I already had, image-only titles, product and community
  ads, and "thanks" replies.

Because the comments come from a single day, many of them are replies inside
the same few threads (one about Elon Musk saying AI should rule humanity, one
about LLMs vs. calculators).

**How I labelled them:**

1. **Cold (19 rows).** I labelled these first, with only my definitions and no
   tools. They are marked `cold` in the note column.
2. **Pre-labelled (131 rows).** I gave Claude my three definitions and decision
   rules and had it label the rest. Those rows are marked `pre-labelled`. I
   then read every one against the decision rules above and corrected it; rows
   I checked are marked `pre-labelled, checked`. I changed ___ of the 131
   labels.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `hot_take` | 71 | 47% |
| `reaction` | 42 | 28% |
| `grounded` | 37 | 25% |
| **Total** | 150 | 100% |

No label is above 70%, and `hot_take` is under my 50% cap from criterion 4.
`grounded` is 3 short of the 40-post floor in that criterion, so it is the label
I need more of. It is also the label that matters most: most of the subreddit is
opinion, and a post with a checkable detail is the rarer thing.

**Three hard cases**

**1.**
> *The post:* "We're only a couple years into this and AI has already solved
> over 300 math problems, many that mathematicians once thought would be
> impossible. Most famously the Navier-Stokes Equation. PHP2Shell is a
> vulnerability AI found that affects 70% of all websites in the world..."
>
> *Could have been:* `grounded` or `hot_take`
>
> *I chose `grounded`, because:* my rule asks whether a reader could check the
> detail, not whether it is true. "300 math problems", "Navier-Stokes" and
> "70% of websites" are all things a reader can look up, and the claim leans on
> them. Some of them look wrong, but a wrong checkable claim is still a checkable
> claim. This is the case where my taxonomy measures *form*, not accuracy.

**2.**
> *The post:* "I'm a developer and bought a Mini AI 'Supercomputer' with little
> idea how to use it... I told my agent that they needed some hobbies and gave
> it a website that it would update by itself. It actually came up with it's
> own hobbies... It was beautiful and strange..."
>
> *Could have been:* `grounded` or `reaction`
>
> *I chose `reaction`, because:* it is a first-hand account, which my
> `grounded` definition counts as support. But it isn't supporting a claim about
> AI; it's describing what happened to the poster and how it felt ("beautiful
> and strange", "I like learning and I guess I'm a nerd"). Something specific
> happened to the poster and the post is mostly feeling, so it is a `reaction`.

**3.**
> *The post:* "i am tired of re-teaching codebase rules every morning: For a
> long time, my biggest frustration with coding models has not been raw
> intelligence, but the fact that every single session starts from zero..."
>
> *Could have been:* `reaction` or `hot_take`
>
> *I chose `reaction`, because:* the title is pure frustration, and it comes
> from something that keeps happening to the poster in their own work, not from
> AI in general. My second rule wants a specific triggering event, and "every
> morning" is a repeated event rather than a single one, which is why I paused.
> I decided a recurring personal experience still counts as the trigger. If it
> had said "coding models are useless because they forget everything" with no
> "I", it would be a `hot_take`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 7. Trained on an Apple Silicon GPU (MPS), torch 2.14.1.

**Anything I changed from the defaults, and why:** I kept every
hyperparameter at its default. With 104 training posts I have no evidence yet
that a different learning rate or more epochs would help, and changing them
before I've seen the default run would just be guessing. The one change is the
seed: 7 instead of the starter's 42. It is one of the three seeds
(42, 7, 2024) I run next unit, so this result is one of those three, not an
extra one.

**Split sizes:** stratified 70/15/15 on 150 rows.

| Label | Train | Val | Test |
|---|---|---|---|
| `hot_take` | 49 | 11 | 11 |
| `reaction` | 30 | 6 | 6 |
| `grounded` | 25 | 6 | 6 |
| **Total** | 104 | 23 | 23 |

`grounded` and `reaction` each have only 6 posts in test, under the 8 that
gives a stable F1. One post changing sides moves either label's recall by about
17 points, so I expect those F1 scores to swing a lot between seeds.

**What the run produced:** accuracy 0.478, macro F1 0.216. The model predicted
`hot_take` for all 23 test posts, so its accuracy is exactly the `hot_take`
share of the test split (11/23), and `grounded` and `reaction` both scored F1 0.
It learned the majority label and nothing else. With 25 `grounded` examples to
train on and the default 3 epochs, that is not surprising, and it is the main
thing next unit has to explain.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1: pre-labelling 131 posts**

- *What I asked for:* I gave Claude my three label definitions and both
  decision rules (the checkable-detail rule for `grounded` vs `hot_take`, and
  the triggering-event rule for `reaction` vs `hot_take`) and asked it to label
  the 131 posts I hadn't labelled cold.
- *What came back:* a label for every row: 37 `grounded`, 71 `hot_take` and
  42 `reaction` across the full 150 once my cold rows were added.
- *What I changed:* name one or two specific labels you overturned and
  which rule decided it, and fill in the count in The Dataset ("I changed ___
  of the 131").

**Moment 2: collecting posts from Arctic Shift**

- *What I asked for:* confirm what you asked AI for here. For example,
  how to get posts out of Reddit when it blocks automated access, or a script
  to pull the newest 100 posts and 100 comments from Arctic Shift.
- *What came back:* what it suggested or wrote.
- *What I changed:* I didn't keep everything it pulled. Out of 143 usable
  candidates I dropped 25: mod-removed posts and removal notices, a duplicate
  of a row I already had, image-only titles, ads, and "thanks" replies, because
  none of them is a take under my scope rule.

**Pre-labelling disclosure:** 131 of my 150 rows (87%) were pre-labelled by
Claude from my written definitions and decision rules. They are marked
`pre-labelled` in the `note` column. The other 19 rows, marked `cold`, I
labelled myself before using any tool. I read every pre-labelled row against
my decision rules and corrected the ones I disagreed with.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.522 | 0.478 | -0.043 |
| Macro F1 | 0.508 | 0.216 | -0.292 |
| F1 — `grounded` | 0.600 | 0.000 | -0.600 |
| F1 — `hot_take` | 0.308 | 0.647 | +0.339 |
| F1 — `reaction` | 0.615 | 0.000 | -0.615 |

**What I predicted before I looked:**


I predict my trained model will beat the baseline on overall accuracy and on
`hot_take` F1, because it learned from 49 `hot_take` training posts (nearly half
my training data) and leans toward that label, while the zero-shot baseline has
never seen a post from this subreddit and spreads its guesses across all three
labels.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

My fine-tuning added nothing. The zero-shot baseline, which never saw one of my
posts, beat my trained model on accuracy (0.522 vs. 0.478) and by a wide margin
on macro F1 (0.508 vs. 0.216). My prediction was half right: the trained model
did win on `hot_take` F1 (+0.339), but it lost on accuracy. And the `hot_take`
win isn't something it learned. It predicted `hot_take` for all 23 test posts,
so it gets every real `hot_take` right for free and every other post wrong. The
baseline at least tries all three labels: it found all 6 `grounded` posts, but
it over-uses that label, calling 6 of the 11 `hot_take` posts `grounded` too.

**What my definitions were worth:** I ran the baseline a second time with bare
label names instead of my definitions (`--definitions none.txt`, saved to
`baseline_bare_names.json`).

| Measure | With definitions | Bare names | Difference |
|---|---|---|---|
| Overall accuracy | 0.522 | 0.478 | +0.043 |
| Macro F1 | 0.508 | 0.393 | +0.115 |
| F1 — `grounded` | 0.600 | 0.000 | +0.600 |
| F1 — `hot_take` | 0.308 | 0.609 | −0.301 |
| F1 — `reaction` | 0.615 | 0.571 | +0.044 |

My definitions are worth 0.115 macro F1, and almost all of it comes from
`grounded`. With just the word "grounded" the model got none of those 6 posts
right. With my definition ("supports it with something specific and
checkable") it got all 6. The cost is `hot_take`: the definition pulls posts
toward `grounded`, so its `hot_take` F1 drops by 0.301. The table above uses the
run with definitions.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
