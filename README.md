# Minecraft AFK Bot

A Mineflayer bot with a small status dashboard and automatic reconnection. Configure it for a Minecraft server you own or are authorized to access.

## Run locally

1. Install Node.js 18 or newer.
2. Run `npm install`.
3. Set the environment variables below, then run `npm start`.
4. Open `http://localhost:5000` for the dashboard.

Required settings:

| Variable | Example | Purpose |
| --- | --- | --- |
| `MC_SERVER_HOST` | `play.example.net` | Minecraft server hostname |
| `MC_BOT_USERNAME` | `AFKHelper` | Bot account name |

Optional settings:

| Variable | Default | Purpose |
| --- | --- | --- |
| `MC_SERVER_PORT` | `25565` | Minecraft server port |
| `MC_SERVER_VERSION` | auto-detect | Minecraft version; leave blank to auto-detect |
| `MC_BOT_AUTH` | `offline` | Mineflayer authentication mode (`offline` or `microsoft`) |
| `MC_BOT_PASSWORD` | empty | Optional account password |
| `MC_ON_JOIN_COMMANDS` | spectator and teleport commands | JSON array of commands sent after joining |
| `DASHBOARD_PASSWORD` | required | Password protecting the dashboard and its command controls |
| `DASHBOARD_USER` | `admin` | Dashboard HTTP Basic Auth username |

The default join commands are `["/gamemode spectator", "/tp @s 0 -65 0"]`. Change the Y coordinate for the server's world/version, or set `MC_ON_JOIN_COMMANDS` to `[]` to disable them. The bot account must already have the needed permissions; grant operator access from your server console. The bot cannot grant itself operator status.

On Windows PowerShell, for example:

```powershell
$env:MC_SERVER_HOST = "play.example.net"
$env:MC_BOT_USERNAME = "AFKHelper"
$env:DASHBOARD_PASSWORD = "choose-a-long-password"
npm start
```

## Deploy to Render

Use the included `render.yaml` as a Blueprint. Set `MC_SERVER_HOST`, `MC_BOT_USERNAME`, and `DASHBOARD_PASSWORD` when Render prompts for them. The Blueprint uses Render's Free plan so you can try it without paying. Render may spin down a Free web service after 15 minutes without inbound traffic; when that happens the bot disconnects, so this is not guaranteed 24/7 hosting. The web dashboard listens on Render's `PORT` environment variable.

The dashboard provides status, logs, and start/stop controls. It requires HTTP Basic Auth; Render prompts you to set a dashboard password in the Blueprint setup.

## Notes

- The bot reconnects after unexpected disconnects. It does not intentionally leave on a timer.
- Render or network maintenance can still interrupt a connection; reconnect behavior restores it when possible.
- `settings.json` contains non-secret defaults. Configure real server and account details with environment variables; do not commit account passwords.
- This project is not affiliated with Mojang or Microsoft.

MIT License. See [LICENSE](LICENSE).
