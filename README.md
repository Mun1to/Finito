# Finito

Ends a coding session properly. You get a summary you can actually read, and the agent gets
two blocks to carry on exactly where this one stopped: a **handoff** to paste right after
`/compact`, and a **resume** block for the day you come back to it cold.

## Install

Inside Claude Code, two lines:

```
/plugin marketplace add Mun1to/Vibeset
/plugin install finito@vibeset
```

That is it. No cloning, no symlinks, no config files to edit. If the install summary says
`Run /reload-plugins to apply`, Claude Code reloads by itself. If it warns and leaves the plugin pending, run `/reload-plugins --force`.

<details>
<summary>Prefer to install it by hand?</summary>

Clone the repository and point your skills folder at it.

```bash
git clone https://github.com/Mun1to/Finito.git
ln -s "$(pwd)/Finito" ~/.claude/skills/finito
```

On Windows, with PowerShell:

```powershell
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\finito" -Target "C:\path\to\Finito"
```
</details>

## Use it

Type `/finito`, or just say "let's wrap up", "close the session" or "I'm about to compact". It
answers in whatever language you are writing in.

| Command | What you get |
| :--- | :--- |
| `/finito` | The full closing: summary, next steps, what got written down, both blocks. |
| `/finito short` | The minimum, plus both blocks. |
| `/finito handoff` | The handoff block on its own, for compacting right now. |
| `/finito resume` | The resume block on its own, for coming back another day. |

## What comes out

```markdown
**Klipse now confines the cursor to one monitor and survives a reboot.**

## Summary
1. Added the hotkey, Ctrl+Alt+L, which locks and unlocks the cursor.
2. Left the Sandbox profiles half finished: the profile loads, it does not launch yet.

## Next steps
1. Finish `sandbox/profiles.py`, the launcher never receives the profile path.
```

And then the part that actually matters. The handoff, for the session you are compacting
right now:

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

And the resume block, for the morning you open a brand new session, or a different tool, with
nothing left of today:

```
# RESUME, Klipse (saved 2026-08-13)

Picking this back up. Before proposing anything:

- Folder: `C:\projects\Klipse`. Read its AGENTS.md, or its README if there is none.
- Where it stopped: hotkey done and committed, Sandbox profiles half written, 3 commits
  unpushed on `main`.
- Next: finish `klipse/sandbox/profiles.py`, the launcher never receives the profile path.
- Watch out for: ClipCursor is dropped on every display change, reapply on WM_DISPLAYCHANGE.

Confirm the state with `git status` and tell me if anything does not match the above.
```

## Why it exists

Summarising a session is not the same as ending one. A summary is for you. The next session is
opened by an agent that knows nothing: not what was decided, not what was ruled out, not which
command had to be run twice before it worked.

So tomorrow starts by re-reading the same files, re-asking questions you already answered, and
paying again for a bug you already solved. Finito writes both closings and keeps them apart.
One for the person, one for the machine.

## What it will not do

- It never marks half finished work as done, and never calls something verified that was not run.
- It never commits and never pushes on its own. It reminds you and asks.
- It does not inflate a twenty minute session into a full report.

## Don't trust it, check it

Open source only helps if somebody actually reads the code, and almost nobody does. So instead
of asking you to trust this project, here is the prompt to check it: point your own AI agent at
this repository and get a security report, in your language, in a few minutes, even if you do
not know how to program.

**[Open AI-AUDIT.md](AI-AUDIT.md)** and paste it into Claude Code, Codex, Cursor, Copilot or
whatever you use. It is the same prompt in every public repository here, so you can compare.

> **ES:** No hace falta que te fíes. Abre [AI-AUDIT.md](AI-AUDIT.md), pega ese texto en tu IA y
> te dirá en tu idioma qué hace este programa de verdad: qué envía por internet, qué toca en tu
> ordenador y qué ejecuta al instalarse.

There is not much to audit here: one Markdown file, no hooks, no scripts, nothing that runs.

## License

MIT, see [LICENSE](LICENSE).
