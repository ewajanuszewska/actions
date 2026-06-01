# GitHub Actions — Learning & Practice

A hands-on exploration of GitHub Actions concepts beyond basic CI/CD pipelines. This repository contains intentionally simple application code so the focus stays on the workflow mechanics.

## What This Repository Covers

### `workflow1.yml` — Advanced Pipeline Concepts

A multi-job pipeline demonstrating:

| Concept | Description |
|---|---|
| **Path filtering** | Only runs tests when `.py` files change, ignoring workflow file changes |
| **Job outputs** | Passes data between jobs using `$GITHUB_OUTPUT` |
| **Matrix strategy** | Tests in parallel on Python 3.10 and 3.11 simultaneously |
| **Reusable workflows** | Calls `reusable.yml` to avoid duplicating test logic |
| **Artifacts** | Uploads and downloads files between jobs |
| **Job dependencies** | `changes → test → deploy → print` sequential flow with `needs:` |
| **Conditional execution** | `if: ${{ always() }}` to control job execution |

### `reusable.yml` — Reusable Workflow

A callable workflow that accepts `python_version` as input, demonstrating:
- `workflow_call` trigger with typed inputs
- `actions/cache` for pip dependency caching
- Artifact upload for downstream jobs

### `output.yml` — GitHub Context

A simple `workflow_dispatch` workflow that outputs the full GitHub context as JSON — useful for understanding what data is available in workflows.

## Pipeline Flow

```
push to main
      │
      ▼
  [changes]  ← checks if .py files changed
      │
      ▼
  [test]     ← runs in parallel on Python 3.10 and 3.11
  (reusable.yml)
      │
      ▼
  [deploy]   ← downloads artifacts, publishes filename as output
      │
      ▼
  [print]    ← prints the filename from deploy job output
```

## Key Concepts Demonstrated

**Reusable workflows** avoid duplicating CI logic across repositories. Instead of copying the same test steps into every project, a central reusable workflow can be called with different inputs.

**Matrix strategy** runs the same job with different parameter combinations simultaneously — useful for cross-version or cross-platform testing.

**Job outputs** allow passing dynamic values between jobs, for example passing a generated filename from a deploy job to a notification job.

**Pip caching** uses `hashFiles('**/requirements.txt')` as the cache key — the cache invalidates automatically when dependencies change.

## Note

The application code (`app.py`, `app_test.py`) is intentionally minimal — the purpose of this repository is to explore workflow mechanics, not application development.

For a production CI/CD pipeline using these concepts applied to a real application, see [sales-fastapi-app](https://github.com/ewajanuszewska/sales-fastapi-app).
