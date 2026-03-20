# Changelog

Alle nennenswerten Änderungen an diesem Projekt werden hier dokumentiert.

Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.0.0/).

---

## [Unreleased]

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
