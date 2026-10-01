# AIHOT cheat sheet (OwnerAutomate ep 21)

Repo: https://github.com/KKKKhazix/AIHOT · Live site: https://aihot.news · MIT license · checked 30 Sep 2026.
Everything below comes from the project's README and docs (in Chinese), translated. Where it's the project's claim, it says so.

## What it is
A self-running news site framework. It collects from your sources, has a language model filter and score every item twice, writes a headline and summary, groups reports of the same story into one "event", ranks events by how many independent sources cover them, and publishes a daily report. Swap in your sources and your selection standards and it becomes the news site for your industry.

## Install (docs/deploy.md)
Needs Docker and an OpenAI-compatible model API key (the README names DeepSeek, Qwen and Zhipu). Cloud server: at least 2 cores and 4 GB RAM recommended.

```bash
git clone https://github.com/KKKKhazix/AIHOT.git myhot   # download the code
cd myhot
node scripts/init-env.ts --llm-key <your model API key>  # writes .env, random secrets, prints admin password once
docker compose up -d --build                             # starts db, setup, api, worker, web
```

- Site: http://localhost:3000 · Admin: /admin (password = `ADMIN_PASSWORD` in `.env`)
- First items in 1-2 minutes; the first import (100+ demo items) takes about 30 minutes.
- Domain + HTTPS: set `SITE_URL`, `SITE_DOMAIN`, `PORT=127.0.0.1:3000`, `TRUST_PROXY=true`, then `docker compose --profile https up -d --build`.
- Update: `git pull && docker compose up -d --build`.

## Make it yours: edit `industry/`
| File | What it controls |
|---|---|
| `site.ts` | Site name, industry word, homepage text, about page |
| `taxonomy.ts`, `topics.json` | Categories, tags, topic pages |
| `sources.json` | Sources imported on first start (later: admin "Sources" page) |
| `prompts/` | Selection standard and writing rules. Your know-how goes here. Default writing prompts ask for Chinese headlines |
| `selection.ts` | Pass marks (defaults T1 60, T1.5 65, T2 76; two scores must sum to at least 2x the mark) |
| `features.ts` | Turn off the AI-only model leaderboard and Codex reset monitor |
| `brand/`, `pages/` | Icons, terms and privacy (templates: rewrite before launch) |

Prompt to hand Claude Code or Codex (adapted from the README):
> Read AGENTS.md and docs/customize.md and turn this site into a news site for the [your industry] industry, writing in English. The sources I care about are: ... What counts as important: ... What is noise: ... When done, run npm run typecheck, npm test and node scripts/smoke.ts, and tell me which decisions are still mine.

Calibrate: label 100-200 of your own items as pick / don't pick in `.data/gold.jsonl`, then
`node --env-file=.env scripts/eval-selection.ts --gold .data/gold.jsonl`.

## Cost guardrails (day one)
- Code: free (MIT). The AIHOT name and logo are not licensed: use your own.
- Model: the docs' demo run used about 930 model calls for 152 items on first import; daily use depends on how much your sources publish. Admin "Models" page shows calls and tokens per step.
- Paid collectors (X via SocialData, WeChat via Dajiala, Jina reader) are off unless you add a key.
- Set per-minute / per-hour / per-day caps in Settings, Budget. 0 switches a service off.
- Kill switches in `.env`: `COLLECT_ENABLED=false`, `MODEL_CALLS_ENABLED=false`.

## n8n: push items in
Set `INGEST_TOKEN` (16+ characters) in `.env`. Then one n8n HTTP Request node:
- `POST https://your-site/api/ingest/items`
- Header `Authorization: Bearer <INGEST_TOKEN>`
- Body: `{"sourceId":"my-n8n","sourceName":"My n8n flow","items":[{"title":"...","url":"...","publishedAt":"2026-10-01T08:00:00Z"}]}`
- Max 50 items per call, 10 calls per minute. New sources start hidden: set them to `editorial` in the admin to show them.

## Limits
- Two days old at recording, no releases. The author calls it a snapshot of the live site's code and says they're a designer, not a professional developer.
- Docs are in Chinese; several integrations (WeChat, Feishu, ICP filing) are China-specific and optional.
- The demo ships 18 public AI news sources; AIHOT's own source list and data are not included.
- Source full text is off by default (summary + link only). Keep it that way unless the source allows it.

## Verdict
Worth it if you sell know-how in one niche and have a technical helper for an afternoon: a daily briefing site with your name on it. If you only want to read the news, keep Google Alerts.
