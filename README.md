# TP 3 : Refactoring d'une « God class » (Principes SOLID et Injection de Dépendances)

**Auteurs :** Soufiane Benhabboul et Abid Faycal  
**Parcours :** Data / Cyber

## 📝 Description du projet

Ce projet a été réalisé dans le cadre de la séance 3 d'Architecture Logicielle. L'objectif était de diagnostiquer et d'amorcer le refactoring de la classe `ReportService`, une "God class" générant un rapport mensuel des ventes, en appliquant les principes **SOLID** et l'**Injection de Dépendances (DI)**.

Dans le cadre du parcours **Data / Cyber**, nous nous sommes concentrés sur l'analyse de l'existant, la modélisation architecturale, et une extraction ciblée.

## 📂 Contenu du livrable

* **`VIOLATIONS.md`** : Le diagnostic complet des 5 violations des principes SOLID (S, O, L, D, et couplage fort) ainsi que la réponse à la problématique de spécialité DATA (qualité des données, réutilisabilité des calculs et indicateurs manquants).
* **`DIAGRAMMES.md`** : La modélisation de l'architecture en Mermaid.
  * *Existant* : La classe ReportService et ses dépendances techniques directes.
  * *Cible* : L'architecture idéale après un refactoring complet (inversion de dépendances via interfaces).
* **`CsvFormatter.js`** : Extraction de la responsabilité de mise en forme CSV (Principe de Responsabilité Unique - SRP).
* **`ReportService.js`** : La classe métier refactorisée pour accepter l'injection du formateur CSV via son constructeur, tout en conservant son comportement par défaut.
* **`injection.test.js`** : Test unitaire prouvant que la responsabilité extraite peut être remplacée par l'injection d'un faux objet (mock).

## 🚀 Lancement et Tests

Le projet inclut des tests de caractérisation stricts pour garantir que le comportement de la classe n'a pas été altéré pendant le refactoring.

Pour lancer les tests (nécessite Node.js 20+) :

```bash
node --test *.test.js
