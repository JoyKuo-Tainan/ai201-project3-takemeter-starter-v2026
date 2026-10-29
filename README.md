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

**Moment 3 (unit 6): adjudicating the staff disagreements and finding the
pattern**

- *What I asked for:* I gave Claude my 3 disagreements from
  `agreement_results.json` and asked it to write up each call from
  `data/staff_taxonomy.md`. On #1 I had already decided staff was right
  (the fact carries the argument, whatever its strength). On #2 I disagreed
  with staff, because I thought "the shoe rules changed" carried the
  argument. On #3 I asked what it thought.
- *What came back:* It wrote up #1 and #2 from my reasoning. For #2 it found
  the structural argument (fact, then "so", then conclusion, the same shape as
  the staff's own `analysis` example) and named the strongest point against
  me. On #3 it argued that staff was right: "before 90 minutes" is part of the
  claim, not evidence for it. It also pointed out the pattern across all three:
  every disagreement was on `analysis` vs `hot_take`, and my errors went both
  ways, because I was asking "is this convincing?" or "is there a number?"
  instead of "does the fact carry the argument?"
- *What I changed:* On #3 I dropped my `analysis` call and accepted that I'd
  applied Rule 1 loosely (pile a). I used the pattern as evidence in my
  Diagnoses. Because none of my disagreements involved `reaction`, labelling
  couldn't explain `reaction` failing completely, which pointed the
  diagnosis at training rather than relabelling.

**Moment 4 (unit 6): the improvement run**

- *What I asked for:* help picking the improvement and running it.
- *What came back:* Claude pointed out that 104 posts at batch size 16 for 3
  epochs is only about 21 training updates, suggested raising `EPOCHS` to 10
  as the one change, and ran section 6 with `RUN_LABEL = "after"`. Running the
  notebook also re-ran section 5, which overwrote my unit 5 `results.json` and
  `test_split.csv`, so it restored both from git.
- *What I changed:* I kept it to one change, even though the diagnosis also
  pointed at collecting more `grounded` posts. I reported that the change
  didn't flip any verdict instead of only highlighting seeds 42 and 7.

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
| 1. Overall accuracy | ≥ 0.70 on each seed | 0.478 | 0.478 | 0.478 | MISSED |
| 2. `grounded` and `hot_take` F1 | each ≥ 0.65 on each seed | 0.000 / 0.647 | 0.000 / 0.647 | 0.000 / 0.647 | MISSED |
| 3. `reaction` F1 | ≥ 0.50 on each seed | 0.000 | 0.000 | 0.000 | MISSED |
| 4. Label balance | every label ≥ 20% (40+ posts), none > 50% | 37 / 71 / 42 | 37 / 71 / 42 | 37 / 71 / 42 | MISSED |
| 5. Labelling consistency | ≥ 24/30 (80%) on the staff set | 27/30 | 27/30 | 27/30 | MET |

Criterion 2 is shown as `grounded` / `hot_take`. Criterion 4 is
`grounded` / `hot_take` / `reaction` counts out of 150. Criteria 4 and 5 come
from `labels.csv` and `agreement_results.json`, so they don't change with the
seed.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | grounded | hot_take | reaction |
|---|---|---|---|
| **grounded** | 0 | 6 | 0 |
| **hot_take** | 0 | 11 | 0 |
| **reaction** | 0 | 6 | 0 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

My biggest off-diagonal number is a tie of 6 and 6: all 6 real `grounded` posts
and all 6 real `reaction` posts were called `hot_take`, and nothing went the
other way, so the model learned neither boundary with `hot_take` and just put
every post in the majority label.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.70 on each seed | MISSED | 0.478 on all three seeds, which is 11/23, exactly the `hot_take` share of the test split |
| 2 | `grounded` and `hot_take` F1 | each ≥ 0.65 on each seed | MISSED | `hot_take` 0.647 just misses, and `grounded` is 0.000 on every seed |
| 3 | `reaction` F1 | ≥ 0.50 on each seed | MISSED | 0.000 on every seed: the model never predicted `reaction` |
| 4 | Label balance | every label ≥ 20% (40+ posts), none > 50% | MISSED | `hot_take` 47% passes the cap and `grounded` is 25% of the posts, but only 37 posts, under the 40 floor. The criterion was written for 200 labels and I have 150 |
| 5 | Labelling consistency | ≥ 24/30 (80%) on the staff set | MET | 27/30 = 90% from `agreement_results.json` |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

Misses 1, 2 and 3 have one shared cause, and miss 4 has its own.

**Criteria 1, 2 and 3: the model can't learn from this much data.** On all
three seeds the model predicted `hot_take` for every one of the 23 test posts.
My matrix shows all 6 `grounded` and all 6 `reaction` posts going to
`hot_take`, and nothing going the other way. Every accuracy and F1 in the run
log follows from that: 0.478 accuracy is just the `hot_take` share, and the
other two labels score 0 because they are never predicted. Three things tell
me this is the model and the amount of data, not my labelling:

- **The agreement report rules out `reaction`.** All 3 of my disagreements
  with staff were on `analysis` vs `hot_take`, and none involved `reaction`. I
  applied the `reaction` boundary the same way staff did every time, yet
  `reaction` failed exactly as badly as `grounded`. Inconsistent labels can't
  explain a boundary I labelled consistently.
- **The labels can be learned from the text.** The zero-shot baseline, given
  only my definitions, scored F1 0.600 on `grounded` and 0.615 on `reaction`
  on the same test posts. So the information is there in the posts. My
  trained model just didn't pick it up.
- **The model barely trained.** 104 training posts at batch size 16 is 7
  steps per epoch, so 3 epochs is only about 21 updates. The model's average
  confidence was 0.45–0.48 when right and 0.42–0.46 when wrong, barely above
  the 0.33 of a uniform guess over three labels. The seeds also gave different
  test splits but identical results, so the output doesn't depend on which
  posts it sees. It learned the majority label and nothing else.

My labelling isn't completely in the clear on `grounded`. Two of my three
disagreements were pile (a) on the boundary that matches my `grounded` vs
`hot_take`, so some of my `grounded` labels are probably noisy. But noise at
the level of 27/30 would blur that boundary, not produce a model that never
predicts `grounded` and never predicts `reaction`. So the main cause is the
amount of data and training, and inconsistent labelling at most makes
`grounded` a bit worse.

**Criterion 4: too few `grounded` examples, a data collection problem.** I
collected 150 posts instead of 200, and `grounded` came out at 37, three
under the floor. I know this from the counts in `labels.csv`. It is also part
of why criteria 1–3 failed: 25 `grounded` training posts is the smallest
training set of the three labels, and with 6 test posts per label one post
moves recall by about 17 points.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 27 / 30 = 90%

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
>Three of us ran the same session on the same day, different shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes do nothing, but whatever they do was smaller than the noise between three people.
> *Staff said / I said:*
>| `analysis` | `hot_take` |
> *My call, and why:*
> Staff is right. Rule 1 asks whether the fact carries the argument, not
> whether the fact is strong enough to prove it. Here it does: "within 10
> seconds of each other over 5 miles" is the whole reason for the conclusion,
> and without it there is no argument left. Three runners on one day is a weak
> test, and I let that push me to `hot_take`. But a small sample is still a
> checkable, load-bearing fact, and the post even scales its claim to fit the
> evidence ("I'm not saying shoes do nothing"). Rule 1 doesn't ask me to grade
> the evidence, so this is `analysis`.
>
> *Which pile:*
> (a) the rule covered it and I applied it loosely
**2.**
> *The post:*
>Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem.
> *Staff said / I said:*
>| `hot_take` | `analysis` |
> *My call, and why:*
> I think I'm right. Staff read the 3% as decoration and "so obviously" as the
> real reason. But Rule 1 asks whether the fact carries the argument, and here
> the post's own structure says it does: fact first ("times are down about 3%
> since the shoe rules changed"), then "so", then the conclusion. That is the
> same shape as the staff's own `analysis` example ("Times are down 3% since
> 2017, and the shoe rule changed in 2017. That's the most likely single
> cause."). The checkable part is the timing (times fell after the rule
> changed), and take that away and the claim about shoes has nothing behind
> it. The staff's `hot_take` example works the other way round: it states the
> conclusion first ("Super shoes have ruined racing") and adds the number
> afterwards as a flourish. The strongest point for staff is that "the whole
> story" and "the sport has a problem" claim more than one 3% drop can show.
> But that makes the argument overconfident, not unsupported, and Rule 1 asks
> whether the fact is load-bearing, not whether the conclusion is
> proportionate.
>
> *Which pile:*
> (c) the rule is ambiguous here and my reading is defensible. Rule 1 doesn't
> say whether an overstated conclusion turns a load-bearing fact into
> decoration.

