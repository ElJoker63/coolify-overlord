# 😈 Coolify Overlord

**Dominate your Coolify from Hermes Agent**

A powerful skill to manage your Coolify instance directly from your Hermes assistant. No limits, no complications.

---

## 🚀 What does it do?

Coolify Overlord lets you control your self-hosted PaaS using natural language:

- **📊 Real-time monitoring** — Status of apps, services, and servers
- **🔄 Full management** — Restart, stop, start, deploy any app
- **🔍 Smart search** — Find apps by name without knowing the UUID
- **📋 Environment variables** — Read, edit, and manage env vars
- **🖥️ Servers** — Information and status of your servers
- **🚀 Deployments** — Trigger builds and monitor progress
- **📈 Auditing** — Complete infrastructure reports

---

## 📦 Installation

### Option 1: Git clone (recommended)

```bash
cd ~/.hermes/skills/
git clone https://github.com/ElJoker63/coolify-overlord.git
```

### Option 2: Manual copy

```bash
cp -r coolify-overlord/coolify ~/.hermes/skills/
```

---

## ⚙️ Configuration

### 1. Get your Coolify API token

1. Open your Coolify dashboard
2. Go to **Security → API Tokens**
3. Create a token with read/write permissions
4. Copy the token

### 2. Set environment variables

Add to your `~/.env` file:

```bash
COOLIFY_URL=http://your-ip:8000
COOLIFY_TOKEN=your-api-token-here
```

Or export directly:

```bash
export COOLIFY_URL="http://your-ip:8000"
export COOLIFY_TOKEN="your-api-token-here"
```

### 3. Verify connection

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/version"
```

Should return the Coolify version (e.g., `4.1.2`).

---

## 💬 Usage

Once installed, just ask Hermes:

### General status
- "What apps are down in Coolify?"
- "Show me the status of all services"
- "How many servers do I have?"

### App management
- "Restart the app mi-app-web"
- "Stop the app mi-bot-api"
- "Start the app mi-service"
- "Deploy mi-project"

### Search
- "Find the app called mi-app"
- "What UUID does mi-bot have?"

### Information
- "What env vars does mi-app-web have?"
- "Give me details of mi-bot-api"

### Servers
- "What's my server's IP?"
- "Show me configured servers"

---

## 🔧 Available Commands

### Applications
| Command | Description |
|---------|-------------|
| `list apps` | List all applications |
| `app status [name]` | Show app status |
| `restart app [name]` | Restart an application |
| `stop app [name]` | Stop an application |
| `start app [name]` | Start an application |
| `deploy [name]` | Trigger build/deploy |

### Services
| Command | Description |
|---------|-------------|
| `list services` | List all services |
| `restart service [name]` | Restart a service |
| `stop service [name]` | Stop a service |

### Servers
| Command | Description |
|---------|-------------|
| `list servers` | List all servers |
| `server details [name]` | Detailed server info |

### Environment variables
| Command | Description |
|---------|-------------|
| `env vars [app]` | List app variables |

---

## 📋 Examples

### View all apps with status

```
Hermes: What apps do I have in Coolify?

Result:
✅ mi-app-web — running:healthy
✅ mi-bot-api — running:healthy
❌ mi-service-legacy — exited:unhealthy
✅ mi-database — running:unknown
...
```

### Restart an app

```
Hermes: Restart mi-service-legacy

Result:
🔍 Searching for mi-service-legacy...
UUID: xxxxxxxx...
🔄 Restarting mi-service-legacy...
✅ mi-service-legacy restarted successfully
```

---

## 🛡️ Security

- Credentials are stored in environment variables (never in code)
- The skill only performs authorized operations
- All API calls use HTTPS when available
- Tokens are transmitted securely via authorization headers

---

## 🐛 Troubleshooting

### Connection failed
- Verify the URL is correct (include port if non-standard)
- Check that the token is valid
- Test manually: `curl -H "Authorization: Bearer $TOKEN" $URL/api/v1/version`

### Permission denied
- Ensure the token has read/write permissions
- Check that the token hasn't been revoked

### App won't start
- Check logs in the Coolify dashboard
- Verify environment variables are set correctly
- Check if the port is already in use

---

## 📚 API Reference

Full Coolify API documentation: https://coolify.io/docs/api

---

## 🤝 Contributing

Contributions are welcome. Open a PR or issue on GitHub.

---

## 📄 License

MIT License

---

## 👨‍💻 Author

**ElJoker63** - [GitHub](https://github.com/ElJoker63)

---

> *"Dominate your cloud, dominate your destiny"* 😈
