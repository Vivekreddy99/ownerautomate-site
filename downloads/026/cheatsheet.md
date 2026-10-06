# Universal Modder tutorial: every command (OwnerAutomate ep 026)

Shot 2026-10-06, 14:23–14:47 EDT (~24 min) on this Ubuntu 24.04 box as user vvrb, no sudo. Everything happened on this machine; nothing staged or re-enacted. (Written by the shoot agent; saved by the orchestrator because the agent's write was blocked.)

## Verdict
It worked end to end. Claude Code with the universal-modder plugin replaced the Freedoom pistol with a fal-generated plasma rifle and doubled its fire rate, delivered as a PWAD (`-file`) plus a standalone DEHACKED patch (`-deh`), tested in the running game, and filmed.
- Visual: plasma rifle sprite replaces the pistol; blue muzzle flash when firing (frames/modded_*.png).
- Fire rate (Claude's measurement): stock pistol every 14 tics (2.5 shots/s), mod every 7 tics (5.0 shots/s), both `-file` and `-deh`.
- Fire rate (independent check): same 3-second fire hold, stock used 8 rounds (50→42), mod 16 (50→34). Exactly 2x.
- fal spend: $0.16 (1 call to fal-ai/nano-banana-2, 2 images at $0.08, per `um fal price` and fal_manifest.jsonl; not checked against the fal billing dashboard).
- Claude cost: none per token (Claude Max). Session 6 m 48 s, Opus 5.5, high effort.

## Exact commands, in order
### 1. Game, user space (no sudo)
```bash
apt-get download freedoom dsda-doom prboom-plus
apt-get download libzip4t64 libsdl2-mixer-2.0-0 libsdl2-image-2.0-0 libmad0 libfluidsynth3 \
                 libdumb1t64 libportmidi0 libmodplug1 libopusfile0 libinstpatch-1.0-2 xdotool libxdo3
for d in *.deb; do dpkg -x "$d" ~/tools/games/root; done
Xvfb :91 -screen 0 1280x720x24 -nolisten tcp &
prboom-plus -iwad freedoom2.wad -window -geom 1280x720 -nosound -warp 1 -skill 3
ffmpeg -f x11grab -framerate 30 -video_size 1280x720 -i :91 -t 10 game-baseline.mp4
```
`~/tools/games/bin/prboom-plus` is a launcher (sets LD_LIBRARY_PATH, DOOMWADPATH, HOME, runs dsda-doom). On Ubuntu 24.04 the prboom-plus package installs dsda-doom 0.27.5, its successor: say "prboom-plus (dsda-doom)".

### 2. Install universal-modder (install.webm, typed live in bash inside ttyd, in ~/tools/games/freedoom-mod)
```bash
ls -la
uv tool install git+https://github.com/rehan-remade/universal-modder   # Installed 1 executable: um (v0.2.0 @76b9c7e)
um --help
claude plugin marketplace add rehan-remade/universal-modder           # Successfully added marketplace
claude plugin install universal-modder@universal-modder               # installed (scope: user)
claude plugin list | grep -A4 universal-modder                        # Status: enabled
um scan ~/tools/games/freedoom-mod                                    # id Tech / Doom family [70%]; route 1 = WAD/PK3 mods
um kb search doom                                                     # nothing yet ("you may be first")
```
Inside Claude Code the README form is `/plugin marketplace add …` and `/plugin install …`; the `claude plugin` CLI is equivalent.

### 3. The mod (claude-session.webm; transcript claude-session.log)
Prompt typed verbatim into the Claude Code TUI, no further human input:
> Mod Freedoom for prboom-plus: add a fal-generated plasma rifle sprite replacing the pistol and make it fire twice as fast. Use the universal-modder skills. Produce a PWAD or DEHACKED patch I can load with -file/-deh, and test it. Setup: freedoom2.wad is in this folder, the engine is `prboom-plus` on PATH (dsda-doom 0.27.5), an Xvfb display is running on DISPLAY=:91 with xdotool and ffmpeg available for testing, and FAL_KEY is set. Keep fal spend under $2 (check prices with um fal price), keep all files in this folder, and you have my OK to launch and drive the game on :91, so don't stop to ask.

DISCLOSURE (must be said in the voiceover): `claude` on screen is a shell alias:
```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude --setting-sources project,local \
  --plugin-dir ~/.claude/plugins/cache/universal-modder/universal-modder/76b9c7e77ead \
  --model opus --effort high --permission-mode acceptEdits \
  --allowedTools Bash Read Glob Grep Skill WebFetch WebSearch mcp__plugin_universal-modder_fal
```
It isolates the session (only universal-modder: 10 skills + its fal MCP server) and pre-approves Bash and file edits. A viewer typing plain `claude` gets the same plugin but approves each command unless they use accept-edits mode and allow Bash.

What Claude did (39 tool calls): loaded `mod-any-game`, ran `um kb search "doom"`, read the id Tech/Doom playbook; loaded `fal-assets`, ran `um fal price` (nano-banana-2 $0.08/image); wrote a WAD/Doom-picture reader-writer (tools/wadlib.py) and extracted stock pistol sprites; ran `um fal image` (2 images on magenta, $0.16), picked one; wrote tools/build.py (background removal, Doom aspect, Freedoom palette) producing a PWAD with PISGA0–E0, a script-drawn blue flash PISFA0 and an embedded DEHACKED lump, plus a standalone .deh; fixed a cyan-to-green palette shift by moving to Doom blue and enlarged the gun; wrote test_fire.sh (xdotool fire hold, 60 fps capture) and shot_interval.py; measured stock fire at 14 tics (last frame skipped while fire held), set firing frames 2/3/2/3/4 = 7 tics; tested -file and -deh (22 shots each at 7.0 tics), checked the engine DEHACKED log, made a side-by-side clip; wrote README.md and MODLOG.md; `um publish check build` PASS. Offered a KB field note (needs human OK for PR; not done).
Not used: `um sprite`, `um fal sprite`, the plugin's fal MCP tools (41 loaded). Do not claim them in the script.

### 4. Play the mod (game-modded.mp4)
```bash
prboom-plus -iwad freedoom2.wad -file build/plasmapistol.wad -window -geom 1280x720 -nosound -warp 1 -skill 3
prboom-plus -iwad freedoom2.wad -deh build/plasmapistol.deh     # fire rate only, stock art
```
Driven with xdotool (`w` forward, `q`/`e` turn, held `Control_L` to fire), recorded with ffmpeg x11grab. Engine log confirms WAD and DEH lump loaded.

