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
