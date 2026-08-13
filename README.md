# Finito

A Claude Code skill that closes a working session properly: a summary anyone can read, the
next steps, the real state of the repository, and a handoff block you paste into the next
session so the agent picks up exactly where this one stopped.

## Why

Ending a session is not the same as summarising it. A summary is for you. The next session is
opened by an agent that knows nothing: not what was decided, not what was ruled out, not
which command had to be run twice before it worked.

So tomorrow starts by re-reading the same files, re-asking the questions you already
answered, and paying again for a bug you already solved. The context is gone, but the work it
came from is still there.

Finito writes both closings and keeps them apart. One for the person, one for the machine.

## What it produces

```markdown
**Klipse now confines the cursor to one monitor and survives a reboot.**

## Summary
1. Added the hotkey, Ctrl+Alt+L, which locks and unlocks the cursor.
2. Made the tray icon show which monitor is locked.
3. Left the Sandbox profiles half finished: the profile loads, it does not launch yet.

## Next steps
1. Finish `sandbox/profiles.py`, the launcher never receives the profile path.
2. Test the hotkey with three monitors, only two were plugged in today.

## Written down
Noted in `docs/GOTCHAS.md` that ClipCursor is released on every display change.

## Handoff
...
```

And inside that last block, the part that actually matters:

```
# HANDOFF, Klipse session, 2026-08-13

## Done, in order
1. `klipse/hotkey.py`, added Ctrl+Alt+L. Chose a global hook over a window one because the
   app lives in the tray and has no focus.

## State
- Branch `main`, `klipse/sandbox/profiles.py` uncommitted, nothing pushed.

## Gotchas
- Windows releases ClipCursor on every display change, so it has to be reapplied on
  WM_DISPLAYCHANGE. Nothing in the documentation says this.
```

## The rules it enforces

- Inventory first: re-read the session and run `git status` before writing a word.
- Half finished work is reported as half finished, never as done.
- Nothing is called verified unless it was actually run.
- What only this session knows gets written to the project docs or to the agent's memory
  before the answer is produced, because that is the part the next session cannot redo.
- The handoff block carries exact paths, the reasons behind decisions, the real repository
  state, and the gotchas that cost time today.
- It never commits and never pushes on its own. It reminds you and asks.
- Proportion: a twenty minute session gets a twenty minute closing.

## How to use it

Run `/finito`, or just say "let's wrap up", "close the session", "summarise what we did" or
"I'm about to compact".

Two arguments: `/finito short` for the minimum summary plus the handoff block, and
`/finito handoff` for the block alone when you are about to compact.

It answers in whatever language you are writing in.

## Install

```bash
git clone https://github.com/Mun1to/Finito.git
ln -s "$(pwd)/Finito/skill" ~/.claude/skills/finito
```

On Windows, with PowerShell:

```powershell
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\finito" -Target "C:\path\to\Finito\skill"
```

No hooks, no background process, no state file. It is a single Markdown file that the agent
reads when you call it.

## Don't trust it, check it

Open source only helps if somebody actually reads the code, and almost nobody does. So instead
of asking you to trust this project, here is the prompt to check it: point your own AI agent
at this repository and get a security report, in your language, in a few minutes, even if you
do not know how to program.

**[Open AI-AUDIT.md](AI-AUDIT.md)** and paste it into Claude Code, Codex, Cursor, Copilot or
whatever you use. It is the same prompt in every public repository here, so you can compare.

> **ES:** No hace falta que te fíes. Abre [AI-AUDIT.md](AI-AUDIT.md), pega ese texto en tu IA y
> te dirá en tu idioma qué hace este programa de verdad: qué envía por internet, qué toca en tu
> ordenador y qué ejecuta al instalarse.

## License

MIT, see [LICENSE](LICENSE).
