# Qualflare — Upload Test Results (GitHub Action)

Upload your CI test results to [Qualflare](https://qualflare.com) in one step — AI failure analysis, history-based flaky-test detection, failure clustering, and release-risk scoring across **27 frameworks** (Playwright, Cypress, Jest, pytest, JUnit, and more), auto-detected.

Cleaner than scripting the CLI by hand: no `brew install`, no manual download.

## Usage

```yaml
- name: Run tests
  run: npx playwright test --reporter=junit
  continue-on-error: true

- name: Upload results to Qualflare
  uses: Qualflare/qualflare-action@v1
  with:
    token: ${{ secrets.QUALFLARE_TOKEN }}
    project: myapp
    results: test-results/*.xml
```

## Inputs

| Input | Required | Default | Description |
|-------|:---:|---|---|
| `token` | ✅ | — | Qualflare project access token (`qfp_…`). Store as an encrypted repo/org **secret**. |
| `project` | ✅ | — | Your Qualflare project identifier (the slug from the dashboard). |
| `results` | ✅ | — | Path or glob to result files, e.g. `test-results/*.xml`. Framework auto-detected. |
| `version` | | `latest` | `qualflare-cli` version to install, e.g. `0.4.0` (0.1.35 or later; earlier versions have no release download). |

## Notes
- **Runners:** Linux and macOS (amd64/arm64) are fully supported; Windows is best-effort.
- **`continue-on-error: true`** on the test step lets results upload even when tests fail (that's when you most want the analysis).
- Get a token at **Project Settings → Access Tokens** in the [Qualflare dashboard](https://app.qualflare.com), and store it as a GitHub secret named `QUALFLARE_TOKEN`.
- The token reaches `qf` through the `QF_TOKEN` environment variable, never as a command-line argument. `qf login` runs with `--force`, so a self-hosted runner that keeps its home directory between jobs works.
- `qf` also reads its other `QF_` variables (`QF_ENVIRONMENT`, `QF_MILESTONE`, `QF_BRANCH`, `QF_COMMIT`, …) from the job's `env:`.
- Wraps the [`qualflare-cli`](https://github.com/Qualflare/qualflare-cli) (`qf`). Full guide: [GitHub Actions in the Qualflare docs](https://docs.qualflare.com/docs/cli/ci-cd/github-actions).

## License

MIT — see [LICENSE](LICENSE).
