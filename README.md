# beaglebone-tooling

CI/CD Tooling für das [BeagleBone Black](https://github.com/paulefl/beaglebone_black) Projekt.

## Enthaltene Tools

| Datei | Beschreibung |
|-------|-------------|
| `collect_results.py` | Sammelt Go- und pytest-Testergebnisse → `test_results.json` |
| `generate_reports.py` | Erstellt HTML-Dashboard und PDF-Report aus Requirements + Testergebnissen |
| `generate_arch.py` | Generiert Architektur-Diagramme und Drone-CI-Pipeline-YAML |
| `report.sh` | Orchestriert den vollständigen Report-Lauf lokal |
| `build_adoc.sh` | Baut AsciiDoc-Dokumentation |
| `trend_summary.py` | Aktualisiert Trend-Daten für Dashboards |
| `req_tracing_summary.py` | Erzeugt Requirements-Traceability-Zusammenfassung |
| `junit_to_sonar_generic.py` | Konvertiert JUnit XML → Sonar Generic Test Format |
| `shellcheck_to_sarif.py` | Konvertiert shellcheck JSON → SARIF |
| `c_coverage.sh` | C Code Coverage mit gcov/lcov → `.info`-Datei für SonarCloud |
| `bausteinsicht` | Bausteinsicht-Diagramm-Generator (Binary) |

## Tests ausführen

```bash
pip install pytest
pytest tests/ -v
```

## GitHub Actions (Composite Actions)

### c-coverage — C Code Coverage mit gcov/lcov

```yaml
- name: Build c-lib with coverage
  run: make -C c-lib coverage CC=gcc

- name: Run C tests
  run: make -C c-lib test

- uses: paulefl/beaglebone-tooling/.github/actions/c-coverage@main
  with:
    source-dir: c-lib/
    output-file: reports/c-coverage.info
    filter-patterns: '/usr/*'   # optional, das ist der Default
```

Danach in `sonar-project.properties`:
```properties
sonar.c.lcov.reportPaths=reports/c-coverage.info
```

## Verwendung im BeagleBone-Projekt

Das Tooling wird via GitHub Releases eingebunden:

```yaml
- gh release download --repo paulefl/beaglebone-tooling --pattern "tooling-v*.tar.gz"
- tar xf tooling-v*.tar.gz
```

## SonarCloud Quality Metrics

Das Projekt nutzt SonarCloud für kontinuierliches Quality-Tracking. Jeder CI-Lauf erzeugt automatisch einen Snapshot (Coverage, Bugs, Technical Debt, Quality Gate).

→ Vollständige Dokumentation: [docs/sonarcloud-trend.md](docs/sonarcloud-trend.md)

## Release erstellen

```bash
git tag v1.0.0
git push origin v1.0.0
```
