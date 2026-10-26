# setup-nimbus

Install the [Renmy](https://renmy.net) CLI in your GitHub Actions workflow and run your Salesforce Apex tests locally in CI — no org, no scratch-org pool, no JWT certificate, no Docker, no JVM.

Renmy executes real Apex — classes, triggers, SOQL, DML, and Flows — against an embedded PostgreSQL database. A typical test finishes in milliseconds, so a full suite that takes 90 minutes against an org finishes in seconds to minutes on a standard GitHub runner.

## Usage

```yaml
name: Apex Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: renmy-solutions/setup-nimbus@v1

      - name: Run Apex tests
        run: renmy test "*" --coverage-report coverage.xml --results-xml results.xml
```

The JUnit XML (`--results-xml`) and Cobertura coverage (`--coverage-report`) outputs plug directly into GitHub test reporting, SonarQube, and Codecov. See the [CI/CD guide](https://renmy.net/ci) for GitLab CI, coverage gates, and coverage-delta reporting.

## Inputs

| Input     | Description                                          | Default  |
|-----------|------------------------------------------------------|----------|
| `version` | Renmy version to install (e.g. `1.1.12`) or `latest` | `latest` |

## Outputs

| Output    | Description                        |
|-----------|------------------------------------|
| `version` | The Renmy version that was installed |

## Pinning a version

```yaml
- uses: renmy-solutions/setup-nimbus@v1
  with:
    version: '1.1.12'
```

## The renmy and nimbus commands

The action puts both `renmy` and `nimbus` on `PATH`, so a workflow can use either name. Releases before 1.50.0 only answer to `nimbus`; for them the action creates `renmy` as a link to the same file (a copy on Windows), which means `renmy test` works with every version you can pin. Workflows that already call `nimbus` need no change.

## Runners

Linux, macOS, and Windows runners are supported, on both x64 and ARM64. Release binaries are verified against the published SHA-256 checksums before installation.

## License keys (Pro/Team)

The free tier needs no configuration. For Pro/Team features in CI, set your license key as a repository secret:

```yaml
- name: Run Apex tests
  run: renmy test "*" --coverage-report coverage.xml
  env:
    NIMBUS_LICENSE_KEY: ${{ secrets.NIMBUS_LICENSE_KEY }}
```

## Links

- [Website](https://renmy.net) · [Docs](https://renmy.net/docs) · [Quickstart](https://renmy.net/quickstart)
- [CI/CD guide](https://renmy.net/ci) — GitHub Actions, GitLab CI, coverage gates
- [Main repository](https://github.com/renmy-solutions/renmy)
