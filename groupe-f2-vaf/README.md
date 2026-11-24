# F2 — Argumentation à valeurs pour la décision médicale partagée

## Membres

- @julianapgir
- @Caro04lin

---

# Sujet

**F2 — Argumentation à valeurs pour la décision médicale partagée**

Le projet étudie comment des préférences différentes peuvent conduire à des décisions différentes à partir des mêmes informations médicales.

Nous utilisons :

- le cadre d’argumentation de Dung ;
- le Value-based Argumentation Framework (VAF) de Bench-Capon.

L’objectif est de montrer comment les valeurs prioritaires d’un patient influencent l’acceptation des arguments et la décision finale.

---

# Cas clinique

Le scénario étudié porte sur l’arthrose sévère du genou.

Deux options thérapeutiques sont comparées :

- chirurgie (prothèse du genou) ;
- traitement conservateur.

Dans une démarche de décision médicale partagée, tous les patients ne donnent pas la même importance à :

- la mobilité ;
- la sécurité ;
- la qualité de vie.

Le projet montre comment ces préférences modifient les conclusions du système d’argumentation.

---

# MVP

Le prototype :

1. modélise un débat médical sous forme de cadre d’argumentation ;
2. associe une valeur à chaque argument ;
3. filtre les attaques en fonction des préférences d’une audience ;
4. calcule cinq sémantiques d’argumentation :
   - grounded ;
   - preferred ;
   - stable ;
   - semi-stable ;
   - ideal ;
5. compare plusieurs profils de patients.

---

# Architecture

```text
groupe-f2-vaf/
│
├── README.md
├── Architecture.md
│
├── src/
│   ├── argumentation.py
│   ├── medical_case.py
│   ├── verifier.py
│   ├── benchmark.py
│   └── main.py
│
├── tests/
│   └── test_argumentation.py
│
├── notebooks/
│   └── analyse_vaf.ipynb
│
└── slides/
```

## Description des composants

### argumentation.py

Implémentation du moteur Dung et VAF :

- AF ;
- grounded ;
- preferred ;
- stable ;
- semi-stable ;
- ideal ;
- af_from_audience.

### medical_case.py

Définition du cas clinique :

- arguments ;
- valeurs ;
- attaques ;
- audiences.

### verifier.py

Vérificateur indépendant de l’extension grounded.

Permet de confirmer les résultats obtenus par le moteur principal.

### benchmark.py

Mesure des performances du calcul des sémantiques.

### main.py

Exécution du projet et comparaison des résultats Dung/VAF.

---

# Installation

Aucune dépendance externe n’est nécessaire.

Python 3.13 ou version compatible suffit.

---

# Exécution

Lancer le projet :

```powershell
python src/main.py
```

Exécuter les tests :

```powershell
python -m unittest discover -s tests -v
```

Mesurer les performances :

```powershell
python src/benchmark.py
```

---

# Résultats

## Profil 1

**Mobilité > Qualité de vie > Sécurité**

Résultat :

- chirurgie favorisée.

Extension grounded :

```text
{A1, A4}
```

---

## Profil 2

**Sécurité > Qualité de vie > Mobilité**

Résultat :

- traitement conservateur favorisé.

Extension grounded :

```text
{A2, A3, A4}
```

---

## Profil 3

**Qualité de vie > Mobilité > Sécurité**

Résultat :

- chirurgie favorisée.

Extension grounded :

```text
{A1, A4}
```

---

## Interprétation

Le système montre que des patients partageant les mêmes informations médicales peuvent parvenir à des conclusions différentes lorsque leurs priorités changent.

Le profil « sécurité » produit une conclusion différente car les arguments liés aux risques chirurgicaux deviennent dominants.

---

# Vérification indépendante

Le projet inclut un second module :

```text
src/verifier.py
```

Ce module recalcule indépendamment l’extension grounded.

Les résultats du vérificateur sont comparés automatiquement à ceux du moteur principal par les tests unitaires.

---

# Tests

Commande :

```powershell
python -m unittest discover -s tests -v
```

Résultat :

```text
10 tests exécutés
10 tests validés
```

Les tests couvrent :

- les propriétés du cadre de Dung ;
- les règles du VAF ;
- les audiences invalides ;
- le cas clinique ;
- le vérificateur indépendant.

---

# Mesures de performance

Résultats obtenus avec :

```powershell
python src/benchmark.py
```

| Nombre d'arguments | Temps |
|-------------------|--------|
| 4 | 0.208 ms |
| 8 | 1.699 ms |
| 12 | 18.916 ms |
| 16 | 267.576 ms |

Le temps d’exécution augmente rapidement avec le nombre d’arguments.

---

# Limites

- Cas clinique simplifié.
- Modélisation pédagogique des attaques.
- Petits graphes uniquement.
- Pas d’intégration Tweety.
- Profils de préférences illustratifs.
- Le système ne fournit pas de recommandation médicale réelle.

---

# Répartition du travail

### @julianapgir

- adaptation du cas clinique ;
- documentation ;
- notebook ;
- préparation de la soutenance.

### @Caro04lin

- implémentation du moteur ;
- vérificateur indépendant ;
- tests ;
- benchmark.

### Travail commun

- modélisation des arguments ;
- définition des attaques ;
- interprétation des résultats ;
- préparation des slides.

---

# Références

## Argumentation

Bench-Capon, T. (2003).
*Persuasion in Practical Argument Using Value-based Argumentation Frameworks.*

Elwyn, G. et al. (2012).
*Shared Decision Making: A Model for Clinical Practice.*

Supports de cours :

- Argumentation-03-Dung-AF-Semantics
- Argumentation-03b-Value-Based-AF

## Décision médicale partagée et arthrose du genou

Bussin, M. et al. (2021).
*A Personalized Shared Decision-Making Tool for Osteoarthritis Management of the Knee.*
PMC8011510.

https://pmc.ncbi.nlm.nih.gov/articles/PMC8011510/

Müns, A. et al. (2025).
*Shared decision-making and patient decision aids in knee osteoarthritis: a scoping review.*
PMC12927214.

https://pmc.ncbi.nlm.nih.gov/articles/PMC12927214/

Agency for Healthcare Research and Quality (AHRQ).

*Incorporating Patient-Reported Outcomes into Shared Decision Making with Patients with Osteoarthritis of the Knee.*

https://digital.ahrq.gov/ahrq-funded-projects/incorporating-patient-reported-outcomes-shared-decision-making-patients

## Cas clinique

Le scénario utilisé dans ce projet est une modélisation pédagogique inspirée de la littérature scientifique sur :

- la décision médicale partagée ;
- les préférences des patients ;
- le choix entre chirurgie et traitement conservateur dans l’arthrose du genou.

Il ne reproduit pas directement un article clinique unique et ne constitue pas une recommandation médicale.
