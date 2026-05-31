# CyberBench Explorer

Dataviz HTML autonome pour explorer un fichier W-CyberBenchmark rempli depuis GitHub Pages.

## Utilisation

1. Publier le repo avec GitHub Pages.
2. Ouvrir `index.html`.
3. Charger le fichier d'évaluation `.xlsm`, `.xlsx`, `.xls` ou `.xlsb`.
4. Optionnellement charger un fichier benchmark contenant des colonnes de type `ID` ou `Domain`, puis `Average` / `Moyenne`, `Sector` / `Secteur`, `Top 10`.

Les fichiers sont lus localement dans le navigateur. Aucun backend n'est utilisé.
La librairie SheetJS est embarquée dans `vendor/` pour éviter une dépendance CDN à l'exécution.

## Benchmark attendu

Le référentiel peut être au niveau domaine ou au niveau mesure.

Colonnes recommandées :

| ID | Domain | Global average | Sector average | Top 10 |
| --- | --- | ---: | ---: | ---: |
| GOV.01 | GOV | 62% | 67% | 84% |
| RISK.01 | RISK | 55% | 60% | 79% |

Quand les `ID` correspondent aux questions du questionnaire, l'onglet Comparaison affiche les mesures où le client est en retard.
