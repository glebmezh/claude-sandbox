# claude-sandbox

Run Claude Code in a Docker container sandboxed to your current working directory, with `--dangerously-skip-permissions` enabled.

## What it does

- Mounts only your **current directory** into the container — Claude can't touch the rest of your filesystem
- Runs as your host UID so file ownership is seamless
- Shares your `~/.claude` config (MCP servers, settings, memory) with the container
- Auto-builds the Docker image on first run

## Install

```bash
# Clone or copy the script somewhere on your PATH, e.g.:
git clone https://github.com/glebmezh/claude-sandbox ~/src/claude-sandbox

# Add to PATH in ~/.zprofile or ~/.zshrc:
export PATH="$HOME/src/claude-sandbox:$PATH"
```

## Usage

```bash
# From any project directory:
claude-sandbox

# Rebuild the Docker image (e.g. after a Claude Code release):
claude-sandbox --rebuild

# Pass extra flags to claude:
claude-sandbox --model claude-opus-4-6
```

## Authentication

The container shares your host `~/.claude` config, so if you're already authenticated on the host, it just works.

**First-time setup** (or if MCP tools are missing): run `/login` inside the container once:

```bash
claude-sandbox
# inside the container:
/login
```

Claude prints a URL + one-time code — paste the code in your browser. No port forwarding needed. Credentials are written to the mounted `~/.claude` and persist for all future sessions.

## Requirements

- Docker
- `ANTHROPIC_API_KEY` set in your environment (or use `/login` for claude.ai OAuth)
