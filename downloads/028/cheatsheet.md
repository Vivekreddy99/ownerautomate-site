# Anatomy (Claude Code skill): one-page cheat sheet

OwnerAutomate episode 028 · checked 2026-10-12 · repo: https://github.com/wheresryan22/anatomy · site: https://skills.wheresryan.sh/anatomy

## What it is
A free, MIT-licensed agent skill by Ryan (@wheresryan22). You name an idea in one line; Claude invents a physical machine whose working *is* the idea and builds it as an interactive isometric figure (SVG, optional WebGL shaders, beta 3D mode). Runs in Claude Code, OpenCode and Codex. About 4,600 GitHub stars four days after it was created (2026-10-08); v1.0.0 released 2026-10-10.

## Install (from the README)
| Route | Command |
|---|---|
| skills CLI, Claude Code, for you | `npx skills add wheresryan22/anatomy -g -a claude-code` |
| skills CLI, OpenCode / Codex | swap `-a opencode` or `-a codex` |
| git, Claude Code, for you | `git clone https://github.com/wheresryan22/anatomy.git ~/.claude/skills/anatomy` |
| git, one project | `git clone https://github.com/wheresryan22/anatomy.git .claude/skills/anatomy` |

- The kit needs **Node 20.10+** and nothing else (no `npm install`, no API keys).
- The README says version 1.7.1 of the `skills` CLI asks for **Node 22.20+**. On Node 20, use the git route.
- The self-checks need **Chrome or Chromium** (set `CHROME_PATH` for an unusual binary).
- New skill not showing in a running Claude Code session? Run `/reload-skills`.

What we ran on camera (Ubuntu, no sudo): `node --version` (v20.20.2), then the git clone into a test folder. We opened the example pages that ship in the repo. We did **not** generate a new figure for the video.

## First prompt
```
/anatomy explain how a token bucket rate limiter works, interactive
```
In Codex type `$anatomy`. Three habits (README):
1. **Name the idea, not the drawing**, with the real numbers (sizes, counts, rates).
2. **Or name the object** (an espresso machine, a lock) and say what should move.
3. **Say where it will live**: standalone HTML page, one SVG file, or React/Next.js; light or dark.

## Cost and time
- Skill: free, MIT. You pay for your own agent plan.
- The skill sizes each job first (CHANGELOG): **Figure** about 30-90 min, **Hero** 2-4 h, **Epic** 6 h or more. Open issue #41 reports the estimate is wrong.
- Active time the maker published for his own figures: watch movement 1 h 54 min; seasons 1 h 57 min; Raptor engine 10 h 33 min (first working version 38 min); turbofan 14 h.

## Limits (README and site)
- Not a chart library: no axes, series or data plots.
- Draws machines, instruments, tools and furniture; not people or exact maps.
- Slow on purpose. Shaders need WebGL. The 3D mode is beta (a 380-part test hand ran about 20 fps).
- In Codex the sandbox may block the headless Chrome checks; the figure is still built.
- The maker: Claude is "very good at building these and not always good at seeing them". Treat the first figure as a draft.

## Anatomy vs boxes-and-arrows tools
| | Mermaid / whiteboard diagram | Anatomy |
|---|---|---|
| Time | minutes | hours of agent time |
| Shows | that the parts exist | what the parts do |
| Best for | docs, process maps, slides | one hero figure on your site |

## Verdict for a small business owner
Free, impressive, slow. Worth one hero figure on your landing page if customers struggle to picture what you sell (heat pumps, espresso machines, solar, equipment repair, a clever product). Skip it for charts, quick graphics, or if you are on a tight usage cap.
