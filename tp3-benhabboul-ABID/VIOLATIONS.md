# Diagnostic des Violations SOLID et Analyse Data

## Tableau des Violations

| # | Principe | Ligne(s) | Problème | Conséquence concrète |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **S** (Single Responsibility) | 21-72 | La classe fait tout : lecture de fichier, calculs métier, formatage, journalisation et envoi d'e-mails. | Modifier le système d'envoi d'e-mail risque de casser la logique de calcul de la TVA. La classe est un "fourre-tout". |
| **2** | **O** (Open/Closed) | 43-55 | Présence d'un grand `if / else if` pour déterminer le format de sortie (`csv`, `html`, `json`). | Pour ajouter un format (ex: PDF), il faut modifier le code interne de la méthode, au lieu de l'étendre. |
| **3** | **L** (Liskov Substitution) | 78-81 | `FreeReportService` lève des exceptions pour des formats ou envois que la classe parente accepte. | Si une fonction attend un `ReportService`, elle plantera inopinément si on lui passe un `FreeReportService`. |
| **4** | **D** (Dependency Inversion) | 7-8, 16 | Les dépendances techniques (nom du fichier `orders.json`, serveur SMTP) sont instanciées en dur. | Impossible de tester les calculs sans un vrai fichier physique et sans tenter d'envoyer un e-mail. |
| **5** | Autre (Couplage fort) | 59 | Appel direct à `console.log` au beau milieu de la logique métier. | Impossible de rediriger les logs vers un fichier ou un système de monitoring sans modifier la classe métier. |

## Réponse Spécialité DATA

* **Où est le calcul des indicateurs ?** 
  Il est enfoui au milieu de la méthode `generate()` (lignes 25 à 40) de la classe `ReportService`.
* **Peut-on le réutiliser pour un tableau de bord sans le fichier ni l'e-mail ?** 
  Non. Comme les calculs sont mélangés à la lecture du fichier (`loadOrders`) et à l'envoi d'e-mails, il est impossible de récupérer uniquement les statistiques brutes pour un tableau de bord sans générer de fichier texte et envoyer un e-mail.
* **Quelles règles de qualité des données sont appliquées ?** 
  - *Commandes annulées :* Ignorées (`if (o.status === 'CANCELLED') continue;`).
  - *Mois sans commande :* Division par zéro évitée (`count > 0`).
  - *Arrondis :* La TVA et le panier moyen sont arrondis à deux décimales.
* **Laquelle manque selon vous ?** 
  Il manque une vérification sur les montants négatifs (quantité ou prix sous zéro) et sur les données incomplètes (ligne sans catégorie). De plus, le `revenueTTC` n'est pas explicitement arrondi, ce qui peut causer des erreurs de précision flottante.