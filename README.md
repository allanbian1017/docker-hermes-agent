# 🤖 NousResearch Hermes Agent - Docker Deployment

[![Docker Compose](https://img.shields.io/badge/Docker--Compose-v3.8+-blue?logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Hermes Agent](https://img.shields.io/badge/NousResearch-Hermes--Agent-orange?logo=github)](https://github.com/NousResearch/hermes-agent)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-green.svg)](https://opensource.org/licenses/Apache-2.0)

This repository provides a streamlined configuration to run the autonomous, self-improving AI agent **NousResearch Hermes Agent** locally or on your server using Docker Compose.

---

## 🌟 Key Features of Hermes Agent

*   **Persistent Memory & Learnings:** Maintains context, long-term memory, and learned experiences across restarts.
*   **Dynamic Skill Generation:** Creates and refines reusable code skills based on task experience.
*   **Model Agnostic:** Seamlessly integrates with various LLM backends via OpenAI, Anthropic, or OpenRouter.
*   **Built-in Web Dashboard:** A user-friendly web interface to interact with, manage, and monitor the agent.
*   **Multi-Platform Integrations:** Built-in capability for connecting to Telegram, Slack, Discord, WhatsApp, and Microsoft Teams.

---

## 🛠️ Configuration Details

All configurations are defined in the [docker-compose.yml](./docker-compose.yml) file.

### Port Mappings

The container exposes the following ports to the host machine:

| Port | Service / Component | Purpose |
| :--- | :--- | :--- |
| `8642` | **Core API Server** | OpenAI-compatible API endpoint for programmatic integration. |
| `8646` | **Webhook Listener** | Microsoft Graph Webhook Listener (e.g. for incoming Teams/Outlook events). |
| `9119` | **Web Dashboard** | Web-based Graphic User Interface (GUI). |

### Environment Variables

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `API_SERVER_ENABLED` | `true` | Activates the gateway's OpenAI-compatible API server. |
| `API_SERVER_HOST` | `0.0.0.0` | Binds the API server to all network interfaces. |
| `API_SERVER_KEY` | `<paste-your-generated-key-here>` | Security API key to authenticate requests sent to the server. |
| `API_SERVER_CORS_ORIGINS` | `*` | Permits Cross-Origin Resource Sharing (CORS) from any origin. |
| `HERMES_DASHBOARD` | `1` | Enables the built-in web dashboard interface. |
| `HERMES_DASHBOARD_INSECURE` | `1` | Bypasses OAuth authentication for the dashboard (convenient for local-only use). |

---

## 🚀 Quick Start Guide

### 1. Prerequisites
Ensure you have the following installed on your machine:
*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/)

### 2. Initial Setup Wizard
Before running the services, you must configure your AI model keys (e.g., Anthropic, OpenAI, or OpenRouter). Run the Hermes Agent interactive setup wizard to generate these configs:

```bash
docker run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup
```
Follow the interactive prompts in your terminal to set your preferred models and keys. This creates the configurations inside the `~/.hermes` folder on your host machine.

### 3. Configure Your API Key
You can dynamically generate a secure random key and save it to a `.env` file using `openssl`:

```bash
echo "API_SERVER_KEY=$(openssl rand -hex 32)" > .env
```

Alternatively, you can run the agent by passing the key inline:
```bash
API_SERVER_KEY=$(openssl rand -hex 32) docker compose up -d
```

### 4. Launch the Agent
Start the Hermes Agent in detached (background) mode:

```bash
docker compose up -d
```

### 5. Access the Web Dashboard
Open your browser and navigate to:
👉 **[http://localhost:9119](http://localhost:9119)**

---

## 🔒 Security Recommendations

> [!WARNING]
> **Dashboard Access Security**
> By default, `HERMES_DASHBOARD_INSECURE=1` is set to bypass OAuth authentication gates. This is safe **only** on secure localhost or private networks. If deploying to a public server or VPS, set this to `0` or protect port `9119` with a reverse proxy (like Nginx/Caddy) with Basic Auth or VPN access.

> [!IMPORTANT]
> **Generate a Secure API Server Key**
> Do not leave `API_SERVER_KEY` blank or set to a weak value. Always use a strong, randomly generated secret (such as the output of `openssl rand -hex 32` stored in a `.env` file) to prevent unauthorized API requests to your gateway.

---

## 📂 Data Persistence

The container maps `~/.hermes` on the host to `/opt/data` in the container. This ensures that:
*   Your API configurations
*   Learned skills & code modules
*   Conversation logs and memories

are safely persisted on your host machine and will not be lost when upgrading or restarting the Docker containers.

---

## 🛠️ Basic Management Commands

*   **Stop the Agent:**
    ```bash
    docker compose down
    ```
*   **View Live Logs:**
    ```bash
    docker compose logs -f
    ```
*   **Check Service Status:**
    ```bash
    docker compose ps
    ```
*   **Update the Agent Image:**
    ```bash
    docker compose pull
    docker compose up -d
    ```
