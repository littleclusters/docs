---
title: CI/CD
weight: 2
---

# CI/CD

Run `lc` tests automatically in GitHub Actions.

## GitHub Actions

Add `.github/workflows/littleclusters.yaml` to your repository:

```yaml
name: LittleClusters Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: littleclusters/action@main
```

The action runs `lc test` on every push to main and on pull requests.

### Custom Working Directory

If your `littleclusters.yaml` isn't at the repository root:

```yaml
- uses: littleclusters/action@main
  with:
    working-directory: './my-challenge'
```
