---
name: coolify
description: Manage Coolify deployments via API. Use when the user needs to check, deploy, restart, or troubleshoot applications, services, or servers on their Coolify instance.
---

# Coolify Manager

Manage your Coolify self-hosted PaaS via API — apps, services, servers, deployments, and more.

## Setup

### 1. Get your API token

1. Go to your Coolify dashboard
2. Navigate to **Security → API Tokens**
3. Create a new token with read/write permissions
4. Copy the token

### 2. Configure environment variables

Add these to your `.env` file or export them:

```bash
export COOLIFY_URL="http://your-coolify-ip:8000"  # or https://your-domain.com
export COOLIFY_TOKEN="your-api-token-here"
```

### 3. Verify connection

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/version"
```

Should return the Coolify version (e.g., `4.1.2`).

## Quick Reference

### Authentication

All API calls need this header:
```bash
-H "Authorization: Bearer $COOLIFY_TOKEN"
```

### Applications

```bash
# List all apps
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications"

# Get app details
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID"

# Restart app
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID/restart"

# Stop app
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID/stop"

# Start app
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID/start"

# Deploy (trigger build)
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID/deploy"

# Delete app
curl -s -X DELETE -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID"
```

### Services

```bash
# List all services
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services"

# Get service details
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services/$UUID"

# Restart service
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services/$UUID/restart"

# Stop service
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services/$UUID/stop"

# Start service
curl -s -X POST -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services/$UUID/start"

# Delete service
curl -s -X DELETE -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services/$UUID"
```

### Servers

```bash
# List servers
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/servers"

# Get server details
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/servers/$UUID"
```

### Environment Variables

```bash
# List env vars for an app
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications/$UUID" | \
  python3 -c "import sys,json; [print(f'{e[\"key\"]}={e[\"value\"]}') for e in json.load(sys.stdin).get('environment_variables',[])]"
```

## Workflows

### Find App by Name

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications" | python3 -c "
import sys, json
data = json.load(sys.stdin)
search = 'MY_APP_NAME'  # Change this
for app in data:
    if search.upper() in app.get('name','').upper():
        print(f'UUID: {app[\"uuid\"]}')
        print(f'Name: {app[\"name\"]}')
        print(f'Status: {app.get(\"status\",\"unknown\")}')
"
```

### Check All Apps Status

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for app in sorted(data, key=lambda x: x.get('name','')):
    status = str(app.get('status', 'unknown'))
    icon = '✅' if 'running' in status else '❌' if 'stopped' in status or 'exited' in status else '⏳'
    print(f'{icon} {app.get(\"name\",\"?\")} — {status}')
"
```

### Restart App by Name

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/applications" | python3 -c "
import sys, json, subprocess, os
data = json.load(sys.stdin)
search = 'MY_APP_NAME'  # Change this
token = os.environ['COOLIFY_TOKEN']
base = os.environ['COOLIFY_URL']
for app in data:
    if search.upper() in app.get('name','').upper():
        uuid = app['uuid']
        print(f'Restarting {app[\"name\"]} ({uuid})...')
        subprocess.run(['curl', '-s', '-X', 'POST', '-H', f'Authorization: Bearer {token}', f'{base}/api/v1/applications/{uuid}/restart'])
        print('Done!')
"
```

### Check Services Status

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/services" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for svc in sorted(data, key=lambda x: x.get('name','')):
    status = str(svc.get('status', 'unknown'))
    icon = '✅' if 'running' in status else '❌' if 'stopped' in status or 'exited' in status else '⏳'
    print(f'{icon} {svc.get(\"name\",\"?\")} — {status}')
"
```

## Common Patterns

1. **App down?** → Find UUID → Check status → Restart → Verify
2. **Deploy new code?** → Find UUID → POST /deploy → Monitor
3. **Check logs?** → Use Coolify dashboard (API doesn't expose logs directly)
4. **Change env vars?** → Use Coolify dashboard or direct API PATCH
5. **Service issues?** → List services → Find UUID → Restart

## Status Values

- `running:healthy` — Service is operational
- `running:unhealthy` — Service has issues (check logs)
- `running:unknown` — Running but health unconfirmed
- `stopped` / `exited` — Service is not running
- `deploying` — Deployment in progress

## Notes

- API doesn't expose container logs directly — use dashboard for logs
- Deploy triggers a new build from configured source
- Environment variable changes require app restart to take effect
- Coolify uses Traefik with Let's Encrypt for automatic SSL

## Troubleshooting

### Connection Failed
- Verify URL is correct (include port if non-standard)
- Check token is valid (Dashboard → Security → API Tokens)
- Test manually: `curl -H "Authorization: Bearer $TOKEN" $URL/api/v1/version`

### Permission Denied
- Ensure token has read/write permissions
- Check token hasn't been revoked

### App Won't Start
- Check logs in Coolify dashboard
- Verify environment variables are set correctly
- Check if port is already in use

## API Reference

Full Coolify API docs: https://coolify.io/docs/api
