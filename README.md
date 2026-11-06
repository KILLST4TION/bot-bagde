# Discord stats & badge bot

## Setup

1. Python 3.10 or newer.
2. Create the app at <https://discord.com/developers/applications> -> **New Application** -> **Bot** -> **Reset Token** (copy it).
   No privileged intents are needed; leave all three switched off.
3. Invite the bot: **OAuth2 -> URL Generator**, scopes `bot` and `applications.commands`, permissions
   *View Channels, Send Messages, Embed Links, Attach Files, Read Message History*.
4. Install and run:
   ```
   pip install -r requirements.txt
   cp .env.example .env      # then paste your token into .env
   python bot.py
   ```
5. In your server, run `/monitor setup` and choose the channels to track.

## Commands

| Command | What it does |
|---|---|
| `/members` | Member count, tracked channels, tracked messages |
| `/messages [user]` | A user's message count in the monitored channels, by channel |
| `/leaderboard` | Top 10 senders |
| `/monitor setup` | Multi-select menu of channels to track (replaces the list). Needs Manage Server |
| `/monitor all` / `list` / `clear` | Track every text channel / show the list / stop tracking |
| `/monitor backfill [limit]` | Recount existing history (up to `limit` messages per channel) |
| `/badge [user]` | Image of the user's badges |
| `/nitro [user]` | Months of Nitro, current badge and next badge image, time until the upgrade |
| `/nitro_set YYYY-MM-DD` / `/nitro_forget` | Register / delete your own Nitro start date |
| `/boost [user]` | Same card for server boosting, based on this server's boost start date |

## Badge images

The bot draws a coloured circle with initials for any badge that has no icon.
For real artwork, put square PNG files (ideally 256x256, transparent) in the `badges/` folder, named as below.
Use icons you have the right to use; the bot does not download Discord's artwork.

- Profile flags: `discord_staff`, `partner`, `certified_moderator`, `hypesquad_events`, `hypesquad_bravery`,
  `hypesquad_brilliance`, `hypesquad_balance`, `bug_hunter_1`, `bug_hunter_2`, `early_supporter`,
  `verified_developer`, `active_developer`, `verified_app`
- Nitro: `nitro_base`, `nitro_bronze`, `nitro_silver`, `nitro_gold`, `nitro_platinum`, `nitro_diamond`,
  `nitro_emerald`, `nitro_ruby`, `nitro_fire`
- Boost: `boost_0`, `boost_1`, `boost_2`, `boost_3`, `boost_6`, `boost_9`, `boost_12`, `boost_15`, `boost_18`, `boost_24`

Tier thresholds live in `NITRO_TIERS` and `BOOST_TIERS` at the top of `bot.py`.

## Things to know

- **Counting starts when a channel is added.** Use `/monitor backfill` for history. Deleted messages are not subtracted,
  bot messages are ignored, and thread messages are not counted (only the selected channel itself).
- Backfill replaces the stored counts for each monitored channel; avoid running it during very busy moments.
- Counts and registered Nitro dates are stored in `botdata.sqlite3`. Tell your members what you store about them.
- The bot must be running for messages to be counted. Anything sent while it is offline is missed unless you backfill.
