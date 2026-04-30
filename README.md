# Code Review Plugin for local-ai-home-assistant

This repository contains a Home Assistant Local AI plugin for structured code review. The plugin provides git diff and code snippet analysis with security pattern detection, quality checks, and review guidance.

## Features

- Review git diffs and staged changes
- Analyze code snippets for security and quality issues
- Detect patterns like SQL injection, shell injection, hardcoded secrets, weak crypto, XSS, path traversal, and more
- Build a summary report with risk level, file breakdown, and review checklist
- Compatible with the `local-ai-home-assistant` plugin model

## Plugin Tools

The plugin exposes the following worker tools:

- `review_diff`
  - Review a git diff between two refs
  - Parameters: `cwd`, `base`, `head`, `filePaths`

- `review_staged`
  - Review currently staged changes
  - Parameters: `cwd`

- `review_code_snippet`
  - Analyze a standalone code snippet
  - Parameters: `code`, `language`

- `review_security_only`
  - Run only security analysis on code or diff
  - Parameters: `cwd`, `code`, `base`, `head`

- `review_get_context`
  - Get current branch and recent commits
  - Parameters: `cwd`, `commitCount`

## Installation

1. Place the `code-review` plugin folder inside your `local-ai-home-assistant` plugins directory or configure the plugin loader to include this repository.
2. Ensure the runtime has access to the target git repository for `cwd`-based analysis.
3. Restart Local AI Home Assistant so the plugin is loaded.

## Example Usage

Use the plugin through the Local AI Home Assistant worker or agent framework by calling registered tools.

Example payload for `review_diff`:

```json
{
  "name": "review_diff",
  "args": {
    "cwd": "/path/to/repo",
    "base": "HEAD~1",
    "head": "HEAD"
  }
}
```

## Privacy and Security

- This plugin runs locally and does not transmit repository content externally by itself.
- It analyzes diffs and code snippets provided by the caller.
- Do not expose sensitive repository paths or private repo contents to untrusted agents or workflows.
- If you use this plugin inside `local-ai-home-assistant`, verify that the host environment and agent permissions are trusted.

## Contents

- `code-review/code-review-plugin.js` — plugin entrypoint and tool registration
- `code-review/lib/review-domain.js` — diff parsing, git helpers, and pattern detection

## License

This repository is released under the terms of the existing `LICENSE` file.
