# pi-anthropic-code-execution

[![ci](https://github.com/code-yeongyu/pi-anthropic-code-execution/actions/workflows/ci.yml/badge.svg)](https://github.com/code-yeongyu/pi-anthropic-code-execution/actions/workflows/ci.yml) [![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Anthropic native code execution extension for the [pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent).

This package is the standalone extraction of senpi's former builtin `anthropic-code-execution` extension.

## Behavior

The extension does not register a new tool. It intercepts Anthropic requests before they are sent and ensures a native `code_execution_*` tool is present for `anthropic-messages` payloads only when code execution is explicitly enabled.

| Case | Result |
|------|--------|
| `PI_ANTHROPIC_CODE_EXECUTION` is enabled, API is `anthropic-messages`, and no native `code_execution_*` tool exists | injects `{ type: "code_execution_20250825", name: "code_execution" }` |
| `PI_ANTHROPIC_CODE_EXECUTION` is enabled and a native `code_execution_*` tool already exists | preserves existing native tool (no duplication) |
| `PI_ANTHROPIC_CODE_EXECUTION` is enabled and a function variant named `code_execution` is present | strips function variant and keeps/uses native variant |
| `PI_ANTHROPIC_CODE_EXECUTION` is disabled or unset | no-op |
| API is non-Anthropic | no-op |

Truthy values for `PI_ANTHROPIC_CODE_EXECUTION` are: `1`, `true`, `yes`, `on` (case-insensitive, surrounding whitespace allowed).

It also appends a system-prompt section for Anthropic sessions indicating native `code_execution` availability when enabled.

## Installation

The package targets the [`pi`](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent) coding agent. Pi loads extensions from `~/.pi/agent/extensions/`, project `.pi/extensions/`, or via the `--extension` / `-e` CLI flag.

```bash
# From npm (once published)
pi install npm:pi-anthropic-code-execution

# From git
pi install git:github.com/code-yeongyu/pi-anthropic-code-execution

# Manual placement
git clone https://github.com/code-yeongyu/pi-anthropic-code-execution ~/.pi/agent/extensions/pi-anthropic-code-execution
cd ~/.pi/agent/extensions/pi-anthropic-code-execution && npm install

# Dev / one-shot test
pi -e /path/to/pi-anthropic-code-execution/src/index.ts
```

After installation, restart pi or run `/reload` inside an interactive session.

## Development

```bash
npm install
npm test
npm run typecheck
npm run check
pi -e ./src/index.ts
```

The test suite uses vitest. TypeScript is strict, Node-only, and uses ESM imports with `.js` suffixes.

## Origin

Ported from `packages/coding-agent/src/core/extensions/builtin/anthropic-code-execution/index.ts` in `code-yeongyu/senpi-mono`.

## License

[MIT](LICENSE).

## Related

- [senpi](https://github.com/code-yeongyu/senpi) — the fork/runtime these extensions are extracted from.
- [Ultraworkers Discord](https://discord.gg/PUwSMR9XNk) — community link from the senpi README.
- [Dori](https://sisyphuslabs.ai) — the product powered by senpi under the hood.
