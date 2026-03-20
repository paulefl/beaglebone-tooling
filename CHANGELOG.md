# Changelog

Alle nennenswerten Änderungen an diesem Projekt werden hier dokumentiert.

Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.0.0/).

---

## [Unreleased]

---

## [v1.1.14] — 2026-03-20

### Added

- `docs/sonarcloud-trend.md`: Dokumentation für SonarCloud Quality Metrics Trend-Tracking — erklärt automatische Snapshots, vorausgesetzte Einrichtung (Automatic Analysis deaktivieren) und Branch-Vergleich bei PRs (#2)
- `c_coverage.sh`: Shell-Helfer für C Code Coverage mit gcov/lcov — erzeugt `.info`-Datei für `sonar.c.lcov.reportPaths`, unterstützt mehrere Filter-Muster für System-Headers (#10)
- `.github/actions/c-coverage`: GitHub Composite Action für C Coverage — kapselt lcov-Installation, Coverage-Generierung und Artefakt-Upload; verwendbar via `uses: paulefl/beaglebone-tooling/.github/actions/c-coverage@main`
- `.github/actions/download-tooling`: GitHub Composite Action zum Herunterladen des Tooling-Releases — ersetzt 4× identischen Download-Block in beaglebone_black; unterstützt optionale Versionspinnung via `version`-Input
- `.github/actions/go-test`: GitHub Composite Action für die vollständige Go-Test-Pipeline — kapselt setup-go, download-tooling, gotestsum, Coverage Quality Gate (75%), Cobertura-Konvertierung, JUnit→SonarQube, Codecov-Upload und Artefakt-Upload
- `.github/actions/shellcheck-sarif`: GitHub Composite Action für Shell-Script-Qualität — bash -n Syntaxprüfung, ShellCheck JSON→SARIF via shellcheck_to_sarif.py und Artefakt-Upload
- `.github/actions/c-test`: GitHub Composite Action für die vollständige C-Test-Pipeline — bear + lcov Installation, Coverage-Build mit compile_commands.json, Testausführung, lcov-Report via c-coverage und Artefakt-Upload
- `.github/actions/strictdoc`: GitHub Composite Action für den vollständigen StrictDoc-Export — Python-Setup, StrictDoc + Test-Deps, Pytest JUnit-XML, Chrome-Check, make req-tracing, req_tracing_summary.py, HTML/PDF/Excel/ReqIF Artefakt-Upload
- `.github/actions/test-report`: GitHub Composite Action für den vollständigen Test-Report + GitHub Pages Deploy — Trend-Cache, trend_summary.py, generate_reports.py, reports-Branch-Persistenz und Pages-Deployment
- `.github/actions/sonarcloud`: GitHub Composite Action für SonarCloud Analysis — lädt alle Coverage- und SARIF-Artefakte, arrangiert C-Reports, führt Scan durch; erweiterbar für künftige SARIF-Reports (#5–#8, #9)
- `.github/actions/go-lint`: GitHub Composite Action für Go Linting — kapselt setup-go und `make lint`; ersetzt inline lint-go-Job in beaglebone_black
- `.github/actions/html-validate`: GitHub Composite Action für HTML-Validierung via Python's HTMLParser — konfigurierbarer Dateipfad
- `.github/actions/python-test`: GitHub Composite Action für die vollständige Python-Test-Pipeline — setup-python, pip install, pytest mit JUnit-XML + Coverage-XML, Artefakt-Upload
- `.github/actions/go-build`: GitHub Composite Action für plattformübergreifende Go-Binary-Builds — GOOS/GOARCH/GOARM als Inputs, kompatibel mit Matrix-Jobs; ersetzt inline build-cli und build-tui in beaglebone_black

### Changed (Issue #3)

- `go-test`: JUnit → SARIF Konvertierung via `junit2sarif` + Upload zu GitHub Code Scanning (`upload-sarif` Input, default true; erfordert `security-events: write`)
- `python-test`: JUnit → SARIF Konvertierung via `junit2sarif` + Upload zu GitHub Code Scanning (`upload-sarif` Input, default true; erfordert `security-events: write`)

### Added (Issue #4)

- `strictdoc_to_sarif.py`: Konvertiert StrictDoc-Anforderungen ohne `TYPE: File` Relations in SARIF 2.1.0 — erscheinen als Code Smells in SonarCloud und GitHub Code Scanning
- `strictdoc`: SARIF-Generierung via `strictdoc_to_sarif.py` + Upload zu GitHub Code Scanning + Artifact `requirements-sarif`; neue Inputs: `sdoc-glob`, `upload-sarif`, `sarif-artifact`; erfordert `security-events: write`
- `sonarcloud`: lädt `requirements-sarif` Artifact herunter (Issue #4); Placeholder für künftige SARIF-Issues (#5–#8, #9) bleibt erhalten

### Added (Issue #5)

- `.github/actions/python-security`: GitHub Composite Action für Python Security Scanning — bandit OWASP Scanner mit nativem SARIF-Output, Upload zu GitHub Code Scanning + SonarCloud via `python-sarif` Artifact; Inputs: `scan-targets`, `bandit-args`, `upload-sarif`, `artifact-name`
- `sonarcloud`: lädt `python-sarif` Artifact herunter (Issue #5)

### Added (Issue #6)

- `.github/actions/rust-lint`: GitHub Composite Action für Rust Linting — cargo clippy → SARIF 2.1.0 via `clippy-sarif` + `sarif-fmt`, Upload zu GitHub Code Scanning + SonarQube via `rust-sarif` Artifact
- `sonarcloud`: lädt `rust-sarif` Artifact herunter

### Added (Issue #7)

- `.github/actions/c-lint`: GitHub Composite Action für C Linting — clang-tidy → SARIF 2.1.0 via `clang-tidy-sarif` (pip), generiert `compile_commands.json` via `bear` falls nicht vorhanden, Upload zu GitHub Code Scanning + SonarQube via `c-sarif` Artifact
- `sonarcloud`: lädt `c-sarif` Artifact herunter

### Added (Issue #8)

- `go-lint`: SARIF-Generierung erweitert — `golangci-lint --out-format sarif` nach `make lint`; neue Inputs: `sarif-output`, `artifact-name: go-sarif`, `upload-sarif`, `golangci-lint-version`
- `sonarcloud`: lädt `go-sarif` Artifact herunter

### Added (Issue #9)

- `.github/actions/rust-coverage`: GitHub Composite Action für Rust Code Coverage — `cargo-llvm-cov` mit `llvm-tools-preview`, erzeugt Cobertura XML (`rust-coverage.xml`) und lcov-Report; optionaler Codecov-Upload; Artifact `rust-coverage`
- `sonarcloud`: lädt `rust-coverage` Artifact herunter

### Added (Issue #17)

- `.github/workflows/release-bump.yml`: Automatischer Release-Bump-Workflow — erstellt nach jedem Release einen Branch + PR in `beaglebone_black`, ersetzt alle `@vX.Y.Z`-Referenzen in `ci.yml`, wartet auf CI und merged automatisch; erfordert `BUMP_TOKEN` Secret mit `contents`, `pull_requests` und `workflow` Scope

### Fixed

- `python-security`: `bandit-sarif-formatter` wird jetzt explizit installiert — `bandit --format sarif` erfordert dieses Plugin und schlug ohne es fehl
- `release-bump`: bash -e Abbruch bei `gh pr checks` exit code 8 (pending checks) behoben
