# Coolify Manager Skill for Hermes Agent

A skill for [Hermes Agent](https://github.com/hermes-agent) that lets you manage your Coolify self-hosted PaaS via natural language.

## Features

- 📊 Check status of all applications and services
- 🔄 Restart, stop, start apps and services
- 🚀 Trigger deployments
- 🔍 Find apps by name
- 📋 List environment variables
- 🖥️ Server information

## Setup

### 1. Get your Coolify API token

1. Go to your Coolify dashboard
2. Navigate to **Security → API Tokens**
3. Create a new token with read/write permissions
4. Copy the token

### 2. Configure environment variables

Add to your `~/.env` file:

```bash
COOLIFY_URL=http://your-coolify-ip:8000
COOLIFY_TOKEN=your-api-token-here
```

Or export them:

```bash
export COOLIFY_URL="http://your-coolify-ip:8000"
export COOLIFY_TOKEN="your-api-token-here"
```

### 3. Install the skill

Copy the `coolify/` directory to your Hermes skills directory:

```bash
cp -r coolify/ ~/.hermes/skills/
```

Or use the Hermes skill manager:

```bash
hermes skill install coolify
```

## Usage Examples

Once installed, just ask Hermes:

- "¿Qué apps están caídas en Coolify?"
- "Reinicia la app UDYAT BOT"
- "¿Cuántos servidores tengo?"
- "Deploy de APIUDYAT"
- "¿Qué env vars tiene BOT CARLOS?"

## Requirements

- [Hermes Agent](https://github.com/hermes-agent)
- Coolify instance (self-hosted or cloud)
- API token from Coolify dashboard

## API Reference

Full Coolify API documentation: https://coolify.io/docs/api

## License

MIT