**3.**
> *The post:*
>If you need a gel before 90 minutes you have a fuelling problem, not an energy problem.
> *Staff said / I said:*
>| `hot_take` | `analysis` |
> *My call, and why:*
> Staff is right. I took "before 90 minutes" as the checkable fact, but it is
> part of the claim, not evidence for it. It's the writer's own cut-off, and
> the post never says why the line sits at 90 minutes or why needing a gel
> earlier means a fuelling problem rather than an energy one. The `analysis`
> definition asks for a stat, date, time, quote or named comparison that does
> work in the argument, and 90 minutes is a time, but it isn't an observation
> anyone could look up the way "times are down 3% since 2017" is. With no
> support behind it, this matches the `hot_take` definition almost word for
> word: "a confident claim with no support offered. It might be true. It isn't
> argued."
>
> *Which pile:*
> (a) the rule covered it and I applied it loosely. A number was present, so
> the post felt checkable, but Rule 1 asks whether a fact carries the
> argument, and this number is the conclusion itself.

**What the pattern in my disagreements tells me:**

All three disagreements sit on the same boundary, `analysis` vs `hot_take`,
and I never disagreed with staff about `reaction`. In each one I judged the
evidence by something Rule 1 doesn't ask about. In #1 I asked whether the test
was strong enough to convince me, and called a load-bearing fact a `hot_take`
because three runners felt like too few. In #3 the post had a number in it, so
I called it `analysis` even though the number was part of the conclusion. #2
I still think is a defensible `analysis`, but it went the same way: a number
pulled me toward `analysis`. So I'm not consistently too strict or too loose.
My mistakes go both ways because I'm answering the wrong question ("is this
convincing?" or "is there a number?") instead of "does the fact carry the
argument?"

