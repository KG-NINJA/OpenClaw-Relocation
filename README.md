# Moltbot on Cloudflare Workers (Sandbox) 🚀

![moltworker architecture](./assets/logo.png)

Run [Moltbot](https://molt.bot/) personal AI assistant in a [Cloudflare Sandbox](https://developers.cloudflare.com/sandbox/) container. This project allows you to migrate from a traditional VPS hosting (like Sakura VPS) to a fully managed, serverless, and cost-effective Cloudflare environment.

## 🌟 Migration Success Story: From VPS to Serverless
This deployment has been successfully used to migrate a production Moltbot instance from a Sakura VPS.
* **Architecture Change**: Traditional Linux VPS → Cloudflare Workers + Sandbox Containers.
* **OAuth Persistence**: Migrated existing OpenAI Codex (GPT-5.2) OAuth sessions without needing to re-authenticate.
* **Auto-Scaling/Backups**: Automatic 5-minute R2 backups and container-based lifecycle management.

---

## 🏗️ Architecture

The worker acts as a secure reverse proxy with WebSocket support, orchestrating a Cloudflare Sandbox container that runs the Moltbot Gateway.

1. **Cloudflare Worker**: Handles authentication (Cloudflare Access), routing, and WebSocket interception.
2. **Sandbox Container**: Runs the Node.js 22 gateway process.
3. **R2 Storage**: Periodically syncs configuration and skills for persistence.
4. **Browser Rendering**: Enables web-crawling and automation skills directly from the container.

---

## 🛠️ Quick Start

### 1. Requirements
* [Workers Paid plan](https://dash.cloudflare.com/workers-and-pages/plans) ($5/month)
* [Anthropic API key](https://console.anthropic.com/) or OpenAI tokens.
* Cloudflare R2 bucket named `moltbot-data`.

### 2. Initial Setup
```bash
# Clone and install
git clone https://github.com/[your-repo]/molt-sandbox
cd molt-sandbox/moltworker
npm install

# Set required secrets
npx wrangler secret put MOLTBOT_GATEWAY_TOKEN # Access token (e.g. sakuraebi-gateway-2026)
npx wrangler secret put SLACK_BOT_TOKEN
npx wrangler secret put SLACK_APP_TOKEN

# Deploy
npm run deploy
```

### 3. Accessing the UI
Visit your worker URL with your token:
`https://your-worker.workers.dev/?token=YOUR_TOKEN`

---

## 💾 Migration Guide (Restoring Existing Sessions)
If you are moving from a VPS, follow these steps to preserve your OpenAI OAuth:

1. Copy your `auth-profiles.json` from the VPS (~/.clawdbot/agents/main/agent/).
2. Place it in `overlay/clawd/agents/main/agent/` in this repository.
3. Deploy. The Docker image will include these tokens, restoring your session immediately.

---

## 📊 Monitoring & Debugging
Useful for troubleshooting container startup (Exit Code 1, etc.):
* **Logs**: `https://your-worker.workers.dev/debug/startup-logs` (Plain text trace)
* **Processes**: `https://your-worker.workers.dev/debug/processes?logs=true`
* **Force Start**: `https://your-worker.workers.dev/debug/force-start`

---

## 📄 License
MIT

## 🔗 Links
* [Technical Blog / Showcase](https://[your-pages-url].github.io/molt-sandbox/)
* [Moltbot Official](https://molt.bot/)
