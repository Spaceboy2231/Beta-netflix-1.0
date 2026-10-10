# Netflix Cookie Checker Telegram Bot

A Telegram bot for checking Netflix cookies and generating login links (NFToken) via PC or mobile, with an admin panel for managing the cookie pool. Cookies are publicly sourced and this project is for **educational purposes only**.

> **Disclaimer:** Everyone can modify or adjust this bot at their own risk. Use only on accounts and cookies you are authorized to test.

## Features

- Multi-threaded Netflix cookie checking via Telegram
- Supports Netscape `.txt` cookie files
- Cookie validation, account info extraction, and NFToken generation
- Organized output by plan type
- Proxy support with retry rotation
- Duplicate filtering
- Flask webhook backend for Telegram
- **Admin panel** with live dashboard (users, cookie pool stats)
- **Check All Cookies** — validate the whole pool in parallel and remove dead cookies
- Bilingual UI (English / Indonesian)
- Persistent admin sessions (survive bot restarts)

## Requirements

```bash
pip install -r requirements.txt
```

Optional for SOCKS proxies:
```bash
pip install requests[socks]
```

## Quick Start

1. Clone the repo.
2. Install dependencies.
3. Put cookie files in `cookies/`.
4. Copy `.env.example` to `.env` and fill in your settings (see below).
5. Run:

```bash
set -a && . ./.env && set +a
python telegram_bot.py
```

Or deploy on Heroku/Docker with the included `Procfile`.

## Environment Variables

Create a `.env` file (never commit it — it is git-ignored):

| Variable | Required | Description |
|---|---|---|
| `BOT_TOKEN` | Yes | Telegram bot token from [@BotFather](https://t.me/BotFather) |
| `ADMIN_PASSWORD` | No | Password to unlock the admin panel (default: `1509` — **change it**) |
| `WEBHOOK_URL` | No | Public URL for the Flask webhook backend |

## Admin Panel

Unlock the panel from the main menu → **🛡️ Admin**, then enter `ADMIN_PASSWORD`.

The dashboard shows live stats (cookie type, registered users, cookie pool breakdown) and gives access to:

- **Cookie Type** — restrict which plan the bot hands out (Basic / Premium / Both)
- **Cookie Pool** — browse the pool with per-type icons
- **List Cookies** — flat list of cookie files
- **Check All Cookies** — test every cookie in parallel and permanently delete the dead ones (with confirmation)
- **Users** — list of registered chat IDs
- **Broadcast** — send a message to all registered users
- **Logout** — leave admin mode

Admin sessions are stored in `admins.json` (git-ignored), so they survive restarts.

## Cookie Files

Cookie files are in Netscape format (`.txt`). These cookies are found from public sources. Each file should contain at minimum `NetflixId` and `SecureNetflixId` cookies.

## Configuration

Edit `config.yml` to control:
- `txt_fields` - which account fields to extract
- `nftoken` - NFToken generation mode (`false`, `"pc"`, `"mobile"`, `"both"`)
- `notifications` - webhook and Telegram notification settings
- `retries` - network retry attempts
- `performance` - timeout and fallback options

## Output Layout

```
cookies/
output/
output/run_YYYY-MM-DD_HH-MM-SS/
output/run_.../Premium/
output/run_.../Premium (Extra Member)/
output/run_.../Standard/
output/run_.../Standard With Ads/
output/run_.../Basic/
output/run_.../Mobile/
output/run_.../Free/
output/run_.../Duplicate/
output/run_.../On Hold/<Plan>/
failed/
broken/
proxy.txt
config.yml
```

## Files That Are Git-Ignored

`.env`, `admins.json`, `cookies/`, `failed/`, `broken/`, `output/`, `__pycache__/` — never commit these.

## License

MIT License. See `LICENSE`.
