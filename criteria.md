# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy

Overall accuracy on the held-out test split is **at least 0.70 on each of the
three seeds**.

**Why this target:** With three labels a random guesser gets about 0.33, and the
70/15/15 split of ~200 posts leaves only about 30 test posts, so each post is
worth ~3 points of accuracy. 0.70 means getting roughly 21 of 30 right on every
seed, which is clearly useful for sorting r/ArtificialInteligence takes but
leaves room for the fuzzy `grounded`/`hot_take` posts I expect it to miss.

---

## 2. `grounded` and `hot_take` each perform well

`grounded` and `hot_take` each have an F1 of **at least 0.65 on each of the
three seeds**.

**Why this target:** These are the two labels I expect to have the most
examples, since most posts in my scope are opinions about AI, so their scores
should be the most stable. They are also my hardest boundary — the "could a
reader check the detail?" rule — so a model that can't separate them at 0.65
hasn't learned the main thing the taxonomy is for.

---

## 3. `reaction` is allowed to be worse, but not useless

`reaction` has an F1 of **at least 0.50 on each of the three seeds**.

**Why this target:** I expect `reaction` to be my smallest label, because
r/ArtificialInteligence is mostly arguments and posts like "my art mentor just
got fired" are less common. With maybe 6–8 `reaction` posts in a test split, one
mistake moves F1 a lot, so I set it 0.15 looser than the other two. Still, 0.50
is well above what you'd get by ignoring the label, which would be 0.

---

## 4. Label balance

In my 200 labels, **every label has at least 20% of the posts (40 or more), and
no label has more than 50%**.

**Why this target:** Below 40 posts, a label gets about 6 in the test split and
its F1 becomes mostly noise between seeds, which would make criterion 3
impossible to judge. Capping the biggest label at 50% keeps criterion 1
meaningful: if `hot_take` were 70% of my data, a model that always said
`hot_take` would pass 0.70 accuracy without learning anything.

---

## 5. Labelling consistency

On the 30-post staff agreement set, I match the staff labels on **at least 24 of
30 posts (80%)**.

**Why this target:** The staff taxonomy (`analysis` / `hot_take` / `reaction`)
is almost the same as mine and has the same hard boundary — whether a fact
"does work" in the argument — so if I can't apply it 80% of the time, my own
200 labels probably aren't consistent either. It also sits above my 0.70
accuracy target on purpose: the model can't be expected to agree with my labels
more often than a careful human agrees with someone else's.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
