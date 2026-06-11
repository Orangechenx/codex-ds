# Configuration

This fork of Codex CLI is maintained specifically to improve the DeepSeek
experience. The configuration guidance below therefore emphasizes
DeepSeek-compatible providers first.

For basic configuration instructions, see [this documentation](https://developers.openai.com/codex/config-basic).

For advanced configuration instructions, see [this documentation](https://developers.openai.com/codex/config-advanced).

For a full configuration reference, see [this documentation](https://developers.openai.com/codex/config-reference).

## DeepSeek-compatible providers

Codex CLI can be pointed at DeepSeek-compatible Responses endpoints through a
custom `model_provider` entry in `config.toml`.

Recommended settings:

- Use `wire_api = "responses"`.
- Set `requires_openai_auth = false` and provide an `env_key`, such as
  `DEEPSEEK_API_KEY`.
- If the served model is not in Codex's bundled model catalog yet, set
  `model_context_window = 1000000` so long sessions do not compact too early.
- Prefer `model_auto_compact_token_limit_scope = "body_after_prefix"` for
  cache-friendly long conversations.

Example:

```toml
model_provider = "deepseek"
model = "deepseek-v4-flash"
model_context_window = 1000000
model_auto_compact_token_limit_scope = "body_after_prefix"
model_deepseek_compatibility = "auto"

[model_providers.deepseek]
name = "DeepSeek"
base_url = "https://api.deepseek.com/v1"
env_key = "DEEPSEEK_API_KEY"
wire_api = "responses"
requires_openai_auth = false
supports_websockets = false
```

### Built-in DeepSeek optimizations

For DeepSeek-compatible providers — detected from the provider name, base URL,
or model slug containing `deepseek` — Codex applies several provider-specific
optimizations:

- It does **not** request `reasoning.encrypted_content` from the Responses API.
- It strips cached raw/encrypted reasoning content from replayed history before
  sending the next request, reducing prompt-token overhead.
- It more aggressively truncates older tool outputs from prior turns while
  leaving the most recent post-user-boundary tool outputs intact, reducing
  prompt cost without discarding the current turn's working state.
- It understands DeepSeek-style prompt-cache accounting fields such as
  `prompt_cache_hit_tokens` in addition to the OpenAI-style nested cached-token
  fields.
- Unknown DeepSeek model slugs inherit a 1,000,000-token fallback context
  window unless you explicitly override `model_context_window`.

### Explicit compatibility override

`model_deepseek_compatibility` accepts three values:

- `auto` — use Codex's built-in DeepSeek detection heuristics.
- `enabled` — force-enable DeepSeek compatibility even if the provider name,
  base URL, and model slug do not contain `deepseek`.
- `disabled` — force-disable DeepSeek compatibility, even for a provider that
  would normally be detected as DeepSeek-compatible.

Typical cases:

- Use `enabled` when you are routing through a generic OpenAI-compatible
  gateway that fronts DeepSeek models.
- Use `disabled` when you need to compare raw request behavior against a
  non-DeepSeek code path.

## Lifecycle hooks

Admins can set top-level `allow_managed_hooks_only = true` in
`requirements.toml` to ignore user, project, and session hook configs while
still allowing managed hooks from requirements and managed config layers. This
setting is only supported in `requirements.toml`; putting it in `config.toml`
does not enable managed-hooks-only mode.
