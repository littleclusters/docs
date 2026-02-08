---
title: CLI
weight: 1
---

# CLI

## Installation

### `go install`

```console
$ go install github.com/littleclusters/lc/cmd/lc@latest
```

This installs `lc` to your `$GOPATH/bin` directory. Make sure it's in your `$PATH`.

You can also install a specific version using tags (see [available versions](https://github.com/littleclusters/lc/tags)):

```console
$ go install github.com/littleclusters/lc/cmd/lc@v0.1.0
```

#### Update

```console
$ go install github.com/littleclusters/lc/cmd/lc@latest
```

### Homebrew

```console
$ brew tap littleclusters/tap
$ brew install littleclusters/tap/lc
```

#### Update

```console
$ brew upgrade littleclusters/tap/lc
```

### Verify Installation

```console
$ lc list
Available challenges:

  kv-store             - Distributed Key-Value Store (8 stages)

Start with: lc init <challenge-name>
```

## Quick Start

```console
$ lc init kv-store    # Create challenge in current directory
$ lc test             # Test your implementation
$ lc next             # Advance to the next stage
```

Edit `run.sh` to launch your implementation, then run `lc test` to get feedback.

> [!NOTE]
> Commands support short aliases for faster typing:
> - `lc i` → `lc init`
> - `lc t` → `lc test`
> - `lc n` → `lc next`
> - `lc s` → `lc status`
> - `lc l` or `lc ls` → `lc list`

## Basic Workflow

### 1. Start a Challenge

```console
$ lc init <challenge> [path]
```

Creates a new challenge directory with:
- `run.sh` - Script that builds and runs your implementation
- `README.md` - Challenge overview and requirements
- `lc.yaml`   - Tracks your progress
- `.gitignore` - Ignores `.lc/` working directory (server files and logs)

**Examples:**

```console
$ lc init kv-store           # Create in current directory
$ lc init kv-store my-kvs    # Create in ./my-kvs
```

### 2. Implement & Test

Edit `run.sh` to start your implementation.
The script must launch your server and pass through any arguments from `lc`:

```bash
#!/bin/bash -e

# Go
exec go run ./cmd/server "$@"

# Python
# exec python main.py "$@"

# Rust
# cargo build --release && exec ./target/release/kvstore "$@"

# Node.js
# exec node server.js "$@"
```

Then test:

```console
$ lc test
```

**When tests pass:**
```
PASSED ✓

Run 'lc next' to advance to the next stage.
```

**When tests fail:**
```
FAILED ✗

PUT http://127.0.0.1:45123/kv/ "foo"
  Expected: "key cannot be empty"
  Actual: ""

  Your server accepted an empty key when it should reject it.
  Add validation to return 400 Bad Request for empty keys.

Read the guide: littleclusters.com/kv-store/http-api
```

Fix the issues, then run `lc test` again. The CLI is designed for quick iteration, just keep running `lc test` as you make changes.

### 3. Progress Through Stages

```console
$ lc next
```

Advances to the next stage after verifying the current stage passes. Updates `lc.yaml` automatically.

If the current stage hasn't been completed, `lc next` runs tests first and only advances if they pass.

## Commands Reference

Run `lc --help` to see all available commands, or `lc <command> --help` for command-specific options.

### lc init

**Usage:** `lc init <challenge> [path]`

Creates a new challenge in the specified directory (or current directory if not specified).

### lc test

**Usage:** `lc test [stage]`

Runs tests for the current stage (from `lc.yaml`) or a specific stage if provided.

```console
$ lc test                # Test current stage
$ lc test persistence    # Test specific stage
```

### lc next

**Usage:** `lc next`

Advances to the next stage after verifying current stage passes all tests.

### lc status

**Usage:** `lc status`

Shows challenge progress and next steps:

```console
$ lc status
Distributed Key-Value Store

Build a distributed key-value store from scratch using the Raft consensus algorithm.

Progress:
✓ http-api           - Store and Retrieve Data
✓ persistence        - Data Survives SIGTERM
✓ crash-recovery     - Data Survives SIGKILL
→ leader-election    - Cluster Elects and Maintains Leader
  log-replication    - Data Replicates to All Nodes
  membership-changes - Add and Remove Nodes Dynamically
  fault-tolerance    - Cluster Survives Failures and Partitions
  log-compaction     - System Manages Log Growth

Read the guide: littleclusters.com/kv-store/leader-election

Implement leader-election, then run 'lc test'.
```

### lc list

**Usage:** `lc list`

Lists all available challenges with stage counts.

## Understanding lc.yaml

The config file tracks your progress:

```yaml
challenge: kv-store
stages:
  current: persistence
  completed:
    - http-api
```

You can edit this manually to jump between stages or reset progress. You can also use `lc test <stage>` to test any stage without changing your current progress.
