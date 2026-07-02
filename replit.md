# Digital Craftsmen — OpenClaw Gateway

Personal AI Assistant gateway server. Deployed on Railway via Docker.

## Stack

- **Runtime:** Node.js 24 (via Docker)
- **Language:** TypeScript (built with tsdown)
- **Package manager:** pnpm
- **Entry point:** `openclaw.mjs` (compiled gateway binary)
- **Default port:** 18789 (overridden by `OPENCLAW_GATEWAY_PORT` or Railway's `PORT`)

## How to run (Railway)

Railway builds using the `Dockerfile` and starts with:

```
sh -c 'OPENCLAW_GATEWAY_PORT=${PORT:-18789} node openclaw.mjs gateway'
```

Railway's injected `PORT` env var is forwarded to the gateway via `OPENCLAW_GATEWAY_PORT`.

## Required environment variables (set in Railway)

| Variable | Purpose |
|---|---|
| `GROQ_API_KEY` | Groq LLM / transcription provider API key |
| `OPENCLAW_GATEWAY_TOKEN` | Auth token clients use to connect to the gateway (generate a strong random string) |

## Optional environment variables

| Variable | Purpose |
|---|---|
| `OPENAI_API_KEY` | OpenAI provider |
| `ANTHROPIC_API_KEY` | Anthropic provider |
| `OPENROUTER_API_KEY` | OpenRouter (multi-provider) |
| `GITHUB_TOKEN` | GitHub tool access |

## Health check endpoints

- `GET /healthz` — liveness
- `GET /readyz` — readiness

## Config file

`.railway/config.json` — OpenClaw gateway config (name, bind address, auth mode, channels, skills).
The port is **not** hardcoded here; it comes from `OPENCLAW_GATEWAY_PORT` / Railway `PORT`.

## User preferences

- Deploy on Railway using Dockerfile builder.
- Gateway auth mode: `token` — always set `OPENCLAW_GATEWAY_TOKEN` in Railway environment.
- Groq is the primary LLM provider (`GROQ_API_KEY`).