This matters for my own data because `analysis` vs `hot_take` is the same
boundary as my `grounded` vs `hot_take`, which I already named as my hardest.
Two of my three disagreements were pile (a), so some of my own `grounded`
and `hot_take` labels probably have the same inconsistency, especially among
the 131 pre-labelled rows, where I was checking Claude's call rather than
making my own. But 27/30 (90%) is high, and my errors are confined to one
boundary. Labelling noise of that size would blur the `grounded`/`hot_take`
line. It can't explain a model that predicts `hot_take` for every post,
including all 6 `reaction` posts, a boundary I applied the same way staff did
every time.



---

## The Improvement

**What I changed:** One training setting: `EPOCHS` from 3 to 10. Everything
else is the same: the same 150 labels, learning rate 2e-5, batch size 16, max
length 128, and the same three seeds (42, 7, 2024) on the same Apple Silicon
GPU (MPS).

**Which diagnosis pointed at it:** The diagnosis for criteria 1–3. With 104
training posts at batch size 16, 3 epochs is only about 21 updates. The model
predicted `hot_take` for every post with confidence around 0.45, barely above
a uniform guess. My agreement report ruled out labelling as the main cause:
`reaction` failed completely even though I never disagreed with staff on it.
So the change to make was training, not relabelling. 10 epochs is about 70
updates.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 on each seed | 0.652 | 0.696 | 0.478 | MISSED |
| 2. `grounded` and `hot_take` F1 | each ≥ 0.65 on each seed | 0.600 / 0.741 | 0.600 / 0.786 | 0.250 / 0.645 | MISSED |
| 3. `reaction` F1 | ≥ 0.50 on each seed | 0.444 | 0.500 | 0.000 | MISSED |
| 4. Label balance | every label ≥ 20% (40+ posts), none > 50% | 37 / 71 / 42 | 37 / 71 / 42 | 37 / 71 / 42 | MISSED |
| 5. Labelling consistency | ≥ 24/30 (80%) on the staff set | 27/30 | 27/30 | 27/30 | MET |

