# depwalk-action

> GitHub Action to run [@sulthonzh/depwalk](https://github.com/sulthonzh/depwalk) security audits in your CI pipeline.

Ever merged a PR that added a dependency with a sketchy install script? Or shipped deprecated packages to production? This action catches that stuff automatically.

## What it checks

- **Install scripts** — flags `curl`, `wget`, `node -e`, and env access as suspicious
- **Deprecated packages** — surfaces packages the maintainers abandoned
- **Missing licenses** — because legal teams exist
- **Bin name mismatches** — when a package tries to shadow another CLI tool
- **Crypto wallet funding** — because that's a thing apparently

## Quick start

```yaml
# .github/workflows/deps.yml
name: Dependency Audit
on: [push, pull_request]

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm install
      - uses: sulthonzh/depwalk-action@v1
```

That's it. If high-severity issues are found, the workflow fails.

## Configuration

| Input | Default | Description |
|-------|---------|-------------|
| `command` | `check` | depwalk command: `check`, `outdated`, `size`, `lockfile`, `graph` |
| `directory` | `.` | Path to project directory |
| `severity` | `low` | Minimum severity to report: `high`, `medium`, `low` |
| `output` | `text` | Output format: `text`, `json`, `markdown` |
| `fail-on` | `high` | Fail workflow at severity: `high`, `medium`, `low`, `none` |

## Examples

### Fail on medium severity

```yaml
- uses: sulthonzh/depwalk-action@v1
  with:
    fail-on: medium
```

### Markdown report in PR summary

```yaml
- uses: sulthonzh/depwalk-action@v1
  with:
    output: markdown
```

This adds a nice formatted table to the GitHub Actions summary.

### Check a specific workspace package

```yaml
- uses: sulthonzh/depwalk-action@v1
  with:
    directory: ./packages/my-app
```

### Outdated dependencies check

```yaml
- uses: sulthonzh/depwalk-action@v1
  with:
    command: outdated
    fail-on: none  # just report, don't fail
```

### Use the output programmatically

```yaml
- uses: sulthonzh/depwalk-action@v1
  id: audit
  with:
    output: json

- name: Process results
  run: echo "${{ steps.audit.outputs.result }}" | jq '.issues | length'
```

## Why not just use `npm audit`?

`npm audit` checks for known CVEs. depwalk checks for *suspicious patterns* — things that don't have a CVE yet but are still sketchy. They complement each other.

Run both:

```yaml
- run: npm audit --audit-level=high
- uses: sulthonzh/depwalk-action@v1
```

## License

MIT
