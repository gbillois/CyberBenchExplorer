# Wavestone Cyberbenchmark Explorer

Standalone HTML dataviz to explore a completed W-CyberBenchmark file from GitHub Pages.
The UI defaults to English and automatically switches to French when the browser language starts with `fr`.

## Utilisation

1. Publish the repository with GitHub Pages.
2. Open `index.html`.
3. A complete synthetic data set is displayed by default for testing without a real file (`data/dummy-assessment.json`: 220 questions whose IDs, subtopics, and assessment points mirror the real W-CyberBenchmark question bank, and `data/dummy-benchmark.json`: question-level Top 10 referential). Only the scores, maturity levels, comments, and reliability are synthetic.
4. Upload an assessment `.xlsm`, `.xlsx`, `.xls`, or `.xlsb` file to replace the example.
5. Optionally upload a benchmark file with columns such as `ID` or `Domain`, then `Average`, `Sector`, and `Top 10`.

Files are read locally in the browser. No backend is used.
SheetJS is vendored in `vendor/` to avoid a runtime CDN dependency.
Demo data is stored as static JSON files. If those files are missing or invalid, `index.html` loads a minimal fallback to avoid an empty screen. User-uploaded files are neither stored in the browser nor sent to a server.

## Expected Benchmark

The referential can be at domain level or question level.

Colonnes recommandées :

| ID | Domain | Global average | Sector average | Top 10 | Top 10 maturity |
| --- | --- | ---: | ---: | ---: | --- |
| GOV.01 | GOV | 62% | 67% | 84% | L4 |
| RISK.01 | RISK | 55% | 60% | 79% | L4 |

When `ID` values match questionnaire questions, the Compare tab displays lagging measures. The demo JSON referential also adds `topMaturityLevel` and `topLevels` per question to test Top 10 maturity comparison.