Results are in `results_three_seeds_after.json`. Criteria 4 and 5 don't
depend on training, so they are unchanged.

**Did it help, and how do I know:**

It helped, but it didn't flip a single verdict. Every criterion that was
MISSED before is still MISSED.

| Measure | Before (3 epochs), every seed | After (10 epochs): 42 / 7 / 2024 |
|---|---|---|
| Overall accuracy | 0.478 | 0.652 / 0.696 / 0.478 |
| Macro F1 | 0.216 | 0.595 / 0.629 / 0.298 |
| F1 — `grounded` | 0.000 | 0.600 / 0.600 / 0.250 |
| F1 — `hot_take` | 0.647 | 0.741 / 0.786 / 0.645 |
| F1 — `reaction` | 0.000 | 0.444 / 0.500 / 0.000 |

On seeds 42 and 7 the model now actually uses all three labels. Seed 7 came
within one post of the accuracy target (0.696 vs. 0.70), and its `reaction`
F1 hit 0.500 exactly. The model's confidence now means something too: on seed
42 its most confident third of predictions was 86% right and its least
confident third 29%. Before, every prediction sat around 0.45. That confirms
the diagnosis: the model was under-trained, and more updates let it learn
something.

Seed 2024 barely moved. Accuracy is still 0.478 and the model called 20 of
the 23 test posts `hot_take`, so `reaction` is still at 0. Accuracy spread
0.217 across the three seeds, which means the score depends heavily on which
posts land in the split. With 6 `grounded` and 6 `reaction` posts in each
test set, one post moves either label's F1 a lot. More epochs fixed the
training problem. What's left is the too-few-examples problem from criterion
4.

The leftover errors still point one way. On seed 42, 3 `grounded` posts were
called `hot_take` and none went the other way, and 3 `reaction` posts were
called `hot_take`. `hot_take` is still where the model sends anything it's
unsure of, which is what I'd expect when it's 47% of the training data.



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

Criteria 1–4 are still missed after the 10-epoch run. I stopped after one
change because the improvement step allows one, and I wanted the before/after
comparison to show what that change did on its own.

**Criterion 4 (label balance), the one I'd fix first.** `grounded` is 37
posts, 3 under the floor, and I have 150 labels instead of the 200 the
criterion was written for. I'd collect about 50 more posts, aiming for
`grounded` first, and take them from several different days on Arctic Shift.
All my scraped rows come from one day, so many are replies inside the same
few threads. This is a collection fix, not a training fix, so the one-change
rule kept it out of this round.

**Criterion 1 (accuracy ≥ 0.70) and criterion 3 (`reaction` F1 ≥ 0.50).**
Seed 7 got to 0.696 and 0.500, so these are close on a friendly split. Seed
2024 is still at 0.478 and 0.000, and that seed is what fails them. Accuracy
spread 0.217 across seeds, and with 6 `reaction` posts per test split one post
moves `reaction` F1 by a lot. More posts (the criterion 4 fix) is what would
make these hold on every seed. A different training setting wouldn't fix a
test split of 23 posts.

**Criterion 2 (`grounded` and `hot_take` F1 ≥ 0.65).** `grounded` reached
0.600 on two seeds and 0.250 on the third, and its errors all go to
`hot_take`. Besides more `grounded` examples, I'd re-check the
`grounded`/`hot_take` boundary in my 131 pre-labelled rows against one
question: does the detail carry the argument? My agreement report showed I
applied the matching staff rule loosely in two of three disagreements (pile
a), once by judging how strong the evidence was and once by treating any
number as a fact. That relabelling would be a second change, so I didn't do it
this round.

**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

I meant `grounded` vs `hot_take` to be about whether a checkable detail
carries the argument, and `reaction` to be about a specific event that just
happened to the poster. What the model learned is closer to "call it
`hot_take` unless you're sure". Even after 10 epochs, on seed 42 half the real
`grounded` posts (3 of 6) and half the real `reaction` posts (3 of 6) went to
`hot_take`, while not one real `hot_take` was called `grounded`.



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
