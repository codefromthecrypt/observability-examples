# OGX

This shows how to use [OGX][docs] to proxy Ollama via an OpenAI
compatible API.

## Prerequisites

Start Ollama and your OpenTelemetry Collector via this repository's [README](../README.md).

## Run OGX

```bash
docker compose up --build --force-recreate --remove-orphans
```

Clean up when finished, like this:

```bash
docker compose down
```

## Call OGX with python

Once OGX is running, use [uv][uv] to make an OpenAI request via
[chat.py](../chat.py):

```bash
uv run --exact -q --env-file env.local ../chat.py
```

Or, for the OpenAI Responses API
```bash
uv run --exact -q --env-file env.local ../chat.py --use-responses-api
```

### MCP Agent

```bash
uv run --exact -q --env-file env.local ../agent.py --use-responses-api
```

## Notes

* OGX's Responses API connects to MCP servers server-side (unlike aigw
  which proxies MCP). The agent passes MCP configuration via `HostedMCPTool`.
* Uses the `starter` distribution with its built-in `remote::ollama` provider,
  pointing to Ollama via `OLLAMA_URL` environment variable.
* Models require `provider_id/` prefix (e.g., `ollama/qwen3:0.6b`)

---
[docs]: https://ogx-ai.github.io/docs
[otel-sink]: https://ogx-ai.github.io/docs/building_applications/telemetry
[uv]: https://docs.astral.sh/uv/getting-started/installation/
