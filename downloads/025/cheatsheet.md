# universal-modder cheat sheet (OwnerAutomate ep 25)

Repo: https://github.com/rehan-remade/universal-modder · MIT license · created 30 Sept 2026 · checked 3 Oct 2026.
Everything below comes from the project's README and repo page. The examples are the author's own sessions; OwnerAutomate did not run a mod for this video.

## What it is
Skills, a command-line tool (`um`) and a shared knowledge base that let an AI coding agent mod almost any PC game you own. The README lists Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, OpenCode, "or anything that reads AGENTS.md". Art, 3D, sound and video come from fal.

## What you need
- An AI coding agent you already use (see install table).
- Python 3.10+ and ffmpeg. `uv` is recommended.
- Blender, only for 3D-to-sprite renders.
- A fal API key, only for generated assets: `export FAL_KEY=...`
- Windows games are driven natively or from WSL.

## Install (pick your agent)
| Agent | Install |
|---|---|
| Claude Code | `/plugin marketplace add rehan-remade/universal-modder`, then `/plugin install universal-modder@universal-modder` |
| Codex | `codex plugin marketplace add rehan-remade/universal-modder`, then `codex plugin add universal-modder@universal-modder` |
| Gemini CLI | `gemini extensions install https://github.com/rehan-remade/universal-modder` |
| VS Code / Copilot | Enable `chat.plugins.enabled`, run **Chat: Install Plugin From Source**, enter the repo URL |
| Cursor | Cursor Marketplace, or clone (Cursor reads `AGENTS.md` and `.cursor/mcp.json`) |
| Skills only | `npx skills add https://github.com/rehan-remade/universal-modder` |
| Anything else | `git clone https://github.com/rehan-remade/universal-modder` and start your agent inside it |

The `um` CLI anywhere else: `uv tool install git+https://github.com/rehan-remade/universal-modder`

## The ten-step loop (skill: mod-any-game)
1. Search the knowledge base for prior notes on this game.
2. Recon: find the engine and version, then pick a route.
3. Set up a safe lab (saves backed up).
4. Read the actual code.
5. Build one working slice.
6. Generate assets.
7. Verify in the real game.
8. Record.
9. Package.
10. Write a field note for the next agent.

## Example prompts from the README
- "Mod Terraria: add a homing missile launcher and a tactical nuke that craters the world. Make the sprites with fal."
- "Make a new civilization for Age of Empires II with a unique unit rendered from 3D."
- "Put real Minecraft inside GTA V story mode. Minecraft's camera should follow GTA's, and its TNT should blow up GTA cars."
- "What engine is C:\Games\Foo, and has anyone modded it before?"

## The skills
mod-any-game (the loop, safety rules, 12 engine playbooks) · game-recon · reverse-engineering · fal-assets · asset-pipeline · game-automation · showcase-video · mashup-mods · publish-mod · share-field-notes

## Useful `um` commands
- `um scan`: find Steam/Epic/Xbox installs, engine and version, anti-cheat, loaders, save folders, ranked routes.
- `um fal price`: check what a generation costs before you run it.
- `um backup`: snapshot, diff and restore save folders.
- `um publish check`: blocks shipping game files, decompiled code and leaked keys.
- `um kb search "game name"`: prior art before you start.

## Safety rules the README says it follows
- Single-player and offline, on games you own. It refuses to inject into online games with anti-cheat, write multiplayer cheats, or bypass anti-cheat, DRM or ownership checks.
- It never ships game files or decompiled code.
- It backs up before touching saves, and kills processes by PID only.
- It asks before driving your mouse and keyboard, installing loaders into game folders, or publishing (PRs included).

## Risks to check yourself
- Days old: four commits, no release, three contributors when checked.
- It runs on your machine with your agent: use a gaming PC, not the laptop with your business logins.
- The capture and input tools are Windows PowerShell.
- Check the game publisher's mod policy before you share or sell anything.

## Costs
- universal-modder: free (MIT).
- Your agent: whatever your existing plan costs.
- fal: paid per generation; prices vary by model, check with `um fal price` or your fal account.

## Borrow the idea for your business: a field-note template
The project's knowledge base stores one note per job so the next agent "starts where it left off instead of rediscovering the same traps". The same shape works for any AI task you repeat (invoices, quotes, inbox triage). Keep a `notes/` folder and tell your agent to read it first and write one at the end:

```
# <task> - <date>
Versions that worked: <app / API / model versions>
Route, and why: <the approach that worked>
What the system really does: <surprises vs the docs>
How it was verified: <the check you ran>
Gotchas: <symptom> -> <cause> -> <fix>
```
