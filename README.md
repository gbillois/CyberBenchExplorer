# Wavestone Cyberbenchmark Workspace

Standalone GitHub Pages workspace to ingest, check, explore, and export a completed W-CyberBenchmark assessment.
The UI defaults to English and automatically switches to French when the browser language starts with `fr`.

## Utilisation

1. Publish the repository with GitHub Pages.
2. Open `index.html`.
3. **Ingest**: upload the pre-positioned Cyberbenchmark Excel workbook, transcribe an audio/video file through Azure Speech, or paste a transcript. The LLM pipeline runs an extraction call and an independent check-and-challenge call per domain.
4. **Check**: validate initial business rules such as unique IDs, percentage bounds, scored-answer totals equal to 100%, and N/A consistency.
5. **Explore**: upload the score benchmark and access the existing Summary, Matrix, Radar, Domains, Findings, Comparison, Recommendations, and Response Details views.
6. **Export**: download the enriched Excel workbook, the central-database CSV, and the complete 16:9 PowerPoint.
7. **Configuration**: configure Anthropic, OpenAI, Azure OpenAI, Google Gemini, and Azure Speech. API keys stay in memory only.

Files are read locally in the browser. No backend is used.
SheetJS is vendored in `vendor/` to avoid a runtime CDN dependency.
PptxGenJS is also vendored in `vendor/` so PowerPoint exports work without a runtime CDN dependency.
Default data is stored as a static assessment JSON file and a benchmark workbook. If either file is missing or invalid, `index.html` loads a minimal fallback to avoid an empty screen. User-uploaded files are neither stored in the browser nor sent to a server.

Transcript ingestion is the exception: transcripts and selected questionnaire requirements are sent directly from the browser to the configured LLM provider. Audio/video selected for transcription is sent to Microsoft Azure Speech. API keys are held only in memory and are not persisted. Obtain authorization before processing confidential data. For production use, route calls through an approved secure proxy.

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
