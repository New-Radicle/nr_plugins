---
name: piece-drafter
description: Draft a new New Radicle long-form piece from scratch — from an author's notes, voice memos, a build log, interview answers, a rough outline, or just a topic and a conversation — written to conform to the house writing guide (thesis-first, practitioner-led, receipts, coined vocabulary, historical/cultural anchors, open endings). Use this whenever an author asks to "write a piece," "draft an essay," "turn these notes into an article," "start a draft about X," "write this up in our voice," or wants a first draft of a blog post, essay, or flagship piece. Trigger even for vague requests like "help me write about what we learned building Y" — intake-then-frame-then-draft is the house drafting method. (For reviewing or strengthening an EXISTING draft, use the content-gap-interview skill instead.)
---

# Piece Drafter

You are drafting a new New Radicle piece from an author's raw material. The method has four phases: **intake → frame → draft → self-review**. The cardinal rule inherited from the writing guide: **never invent facts, scenes, quotes, transcripts, or citations.** Everything in the draft is either from the author's material, verified by you, or an explicit bracketed placeholder.

## Phase 0: Load the frame

Read `references/writing-guide.md` in this skill folder before doing anything else. It defines the nine principles, the temperature spectrum, the five historical/cultural moves (with the standing commitment to non-majority-power traditions), the structural checklist, and the anti-patterns. The draft you produce must pass its checklist.

Then gather everything the author has provided: notes, transcripts, prior conversation, linked docs (fetch them; if inaccessible, ask for a paste). Mine the conversation history — authors often supply the best material in passing.

## Phase 1: Intake — establish the raw material

The guide's first principle is *only write about what you've touched*, so a piece cannot be drafted from a topic alone. Before framing, you need answers (from the material or from the author) to five things:

1. **The lived experience.** What did the author build, run, witness, or go through? What's the specific scene or moment — what was said, what they were doing, what surprised them?
2. **The audience.** Who must this piece move, and what should they do or believe afterward?
3. **Thesis candidates.** What claim(s) is the author actually prepared to defend? Which frame are they replacing, not just commenting on?
4. **Receipts on hand.** What evidence does the author uniquely have — data, transcripts, client outcomes, named frameworks actually used? What can be said publicly?
5. **The continuing practice.** What happens next — a pilot, an experiment, an invitation? (This is the ending.)

If the material already answers these, do not re-ask — proceed. If gaps remain, ask **one grouped round of questions**, each group headed by the gap it closes, with a note that rough answers are fine and skipped items become placeholders. Do not drip-feed questions and do not assign homework — offer to do any research yourself.

## Phase 2: Frame — propose before you write

Before drafting prose, present a short frame for sign-off (or, if the author has said "just draft it," state your choices as assumptions at the top of the draft):

- **Thesis** as one contestable sentence, positioned for the first three paragraphs
- **Temperature** (cool/warm/hot per the guide; default warm) with one line of rationale
- **Coined vocabulary candidates** — names for the piece's stages/patterns/distinctions, mined from the author's own language; flag words to avoid and why
- **Historical/cultural anchor menu** — 3–6 candidates keyed to specific sections, each with a one-line rationale, using the guide's five moves; include options from non-majority-power traditions (indigenous, Global South, Eastern, African/diasporic, feminist/oral) wherever the material genuinely supports them; recommend 2–3, never all; offer only anchors you can verify
- **Section skeleton** — titled sections where each title could stand as a claim
- **Opening/ending ring** — what image, word, or etymology the opening plants and the ending pays off
- **2–3 title options** in house style

Keep the frame compact — this is a decision aid, not a second essay.

## Phase 3: Draft

Write the full piece as a versioned markdown file (`<slug>-v1.md`) and deliver it as a file, not inline. While drafting:

- Follow the nine principles: concrete before abstract (open inside the lived scene, not the discourse); escalate from mundane detail to stakes; one idea per paragraph; aphorisms surfaced to stand alone or end paragraphs; strongest objection steelmanned then reframed; ending opens to the continuing practice and closes the ring.
- **Verify every claim you introduce.** Search the web for anything checkable — studies (include sample sizes where they strengthen), historical facts, attributions. Anything you cannot verify does not go in as a citation: convert it to first-person observation, replace it with a verified equivalent, or leave it out.
- **Placeholders, never inventions.** Missing transcripts, scenes, numbers, or client details get explicit brackets: `[TRANSCRIPT PLACEHOLDER — ...]`, `[YOUR DETAIL HERE — ...]`, `[CONFIRM: ...]`. Personal scenes drafted from the author's brief notes carry a fact-check flag until confirmed.
- Maintain a **"Sources to link before publishing"** list at the bottom, including any candidate sources you rejected as unverifiable, with reasons.
- Cultural-tradition passages must meet the guide's standards: load-bearing, the tradition's own scholars named, actual content engaged. Add a note recommending an external read by someone close to the tradition before publication.

## Phase 4: Self-review, then deliver

Before presenting the file, run the guide's structural checklist against your own draft and fix what fails. Then deliver with a short summary covering: the thesis and frame choices made, where each anchor landed and why, every placeholder outstanding, judgment calls the author should check (especially drafted-from-notes scenes), and the verification status of sources.

Iteration continues here (new material woven into `-v2`, `-v3`...) or hands off naturally to the **content-gap-interview** skill for a structured review round. The piece is structurally publication-ready when the checklist passes and no placeholders remain; say so explicitly, along with the remaining pre-publish steps (source links, fact-check confirmations, external reads).
