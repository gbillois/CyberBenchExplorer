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
7. Use **Export all visuals to PPTX** to generate a 16:9 PowerPoint containing a title slide and every main chart.
8. Open the Recommendations tab to review the questions with the largest gaps to the market and static remediation actions derived from the questionnaire requirements.
9. Open the Ingest tab to paste a meeting transcript and configure Anthropic, OpenAI, Azure OpenAI, or Google Gemini. The app runs one assessment call and one independent check-and-challenge call per selected domain before results can be applied and exported to Excel.

Files are read locally in the browser. No backend is used.
SheetJS is vendored in `vendor/` to avoid a runtime CDN dependency.
PptxGenJS is also vendored in `vendor/` so PowerPoint exports work without a runtime CDN dependency.
Default data is stored as a static assessment JSON file and a benchmark workbook. If either file is missing or invalid, `index.html` loads a minimal fallback to avoid an empty screen. User-uploaded files are neither stored in the browser nor sent to a server.

Transcript ingestion is the exception: the transcript and selected questionnaire requirements are sent directly from the browser to the configured LLM provider. API keys are held only in memory and are not persisted. Obtain authorization before processing confidential data. For production use, route calls through an approved secure proxy.

## Expected Benchmark

The default and recommended import format is the `Score par catégorie` matrix in `data/Scores-CyberBenchmark-03062026.xlsx`: domains in the first column, then `Marché`, `Grandes Entreprises`, sector columns, and `Top 10`. Every numeric reference column is exposed automatically in the comparison-target selectors.

The referential can also be at domain level or question level using the tabular format below.

## Recommendations

Recommendations are generated locally and statically: each scored question is compared with the `Marché` score of its domain, then the next relevant L1-L5 requirements are proposed as remediation actions. No assessment comment is used to generate the static actions.

The application also prepares a versioned enrichment payload containing the static actions, assessment comments, expected evidence, and reliability. Future AI enrichment should send this payload through a secure server-side proxy that stores the API key; API keys must not be embedded in the static GitHub Pages application.

Colonnes recommandées :

| ID | Domain | Global average | Sector average | Top 10 | Top 10 maturity |
| --- | --- | ---: | ---: | ---: | --- |
| GOV.01 | GOV | 62% | 67% | 84% | L4 |
| RISK.01 | RISK | 55% | 60% | 79% | L4 |

When `ID` values match questionnaire questions, the Compare tab displays lagging measures. The demo JSON referential also adds `topMaturityLevel` and `topLevels` per question to test Top 10 maturity comparison.
