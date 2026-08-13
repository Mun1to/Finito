---
name: finito
description: >-
  Closes a working session with a coding agent and produces the full closing package: a
  numbered summary anyone can read, the next steps, the real state of the repository, and a
  handoff block ready to paste after /compact or at the start of a fresh session. Use it when
  the user says "let's wrap up", "close the session", "summarise what we did", "give me the
  next steps", "I'm about to compact", "I'm leaving it here for today", or runs /finito.
---

# Finito, the end of a working session

A session ends twice. First for the person, who closes the laptop and needs to know in thirty
seconds what happened and what comes next. Then for the next agent, who opens a blank context
tomorrow and knows nothing about today.

Most closings only serve the first reader. Then the next session starts by re-reading files,
re-asking questions already answered, and repeating a mistake that was already paid for once.
This skill writes both closings, and keeps them apart, because they are written for different
readers and mixing them ruins both.

- **The summary and the next steps** are for the person: short, plain, no jargon, readable on
  a phone. What cannot be understood is not useful.
- **The handoff block** is for the next agent: dense, exact file paths, decisions and their
  reasons, real state. Vagueness here becomes lost context forever.

## Step 1. Take inventory before writing a single line

Go back over the conversation from the start, or from the last close if there was one, and
run `git status` plus `git log --oneline -10` in the active project. The closing has to
reflect what actually happened and the real state of the repository, not what you vaguely
remember. Memory of a long session is optimistic: it remembers intentions as if they were
outcomes.

Three hard rules for this step:

- Never report work that was not done. Anything half finished is reported as half finished.
- Never call something verified that was not run. "The build passes" requires having run it.
- If a plan was abandoned mid session, say so and say why. That is the most valuable thing
  the next session can inherit.

## Step 2. Write down what only this session knows

Some of what happened today lives nowhere but in this conversation, and disappears when the
context does. Before answering, put it where it survives:

- **Project documents.** If decisions were made, or a goal moved, or something broke in an
  interesting way, write it where the project already writes those things. Only what is
  missing: if it was already written during the session, do not write it twice.
- **Persistent memory**, if the agent has one. Durable facts only: decisions with weight,
  how the person wants to be worked with, gotchas that cost time to discover, changes of
  direction. Before creating a new entry, check whether an existing one already covers the
  topic and update that one instead of duplicating.

Do not save what the repository already records. Code, commits and documentation are already
written down: memory is for what cannot be worked out from them. If there is nothing new
worth saving, say "nothing new to save" and move on. Do not invent something to fill it.

## Step 3. Answer in this exact shape

Write in the language the user has been writing in. First line is one sentence, in bold, with
no preamble.

```markdown
**<One sentence: the essence of the session.>**

## Summary
1. <One thing done, in one simple sentence.>
2. <...in chronological order, eight points maximum. Group the small stuff.>

## Next steps
1. <A concrete action. The first one is the most important.>
2. <...five maximum.>

## Written down
<One or two lines: which documents or memory entries were touched and why, or "nothing new
to save".>

## Handoff
<code block, see below>
```

Close with one line about the repository if anything is uncommitted or unpushed, and ask.
Never commit and never push on your own at the end of a session: a closing that silently
publishes is a closing nobody can trust.

## Step 4. The handoff block

Inside a code block, so it can be copied in one click and pasted after `/compact` or into the
first message of a new session.

```
# HANDOFF, <project> session, <date>

## Context
<Who this is for, what the project is, the conventions that are not negotiable: language,
commit style, what is never done without asking.>

## Done, in order
1. <Each point with the exact paths of the files touched and the decision taken, with the
   reason in one sentence when the decision mattered.>

## State
- <Branch, what is uncommitted, what is unpushed.>
- <Anything running: dev server, background job, open terminal that matters.>

## Next
- <Open tasks, each with the path or the value needed to resume without re-reading anything.>

## Gotchas
- <What cost time to discover today: the tool that lies, the command that had to be run
  twice, the setting that is not where the documentation says. Whatever is not written here
  gets paid for again tomorrow.>
```

The density test is simple: could the next agent resume the work using this block alone?
When unsure whether a detail belongs, ask whether the work can continue without it. If it
cannot, it goes in.

The `Gotchas` section is the one everybody skips and the one that pays for itself. A bug
already solved, solved again from scratch, is the most expensive thing in a long project.

## Style rules

- Complete short sentences. No telegraphic fragments, no chains of arrows, no walls of
  abbreviations. Density comes from removing filler, never from removing words that carry
  meaning.
- Proportion. A twenty minute session gets a twenty minute closing. Do not inflate a small
  session to the size of a full working day.
- No decorative emoji, no severity theatre, no congratulating the user on the session.
- Numbers, versions and paths go in full and exact, in both halves of the closing.

## Arguments

- `/finito short`: summary and next steps at their minimum, plus the handoff block. Step 2 is
  never skipped, in any variant, because it is the only part the next session cannot redo.
- `/finito handoff`: the handoff block alone, for when the summary is not needed and the
  context is about to be compacted.
