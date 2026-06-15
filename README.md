# Wavestone Cyberbenchmark Workspace

Standalone GitHub Pages workspace to ingest, check, explore, and export a completed W-CyberBenchmark assessment.
The UI defaults to English and automatically switches to French when the browser language starts with `fr`.

## Applications

- `index.html`: workspace historique complet.
- `CyberBenchHelper.html`: application utilisateur pour transcrire et diariser un entretien (avec option d’anonymisation des noms, activée par défaut), corriger le transcript à partir de la terminologie du questionnaire, enrichir le questionnaire, lancer des passes indépendantes de Check & Challenge, contrôler le fichier (les anomalies sont signalées et corrigeables directement sur chaque mesure) et générer un email `.eml`. L’enrichissement et les corrections sont réécrits dans le fichier Excel/XLSM d’origine — sans recréer de nouveau fichier ni perdre les macros.
- `CyberBenchManager.html`: application équipe avec **Import & Check**, **Export results**, **Database Management** et **Réglages**. Le module d’export génère un classeur Excel de résultats et de comparaisons exactes sans inclure les notes des référentiels. Database Management valide les questionnaires un par un, extrait les informations de mission, organisation, secteur et taille, alerte sur les codes mission en doublon avec possibilité de forcer l’ajout, calcule les moyennes par domaine et signale les organisations présentant des écarts forts.

Helper et Manager conservent leur configuration dans le stockage local du navigateur. Les configurations et registres peuvent être exportés en JSON. Les JSON Helper et emails `.eml` peuvent contenir de véritables clés API et doivent être traités comme des secrets.

Les réglages de Helper et Manager proposent un design system Wavestone optionnel, partagé avec l'Explorer dans le même navigateur. Le style historique reste sélectionné par défaut. Les exports HTML autonomes embarquent le thème actif.

L’enregistrement SharePoint depuis Manager cible un répertoire SharePoint synchronisé localement par OneDrive via l’API navigateur de sélection de répertoire, disponible principalement dans Chrome et Edge.

Les règles métier considèrent que les pourcentages L1 à L4 doivent totaliser 100 %. L5 est un bonus additionnel, exclu de ce total.

Pour la transcription, OpenAI accepte les fichiers jusqu’à 25 Mo. Helper bascule automatiquement vers Azure Speech lorsqu’une clé Azure est configurée et que le fichier dépasse cette limite. L’endpoint Azure Fast Transcription utilisé accepte les fichiers de moins de 2 heures et jusqu’à 250 Mo. Les fichiers dépassant ces limites doivent être découpés avant import.

Azure OpenAI peut être utilisé pour l’analyse et pour la passe indépendante de Check & Challenge. Chaque configuration indique l’endpoint HTTPS, la région Azure utilisée (`westeurope` par défaut), le nom du déploiement et la clé API. La seconde passe peut réutiliser la configuration Azure d’analyse ou définir ses propres valeurs.

## Utilisation

1. Publish the repository with GitHub Pages.
2. Open `index.html`.
3. **Ingest**: upload the pre-positioned Cyberbenchmark Excel workbook, transcribe an audio/video file through Azure Speech, or paste a transcript. The LLM pipeline runs an extraction call and an independent check-and-challenge call per domain.
4. **Check**: validate initial business rules such as unique IDs, percentage bounds, scored-answer totals equal to 100%, and N/A consistency.
5. **Explore**: upload the score benchmark and access the existing Summary, Matrix, Radar, Domains, Findings, Comparison, Recommendations, and Response Details views.
6. **Export**: download the enriched Excel workbook, the central-database CSV, the complete 16:9 PowerPoint, or a standalone interactive HTML client report.
7. **Configuration**: configure Anthropic, OpenAI, Azure OpenAI, Google Gemini, and Azure Speech. API keys stay in memory only.

Files are read locally in the browser. No backend is used.
SheetJS is vendored in `vendor/` to avoid a runtime CDN dependency.
PptxGenJS is also vendored in `vendor/` so PowerPoint exports work without a runtime CDN dependency.
Default data is stored as a static assessment JSON file and a benchmark workbook. If either file is missing or invalid, `index.html` loads a minimal fallback to avoid an empty screen. User-uploaded files are neither stored in the browser nor sent to a server.

The standalone HTML client export embeds only sanitized, pre-calculated results. It excludes the benchmark, question-level maturity distributions, calculation requirements, and market values. It opens directly on Explore, keeps useful tabs, filters, sorting, drill-downs, and PNG exports, and does not load any external file or network resource.
Question- and domain-level score differences are exported as exact pre-calculated gaps for every available comparison referential. Benchmark source values are never embedded. Standalone radar, matrix, domain bars, question scatter, findings, recommendations, and response details use these pre-calculated gaps.
As with any local HTML file, a recipient can edit their own copy visually. Sanitized mode prevents benchmark extraction and score recalculation from source data; it does not make the file tamper-proof.

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
