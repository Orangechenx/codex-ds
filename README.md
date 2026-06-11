<p align="center"><strong>codex-ds</strong> is a DeepSeek-optimized fork of OpenAI Codex CLI that runs locally on your computer.
<p align="center">
  <img src="https://github.com/openai/codex/blob/main/.github/codex-cli-splash.png" alt="Codex CLI splash" width="80%" />
</p>
</br>
This fork is maintained at <a href="https://github.com/Orangechenx/codex-ds">Orangechenx/codex-ds</a> and focuses on making Codex work better with DeepSeek-compatible providers.</p>

---

## Fork focus

Compared with upstream Codex CLI, this fork focuses on DeepSeek-oriented behavior:

- better prompt-cache accounting for DeepSeek-compatible Responses endpoints
- lower token cost by stripping replayed reasoning content
- more aggressive truncation of older tool outputs in long DeepSeek sessions
- explicit `model_deepseek_compatibility` controls: `auto`, `enabled`, `disabled`
- DeepSeek-friendly fallback context-window behavior for unknown model slugs

## Quickstart for this fork

### Build and run from source

If you want to use <strong>this fork</strong>, build from source:

```bash
git clone https://github.com/Orangechenx/codex-ds.git
cd codex-ds/codex-rs

cargo build
cargo run --bin codex -- --help
```

For a DeepSeek-compatible setup example, see:

- [Configuration](./docs/config.md)
- [Sample configuration](./docs/example-config.md)
- [Installing & building](./docs/install.md)

> [!IMPORTANT]
> The official install script, npm package, Homebrew cask, and OpenAI release
> artifacts install upstream Codex CLI, not this DeepSeek-optimized fork.

## Docs

- [**Fork configuration guide**](./docs/config.md)
- [**Fork sample configuration**](./docs/example-config.md)
- [**Contributing**](./docs/contributing.md)
- [**Installing & building**](./docs/install.md)
- [**Open source fund**](./docs/open-source-fund.md)

This repository is licensed under the [Apache-2.0 License](LICENSE).
