# 🤖 Telegram Live Join Bot — Render + Cron-job.org Setup

## ✅ Features
- Multiple Telegram account add/login (Telethon)
- Live join with real UDP/WebRTC connection (py-tgcalls)
- Auto-rejoin watchdog (every 4 seconds)
- Auto-mute on unmute
- Admin system
- 24/7 via Render + Cron-job.org keep-alive

---

## 🚀 Deploy on Render (Step by Step)

### Step 1 — GitHub-এ Upload করুন
1. GitHub-এ একটি নতুন **Private repo** তৈরি করুন
2. এই folder-এর সব ফাইল upload করুন:
   - `bot.py`
   - `requirements.txt`
   - `render.yaml`
   - `nixpacks.toml`
   - `Procfile`

### Step 2 — Render-এ Deploy করুন
1. [render.com](https://render.com) এ যান → **New +** → **Web Service**
2. **GitHub repo** connect করুন
3. Settings:
   - **Name:** live-joiner-bot (যেকোনো নাম)
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `python bot.py`
   - **Plan:** Free

### Step 3 — Environment Variables যোগ করুন
Render dashboard → **Environment** tab:

| Key | Value |
|-----|-------|
| `BOT_TOKEN` | BotFather থেকে পাওয়া token |
| `OWNER_ID` | আপনার Telegram User ID |
| `API_ID` | my.telegram.org থেকে |
| `API_HASH` | my.telegram.org থেকে |

### Step 4 — Deploy করুন
**Deploy** চাপুন। Deploy হলে একটি URL পাবেন:
```
https://live-joiner-bot.onrender.com
```
এই URL টি কপি করে রাখুন — cron-job.org এর জন্য লাগবে।

---

## ⏰ Cron-job.org দিয়ে 24/7 Alive রাখুন

Render-এর free plan **15 মিনিট** কোনো request না আসলে bot ঘুমিয়ে পড়ে।
Cron-job.org প্রতি 5 মিনিটে ping করে জাগিয়ে রাখবে।

### Setup:
1. [cron-job.org](https://cron-job.org) এ **Free account** খুলুন
2. **Create Cronjob** চাপুন
3. এভাবে fill করুন:

| Field | Value |
|-------|-------|
| **Title** | Live Bot Keep Alive |
| **URL** | `https://YOUR-APP-NAME.onrender.com/health` |
| **Schedule** | Every 5 minutes |
| **Request Method** | GET |

4. **Create** চাপুন → ✅ Done!

> 💡 URL-এ `YOUR-APP-NAME` টা বদলে আপনার actual Render URL দিন।
> যেমন: `https://live-joiner-bot-abc1.onrender.com/health`

---

## 🔑 Credentials কোথায় পাবেন

### BOT_TOKEN
- Telegram → @BotFather → `/newbot` → token নিন

### API_ID & API_HASH
- [my.telegram.org](https://my.telegram.org) → Login → **API development tools** → App তৈরি করুন

### OWNER_ID
- Telegram → @userinfobot → `/start` → আপনার User ID নিন

---

## 🤖 Bot ব্যবহার

```
/start         → Main menu
/add user_id   → Admin যোগ করুন (owner only)
/rm user_id    → Admin সরান (owner only)
/admins        → Admin list দেখুন
/cancel        → Cancel current action
```

### Main Menu Buttons:
- **➕ ADD ACCOUNT** — নতুন Telegram account login করুন
- **📋 ACCOUNT LIST** — সব account দেখুন
- **🟢 JOIN ON LIVE** — Live-এ join করুন

---

## 📝 গুরুত্বপূর্ণ নোট

- `accounts.json` ফাইলে account data সেভ থাকে
- Render free plan-এ **512 MB RAM** — সাধারণত 5-10 account চালানো যায়
- Free plan-এ **750 hours/month** — একটি bot সারা মাস চালাতে পারবেন
- Cron-job.org এর **free plan** = 5টি cronjob, যথেষ্ট
