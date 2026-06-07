# Dependency Overview

- Date: 2026/06/07
- Sources: `clawspring/pyproject.toml`, `clawspring/requirements.txt`, `claude-code-source-code/package.json`

This meta-repo contains several subprojects. Only three of them declare a dependency manifest:

- `clawspring/` (the "Nano Claude Code" Python assistant) — `pyproject.toml` + `requirements.txt`.
- `claude-code-source-code/` (decompiled Claude Code research build) — `package.json` (build-tooling devDependencies only).
- The other subprojects have **no manifest**: `claw-code/` (Python, no `setup.py`/`pyproject.toml`), the standalone `memory/`, `multi_agent/`, `skill/` packages, and `original-source-code/` (raw source snapshot, no manifest). These were checked and declare no third-party dependencies of their own.

## List

### clawspring/pyproject.toml — [project.dependencies]

- `anthropic`: `>=0.40.0`
    - Official Anthropic Python SDK; used in `providers.py` to call Claude models as the primary (closed-source) backend of the assistant.
- `openai`: `>=1.30.0`
    - OpenAI Python SDK; used in `providers.py`/`tools.py` for OpenAI-compatible model endpoints (OpenAI, DeepSeek, local/open-source servers) and optional Whisper cloud STT.
- `httpx`: `>=0.27.0`
    - Async/sync HTTP client; underlying transport for provider API calls and any direct HTTP requests.
- `rich`: `>=13.0.0`
    - Terminal formatting library; renders the assistant's colored output, panels, and the demo/screenshot UI.

### clawspring/pyproject.toml — [project.optional-dependencies] voice

- `sounddevice`: (unpinned)
    - Cross-platform microphone capture; used by `voice/recorder.py` for push-to-talk voice input.

### clawspring/pyproject.toml — [project.optional-dependencies] vision

- `Pillow`: (unpinned)
    - Python imaging library; optional image handling for vision-capable model inputs.

### clawspring/requirements.txt

(Mirrors the runtime deps above plus the voice extra; STT/recording alternatives are listed as commented suggestions, not declared deps.)

- `anthropic`: `>=0.40.0`
    - Anthropic Python SDK (primary Claude backend) — see above.
- `openai`: `>=1.30.0`
    - OpenAI SDK (OpenAI-compatible endpoints) — see above.
- `httpx`: `>=0.27.0`
    - HTTP client transport — see above.
- `rich`: `>=13.0.0`
    - Terminal UI rendering — see above.
- `sounddevice`: (unpinned)
    - Microphone capture for voice input — see above.

### claude-code-source-code/package.json — devDependencies

- `esbuild`: `^0.27.4`
    - Fast JS/TS bundler; used by `scripts/build.mjs` to bundle the decompiled Claude Code source into a runnable `dist/cli.js`.
- `typescript`: `^6.0.2`
    - TypeScript compiler; used by the `check` script (`tsc --noEmit`) to type-check the decompiled source.

## Taxonomy

- LLM / Model Providers (core — the assistant's reason for existing):
   - `anthropic`
   - `openai`
- HTTP / Networking:
   - `httpx`
- Terminal UI / Output:
   - `rich`
- Voice & Vision I/O (optional extras):
   - `sounddevice`
   - `Pillow`
- Build Tooling (decompiled-source research build):
   - `esbuild`
   - `typescript`
