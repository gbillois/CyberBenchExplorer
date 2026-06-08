# Wavestone Cyberbenchmark Explorer

Standalone HTML dataviz to explore a completed W-CyberBenchmark file from GitHub Pages.
The UI defaults to English and automatically switches to French when the browser language starts with `fr`.

## Utilisation

1. Publish the repository with GitHub Pages.
2. Open `index.html`.
3. A synthetic assessment is displayed by default (`data/dummy-assessment.json`: 220 questions whose IDs, subtopics, and assessment points mirror the real W-CyberBenchmark question bank), together with the default benchmark (`data/Scores-CyberBenchmark-03062026.xlsx`). Only the assessment scores, maturity levels, comments, and reliability are synthetic.
4. Upload an assessment `.xlsm`, `.xlsx`, `.xls`, or `.xlsb` file to replace the example.
5. Optionally upload another benchmark file. Use `data/Scores-CyberBenchmark-03062026.xlsx` as the import template.
6. Use the PNG buttons to export each main visual.

Files are read locally in the browser. No backend is used.
SheetJS is vendored in `vendor/` to avoid a runtime CDN dependency.
Default data is stored as a static assessment JSON file and a benchmark workbook. If either file is missing or invalid, `index.html` loads a minimal fallback to avoid an empty screen. User-uploaded files are neither stored in the browser nor sent to a server.

## Expected Benchmark

The default and recommended import format is the `Score par catégorie` matrix in `data/Scores-CyberBenchmark-03062026.xlsx`: domains in the first column, then `Marché`, `Grandes Entreprises`, sector columns, and `Top 10`. Every numeric reference column is exposed automatically in the comparison-target selectors.

The referential can also be at domain level or question level using the tabular format below.

Colonnes recommandées :

| ID | Domain | Global average | Sector average | Top 10 | Top 10 maturity |
| --- | --- | ---: | ---: | ---: | --- |
| GOV.01 | GOV | 62% | 67% | 84% | L4 |
| RISK.01 | RISK | 55% | 60% | 79% | L4 |

When `ID` values match questionnaire questions, the Compare tab displays lagging measures. The demo JSON referential also adds `topMaturityLevel` and `topLevels` per question to test Top 10 maturity comparison.
