# Claude Code, Cursor, OpenCode and Codex CLI

7API works with the coding tools developers already use. The common pattern is the same everywhere: point the tool at the 7API base URL, provide the key, and choose a current model ID.

> Replace `MODEL_ID` with an ID from the 7API dashboard. Model IDs change as providers release new versions.

## Claude Code

Claude Code speaks the Anthropic Messages API. Configure it with environment variables:

```bash
export ANTHROPIC_BASE_URL="https://.../v1"
export ANTHROPIC_AUTH_TOKEN="sk-..."   # some versions use ANTHROPIC_API_KEY
export ANTHROPIC_MODEL="MODEL_ID"
```

Start Claude Code from the same shell so it inherits the variables:

```bash
claude
```

If Claude Code ignores the custom URL:

1. Confirm you are editing the environment of the process that launches it.
2. Check whether your version expects `ANTHROPIC_AUTH_TOKEN` or `ANTHROPIC_API_KEY`.
3. Verify the base URL is the Anthropic-compatible URL from the dashboard.
4. Test the key with a direct `curl` to `/messages` before debugging the tool.

## Cursor

Cursor supports custom OpenAI-compatible providers.

1. Open **Cursor Settings → Models**.
2. Add your 7API key as the OpenAI API key.
3. Enable **Override OpenAI Base URL** and paste the OpenAI-compatible base URL from the dashboard.
4. Add the model ID you want to use.
5. Send a short test prompt and confirm the request appears in your 7API usage.

If Cursor returns `404`, the base URL is usually the culprit. Use the exact URL, including the version prefix if the dashboard shows one.

## OpenCode

OpenCode supports custom providers through its configuration file. A minimal `opencode.json` provider entry looks like this:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "7api": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "7API",
      "options": {
        "baseURL": "https://.../v1",
        "apiKey": "{env:SEVEN_API_KEY}"
      },
      "models": {
        "MODEL_ID": {
          "name": "7API Model"
        }
      }
    }
  }
}
```

Then select the provider and model in OpenCode. Keep the key in an environment variable rather than committing it to the config file.

## Codex CLI

Codex CLI supports custom model providers through its configuration file. A typical `~/.codex/config.toml` setup:

```toml
model = "MODEL_ID"
model_provider = "7api"

[model_providers.7api]
name = "7API"
base_url = "https://.../v1"
env_key = "SEVEN_API_KEY"
```

Set the key in your shell:

```bash
export SEVEN_API_KEY="sk-..."
```

If Codex CLI reports an authentication error, confirm that `env_key` matches the variable name and that the variable is exported in the same shell.

## Choosing a model for coding agents

- **Claude family** — detailed reasoning, large refactors, careful instructions.
- **GPT family** — broad tool support and predictable instruction following.
- **DeepSeek family** — cost-efficient coding and batch work.
- **GLM family** — agent workflows and bilingual projects.
- **Kimi family** — long repository context and document-heavy tasks.

Test the exact model ID on your repository before committing to it for daily work. Keep a fallback model configured in case a variant is unavailable.

## Security checklist

- [ ] API key stored in an environment variable or secrets manager.
- [ ] No key committed to `opencode.json`, `config.toml`, `.env` files or shell history.
- [ ] Base URL copied from the dashboard, not guessed.
- [ ] Model ID copied from the dashboard.
- [ ] Usage and balance visible in the 7API dashboard.
- [ ] Subkey used for shared machines or teammates.

## Next steps

- [Quickstart](./04-quickstart.md)
- [OpenAI-compatible API](./05-openai-compatible-api.md)
- [Anthropic-compatible API](./06-anthropic-compatible-api.md)
- [Troubleshooting](./13-troubleshooting.md)
- [Get an API key](https://7api.st/?utm_source=github&utm_medium=docs&utm_campaign=uncensored-ai-api)
