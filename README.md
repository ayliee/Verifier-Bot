# AeroX Verifier Bot

A Discord bot that verifies YouTube channel subscriptions using AI vision. The server admin provides a reference screenshot of their own subscription; the bot uses Google Gemini to visually compare every user's uploaded screenshot against that reference before assigning a role.

> Made by **Ayle** · All rights reserved © AeroX Development · [discord.gg/aerox](https://discord.gg/aerox)

---

## How It Works

1. Server admin runs `/setup` with a **reference image** (their own subscription screenshot), a role to assign, the panel channel, and support roles.
2. The bot analyzes the reference image with Gemini to extract channel identity, then posts the verification panel.
3. A user clicks **Verify** and uploads a screenshot showing their subscription.
4. Gemini receives **both** images — the reference and the user's screenshot — and performs a direct side-by-side visual comparison.
5. The bot assigns the role only if all checks pass: correct channel, subscribed state visible, and no signs of tampering.

---

## Bot Responses

| Scenario | Response |
|---|---|
| Not a YouTube screenshot | `Invalid Image.` |
| Wrong YouTube channel | `Invalid Channel, Please Subscribe To the Specified Channel` |
| Correct channel, not subscribed | `Please Subscribe To The Channel First, And Verify Again.` |
| Everything verified ✅ | `Success! You Have Been Verified.` |

---

## Pterodactyl Installation

The easiest way to host this bot. The egg handles dependency installation automatically — you just upload files and fill in four variables.

### 1. Upload the Bot Files

Upload the contents of this folder to `/home/container/` on your server — everything **except** `node_modules/` and `.env` (the panel manages env variables for you).

### 2. Import the Egg

In your Pterodactyl **Admin Panel**:
- Go to **Nests** → select or create a nest → **Import Egg**
- Upload `egg-aerox-verifier.json` from this folder

### 3. Create the Server

- Create a new server using the **AeroX Verifier Bot** egg
- Under **Startup Variables**, fill in:

| Variable | Where to get it |
|---|---|
| `DISCORD_TOKEN` | [discord.com/developers](https://discord.com/developers/applications) → your app → **Bot → Token** |
| `CLIENT_ID` | same page → **General Information → Application ID** |
| `GEMINI_API_KEY` | [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) |
| `DATABASE_URL` | your PostgreSQL provider — Supabase, Railway, Neon, etc. |

### 4. Start

Click **Start**. The egg installs npm dependencies on first run, then starts the bot. You'll see `Ready as Verifier Bot#XXXX` when it's live.

No `.env` file needed — Pterodactyl injects the variables directly.

---

## Manual / VPS Installation

### 1. Create a Discord Application

- Go to [discord.com/developers/applications](https://discord.com/developers/applications) and create a new application.
- Under **Bot**, create a bot and copy the **Token**.
- Copy the **Application ID** (this is your `CLIENT_ID`).
- Enable these **Privileged Gateway Intents**: `Server Members Intent`, `Message Content Intent`.
- Invite the bot with these permissions: `Manage Roles`, `Manage Channels`, `Manage Messages`, `Mention Everyone`, `Send Messages`, `View Channels`.

### 2. Get a Gemini API Key

- Go to [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey) and create an API key.
- This is your `GEMINI_API_KEY`.

### 3. Set Up PostgreSQL

- Create a PostgreSQL database (Supabase, Railway, Neon, etc.).
- Copy the connection string as your `DATABASE_URL`.
- The bot creates the `guild_configs` table automatically on first run.

### 4. Configure Environment Variables

Copy `.env.example` to `.env` and fill in your values:

```env
DISCORD_TOKEN=your_bot_token
CLIENT_ID=your_client_id
GEMINI_API_KEY=your_gemini_api_key
DATABASE_URL=your_postgres_connection_string
```

### 5. Start the Bot

```bash
npm install
node src/index.js
```

---

## Bot Commands

| Command | Description | Who Can Use |
|---|---|---|
| `/setup` | Upload a reference screenshot, configure the role, panel channel, and support roles | Server owner / extra owners |
| `/extraowner add` | Grant another user permission to run `/setup` | Server owner |
| `/logs` | Set a channel for bot activity logs | Server owner / extra owners |
| `/panel` | Edit the verification panel title, description, and footer | Server owner / extra owners |

---

## Requirements

- Node.js 20+
- PostgreSQL database (Supabase, Railway, Neon, or any provider)
- Google Gemini API key (free tier supported)
- A Discord bot application
