# setup-nimbus

Install the [Nimbus](https://testnimbus.dev) CLI in your GitHub Actions workflow and run your Salesforce Apex tests locally in CI — no org, no scratch-org pool, no JWT certificate, no Docker, no JVM.

Nimbus executes real Apex — classes, triggers, SOQL, DML, and Flows — against an embedded PostgreSQL database. A typical test finishes in milliseconds, so a full suite that takes 90 minutes against an org finishes in seconds to minutes on a standard GitHub runner.

## Usage

```yaml
name: Apex Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: nimbus-solution/setup-nimbus@v1

      - name: Run Apex tests
        run: nimbus test "*" --coverage-report coverage.xml --results-xml results.xml
```

The JUnit XML (`--results-xml`) and Cobertura coverage (`--coverage-report`) outputs plug directly into GitHub test reporting, SonarQube, and Codecov. See the [CI/CD guide](https://testnimbus.dev/ci) for GitLab CI, coverage gates, and coverage-delta reporting.

## Inputs

| Input     | Description                                          | Default  |
|-----------|------------------------------------------------------|----------|
| `version` | Nimbus version to install (e.g. `1.1.12`) or `latest` | `latest` |

## Outputs

| Output    | Description                        |
|-----------|------------------------------------|
| `version` | The Nimbus version that was installed |

## Pinning a version

```yaml
- uses: nimbus-solution/setup-nimbus@v1
  with:
    version: '1.1.12'
```

## Runners

Linux, macOS, and Windows runners are supported, on both x64 and ARM64. Release binaries are verified against the published SHA-256 checksums before installation.

## License keys (Pro/Team)

The free tier needs no configuration. For Pro/Team features in CI, set your license key as a repository secret:

```yaml
- name: Run Apex tests
  run: nimbus test "*" --coverage-report coverage.xml
  env:
    NIMBUS_LICENSE_KEY: ${{ secrets.NIMBUS_LICENSE_KEY }}
```

## Links

- [Website](https://testnimbus.dev) · [Docs](https://testnimbus.dev/docs) · [Quickstart](https://testnimbus.dev/quickstart)
- [CI/CD guide](https://testnimbus.dev/ci) — GitHub Actions, GitLab CI, coverage gates
- [Main repository](https://github.com/nimbus-solution/nimbus)
