# Qualflare — Upload Test Results (GitHub Action)

Upload your CI test results to [Qualflare](https://qualflare.com) in one step — AI failure analysis, history-based flaky-test detection, failure clustering, and release-risk scoring across **23 frameworks** (Playwright, Cypress, Jest, pytest, JUnit, and more), auto-detected.

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
| `token` | ✅ | — | Qualflare API token (`qf_…`). Store as an encrypted repo/org **secret**. |
| `project` | ✅ | — | Your Qualflare project identifier (the slug from the dashboard). |
| `results` | ✅ | — | Path or glob to result files, e.g. `test-results/*.xml`. Framework auto-detected. |
| `version` | | `latest` | `qualflare-cli` version to install, e.g. `0.1.9`. |

## Notes
- **Runners:** Linux and macOS (amd64/arm64) are fully supported; Windows is best-effort.
- **`continue-on-error: true`** on the test step lets results upload even when tests fail (that's when you most want the analysis).
- Get a token at **Project → Settings → Access Tokens** in the [Qualflare dashboard](https://app.qualflare.com), and store it as a GitHub secret named `QUALFLARE_TOKEN`.
- Wraps the [`qualflare-cli`](https://github.com/Qualflare/qualflare-cli) (`qf`) — see its docs for advanced usage.

## License

MIT — see [LICENSE](LICENSE).
