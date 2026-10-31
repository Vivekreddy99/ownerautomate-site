# Mika cheat sheet (OwnerAutomate ep 037)

Repo: https://github.com/wangzan101/Mika (created 2026-10-29, Kotlin, Apache-2.0). Latest release checked: v0.1.26, 31 Oct 2026.
Everything below is what the project's README says, checked on 2026-10-31. The app was not installed or tested for this video: it is an Android APK.

## What it is
An Android app that takes a one-sentence request and carries it out on your phone. It drives apps on a private background screen (you keep using the phone), browses the web in a built-in desktop-style browser, and runs scripts in a local Linux sandbox (arm64 phones only). Public beta.

## Requirements
| Use | Android version |
|---|---|
| Chat, web, sandbox | 11+ (sandbox: arm64 only) |
| Controlling phone apps | 12+ |
| Recommended | 14+ (background screen has its own focus) |

The main model must support tool calling.

## Install (README steps)
1. Download the latest APK from https://github.com/wangzan101/Mika/releases/latest (about 88 MB) and install it.
2. For app control: Settings, "Set up phone control" wizard. Turn on developer options, USB debugging and wireless debugging, then pair once with the 6-digit code. Do not touch any other developer-option switch; some wipe the phone (for example "boot with 16KB page size").
3. Settings, Models: add a provider and API key, or sign in with ChatGPT (Plus/Pro), xAI (SuperGrok / X Premium+) or OpenRouter. Press "Test and save".

Optional, from a computer (MCP over USB, needs adb):
```sh
adb shell sh /sdcard/Android/data/com.wangzan101.mika/files/connect-computer.sh
adb forward tcp:8765 tcp:8765
```
Then add a Streamable HTTP MCP server at http://127.0.0.1:8765/mcp with header `Authorization: Bearer YOUR_TOKEN` (the first command prints the token).

## Cost
- App: free, open source (Apache-2.0). No Mika server.
- Model: your provider's per-token price, your ChatGPT/xAI subscription allowance, your OpenRouter balance, or a local model (Ollama, LM Studio) on the same Wi-Fi.
- Settings, "Token usage" shows use per day, provider and model.

## Ask vs Autonomous (Settings, Developer options, "Autonomous mode")
| Mode | Asks first | Does without asking |
|---|---|---|
| Ask (default) | Spending: placing orders, checkout, confirming a page that shows an amount due | Sending messages, posting, submitting forms, deleting, changing settings |
| Autonomous | Nothing | Everything, including payments |

Both modes hand you logins, verification codes and payment passwords. Scheduled tasks in Ask mode skip any spending step and report it.

## Example requests (README)
- "Every morning at 8, check the weather, tell me only if it rains."
- Summarize what a chat group talked about today.
- On Amazon, find a USB-C charger under $30 with 4.5+ stars, add it to the cart, stop before checkout (README demo).
- Find the cheapest nonstop flight San Francisco to New York on Google Flights (README demo).

## Risks and limits
- Public beta, mainly tested on one Android 14 phone; updates may bring incompatible changes.
- Automating third-party apps and sites may violate their terms; accounts can be limited or banned. You are responsible for what it does.
- Screens that block screenshots (payments, passwords) need you to take over.
- Privacy (README): API keys, memory and profile encrypted with the Android keystore; chats and logs stay on the phone; update checks go only to GitHub.

## Verdict for a business owner
Worth a test on a spare Android 14 phone with a cheap model key and a spending limit: scheduled checks, chat summaries, lookups. Not yet on the phone that runs your business accounts, because Ask mode sends messages without asking and account bans are possible.
