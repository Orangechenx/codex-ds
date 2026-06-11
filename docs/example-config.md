# Sample configuration

For a sample configuration file, see [this documentation](https://developers.openai.com/codex/config-sample).

## DeepSeek-compatible provider example

If you are wiring Codex CLI to a DeepSeek-compatible Responses endpoint, a
custom `model_provider` entry like the following is a good starting point:

```toml
model_provider = "deepseek"
model = "deepseek-v4-flash"
model_deepseek_compatibility = "auto"

[model_providers.deepseek]
name = "DeepSeek"
base_url = "https://api.deepseek.com/v1"
env_key = "DEEPSEEK_API_KEY"
wire_api = "responses"
requires_openai_auth = false
supports_websockets = false

# Recommended when the endpoint serves models that are not yet present in
# Codex's bundled model catalog.
model_context_window = 1000000
model_auto_compact_token_limit_scope = "body_after_prefix"
```

Notes:

- Codex automatically treats DeepSeek-compatible providers specially when the
  provider name, base URL, or model slug contains `deepseek`.
- `model_deepseek_compatibility` accepts `auto`, `enabled`, or `disabled`.
  Use `enabled` when a proxy/gateway hides the DeepSeek name, and `disabled`
  when you need to opt out of the compatibility logic for debugging.
- For those providers, Codex will not request
  `reasoning.encrypted_content`, and it strips cached encrypted reasoning from
  replayed history to reduce prompt-token overhead on long sessions.
- If your gateway supports the Responses WebSocket transport, you can set
  `supports_websockets = true`; otherwise leave it disabled.
