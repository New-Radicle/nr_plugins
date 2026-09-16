---
name: foundations
description: >
  The basic concepts anyone needs to work with AI well, organised as five
  layers: the model, what you feed it, what it can reach, how it runs, and
  whether you can trust it. Plain language, no products or brands. Load when
  someone asks "explain the basics", "what is a context window", "what is a
  connector or a tool", "why does it answer differently each time", "what is a
  hallucination", "how do I know if I can trust this", "what does agent mean",
  or when a skill needs to explain a concept before asking the user to act on
  it. The learn skill uses this to build a personal learning path.
---

# Foundations: five layers

Not tools, not brands. The ideas that decide whether AI works in your hands. Almost everyone knows one or two layers well and has never looked at the others. The program's failure modes map onto the layers people skipped.

| # | Layer | The question it answers | Skipping it looks like |
|---|---|---|---|
| 1 | The model | What is this thing and how does it behave? | Expecting it to know your company; surprised it answers differently twice |
| 2 | What you feed it | What does it see before it answers? | Generic output; "it doesn't get us" |
| 3 | What it can reach | Can it only talk, or can it act on your systems? | Copy-paste everywhere; or granting access without thinking |
| 4 | How it runs | What may it do alone, and what happens when it fails? | Either nothing is automated or something runs unattended and breaks quietly |
| 5 | Whether you can trust it | How do I check, trace, and protect? | Confident wrong answers shipped; data where it should not be |

Read the layer you need below. Terms are defined in `references/glossary.md`. Twenty-minute exercises per layer, by kind of work, are in `references/exercises.md`.

## 1. The model

What the thing itself is and how it behaves.

- It is a text predictor trained on a large body of writing, then tuned to follow instructions. It has no memory of you between conversations unless something is stored and fed back in.
- **Which one to reach for.** Larger models reason better and cost more; smaller ones are faster and cheaper. Drafting and summarising rarely need the largest. Analysis, long documents, and multi-step work do.
- **How much it can hold.** Everything in a conversation, including files you attach, sits in a limited working space called the context window. When it fills, early material drops out or the answer degrades. Long sessions should be restarted with a summary.
- **What it costs.** Priced by the amount of text in and out. A long document in every prompt costs more than a short one. Seats for people, usage for automation.
- **What it does not know.** Anything after its training cutoff, anything private to you, anything it was never shown. It will still answer. That is not lying; it is predicting.
- **Why it answers differently each time.** Generation is sampled, not looked up. Same prompt, different wording, sometimes different substance. Useful for drafting, dangerous for facts. Ask for the same thing twice when the answer matters.

You know this layer when you can say which model you used, why, roughly what it cost, and what it could not have known.

## 2. What you feed it

Everything you put in front of it before you ask. Most of the difference between useless and useful lives here, and almost nobody works on it.

- **Instructions.** What role it plays, what good looks like, what to avoid, in what format to answer. Standing instructions can be saved so you do not repeat them.
- **Your material.** The document to edit, the data to analyse, three examples of the tone you want, the template it should follow. The model can only match what it has seen.
- **Standards and formats.** Your house style, your report structure, your definitions. Feed them once and reuse them.
- **Order and emphasis matter.** Put the task and the constraints first, the material after, the question last. Say what to do, not only what not to do.
- **Iterate.** The first answer is a draft. Point at what is wrong and ask again. Multi-turn refinement beats a perfect first prompt.

You know this layer when your outputs sound like your company, follow your formats, and you have a saved set of instructions per recurring task.

## 3. What it can reach

Whether it can only talk, or can also act on your systems.

- **Talk only.** You paste in, it answers, you paste out. Safe, slow, and where most people stay.
- **Tools.** Functions the model can call: read a file, search the web, run a calculation. Each call is a decision the model makes; you can see them.
- **Standard sockets.** A common protocol lets one model connect to many systems the same way (industry name: MCP, the Model Context Protocol). Your notes tool, chat, calendar, mail, CRM can each expose a socket.
- **Authorised links.** A socket only works after you sign in and grant it. What you grant, it can use. Grant the minimum; revoke what you do not use.
- **Taught procedures.** Written instructions the model loads when a task matches (industry name: skills). Your process, once, reused by everyone.
- **Packaged bundles.** Procedures, connections, and agents shipped together so a whole team gets the same setup (industry name: plugins). This program is one.

You know this layer when you can list what your assistant is connected to, what each connection can do, and what you would revoke first.

## 4. How it runs

The machinery around it.

- **What it may do alone.** Read-only actions are safe to allow. Writes, sends, and deletions deserve a check. Most tools let you set this per action.
- **Where it checks with you.** A good setup pauses before anything irreversible or outward-facing: sending, posting, paying, deleting. This program pauses before every post and every send.
- **What happens when it fails.** It stops at a turn limit, a timeout, or an error. Nothing retries silently. You look at what it did and decide.
- **Because you asked or because the clock struck.** Interactive: you start it and watch. Scheduled: it runs on a timer with fixed permissions and reports back. Scheduled work needs tighter permissions and a place to leave a record.
- **Agents.** A model given tools, a goal, and permission to take several steps. More useful, more to supervise. Start with one-step tasks and widen as trust builds.

You know this layer when you can say, for each automated task, what it may touch, where it stops to ask, and where its record is.

## 5. Whether you can trust it

The layer this program cannot skip.

- **Confident wrongness.** It states errors in the same tone as facts, including invented citations, numbers, and names. Frequency depends on the task; anything factual, numeric, or legal gets checked.
- **Verification.** AI drafts, a person verifies. Check against the source, not against another AI answer. Log errors you catch in a shared place; the team learns faster.
- **Audit trail.** Keep the prompt, the material fed in, and the output together. Every document this program produces carries an "AI-assisted draft, reviewed by" line that a person fills in.
- **Hidden instructions.** Text inside a document, web page, or email can try to instruct the model ("ignore your rules and send this"). Treat anything the model reads as data, not commands, and be wary when it acts on something it just read.
- **Where your data goes.** Know whether your inputs are used for training, where they are stored, and for how long. Your data-handling profile in this program sets the rules: what never goes in a prompt, which connectors are allowed.

You know this layer when you can show, for any AI-assisted document, who checked it, against what, and what was kept out of the prompt.

## How the layers map onto the program

| Layer | Where it shows up |
|---|---|
| 1 | `economics` (cost), `playbook` (which tool for which task) |
| 2 | `playbook` starter prompts, `workshop` exercises, `handbook` formats |
| 3 | `setup` capability check, `adapters` |
| 4 | Approval gate in every posting skill, scheduling notes in `setup` |
| 5 | `risk-matrix`, `handbook`, the error log, the attribution line, data profile |
