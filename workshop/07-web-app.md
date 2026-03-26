# 07 - Deploy the Chat Web App

## 1) Overview

The `webapp/` folder contains a standalone Flask chat interface for Biashara Bot. It connects to GitHub Models for the LLM and reads the same JSON data files as the MCP server directly — **you do not need the MCP server running for this step**. The idea here is to see your agent in action.

```
Browser → Flask (webapp/app.py) → GitHub Models API
                ↓
        mcp-server/data/*.json (menu + FAQs)
```

## 2) Set your GitHub Token

The web app uses GitHub Models as the LLM backend. You need a **GitHub personal access token (PAT)** to authenticate.

### Create a token

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens?type=beta) and click **Generate new token**.
2. Select **Fine-grained token** (recommended) or **Classic token**.
3. Give it a descriptive name, e.g. `biashara-bot-workshop`.
4. Set an expiration (7 days is fine for a workshop).
5. **Permissions required:** No additional scopes are needed — the default public access is sufficient for GitHub Models.
6. Click **Generate token** and copy the value immediately (you won't see it again).

### Export the token
 
In a new terminal, ensuring you are in the virtual environment you cerated, add your GitHub token:

```bash
export GITHUB_TOKEN="add-your-token-here"
```

> **Tip:** You can also create a `webapp/.env` file with `GITHUB_TOKEN=your-token` — the app loads `.env` files automatically.

## 3) Start the web app

```bash
python webapp/app.py
```

You should see:

```
🍽️  Biashara Bot Web App starting...
   Model: gpt-4.1-mini
   Endpoint: https://models.github.ai/inference
   Open http://127.0.0.1:5000 in your browser
```

## 4) Chat with Biashara Bot

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser. Try these questions:

```text
I'm in a rush — what's a quick and affordable breakfast option?
```

```text
Niko Kilimani. Mnatuma huku na ni free delivery?
```

```text
We have a meeting tomorrow with 20 people — can you handle that on short notice?
```

## 5) How it works

| Component | Description |
|-----------|-------------|
| `webapp/app.py` | Flask backend with the system prompt, tool definitions, and an agentic tool-calling loop |
| `webapp/templates/index.html` | Chat UI with a green/purple Savanna Bites theme |
| `mcp-server/data/*.json` | Shared data files (menu + FAQs) — same as the MCP server |
| GitHub Models API | LLM inference via the OpenAI-compatible endpoint |

The app mirrors the two MCP tools (`search_business_faqs`, `get_product_catalogue`) as OpenAI function-calling tools. When the model decides it needs data, Flask executes the tool locally and feeds the result back — up to 5 rounds per message.

## 6) Configuration

You can customise the app with environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `GITHUB_TOKEN` | *(required)* | Your GitHub personal access token |
| `MODEL` | `gpt-4.1-mini` | Model name to use |
| `ENDPOINT` | `https://models.github.ai/inference` | OpenAI-compatible API endpoint |

Continue to [08-export-code.md](08-export-code.md).