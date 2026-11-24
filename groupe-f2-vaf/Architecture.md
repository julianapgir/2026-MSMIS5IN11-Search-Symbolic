
# Architecture du projet F2 — Argumentation à valeurs

## 1. Objectif du projet

Ce projet est un prototype pédagogique qui permet de représenter un débat autour d'une décision médicale.

Il modélise :
- des arguments qui présentent différentes raisons ;
- des attaques, qui représentent les désaccords entre arguments ;
- des valeurs, comme l'efficacité, la sécurité ou la qualité de vie ;
- des préférences, qui indiquent quelles valeurs sont prioritaires pour une audience donnée.

Le programme compare deux approches :
- **Dung (AF)** : analyse les arguments et leurs attaques, sans tenir compte des valeurs ;
- **VAF** : ajoute les valeurs et les priorités de l'audience pour déterminer quelles attaques restent actives.

L'objectif est d'observer comment les résultats de l'argumentation évoluent selon les priorités retenues.

> Le programme est un outil de modélisation pédagogique. Il ne fournit pas de diagnostic et ne recommande pas de traitement médical.

---

## 2. Architecture générale

Le projet est organisé en quatre parties fonctionnelles :

1. **Définition du scénario** : création des arguments, des attaques, des valeurs et des audiences.
2. **Modélisation de l'argumentation** : construction du cadre de Dung et du cadre VAF.
3. **Calcul des résultats** : application des différentes sémantiques d'argumentation.
4. **Affichage et vérification** : présentation des résultats et tests automatiques.

### Schéma de fonctionnement

```text
                   medical_case.py
                 (Données du scénario)
                         |
          Arguments, attaques, valeurs,
                profils d'audience
                         |
                         v
                  argumentation.py
                  (Moteur de calcul)
                         |
             +-----------+-----------+
             |                       |
             v                       v
         Modèle Dung              Modèle VAF
          (AF de base)        (AF + valeurs)
             |                       |
             |              Classement des valeurs
             |                       |
             |              Filtrage des attaques
             |                       |
             |                       v
             |                AF propre à l'audience
             |                       |
             +-----------+-----------+
                         |
                         v
                 Calcul des extensions
             Grounded, Preferred, Stable,
                 Semi-stable, Ideal
                         |
                         v
                      main.py
              (Exécution et affichage)
                         |
                         v
                  Résultats comparés


             tests/test_argumentation.py
                (Vérification du moteur)
```

---

## 3. Organisation des fichiers

```text
groupe-f2-vaf/
│
├── README.md
│
├── Architecture.md
│
├── src/
│   ├── argumentation.py
│   ├── medical_case.py
│   └── main.py
│
└── tests/
    └── test_argumentation.py
```

### Rôle de chaque fichier

| Fichier | Fonction |
|---|---|
| `medical_case.py` | Contient les données du scénario médical : arguments, attaques, valeurs associées et profils d'audience. |
| `argumentation.py` | Contient les fonctions qui construisent et analysent les cadres Dung et VAF. C'est le moteur de calcul du projet. |
| `main.py` | Lance les calculs, organise leur déroulement et affiche les résultats. |
| `test_argumentation.py` | Vérifie que les fonctions d'argumentation respectent les règles attendues. |
| `README.md` | Présente le projet et explique comment l'installer et l'exécuter. |
| `Architecture.md` | Décrit l'organisation du code et les liens entre ses différentes parties. |

---

## 4. Les notions utilisées

### Argument

Un argument est une affirmation qui apporte une raison dans le débat.

Dans le code, chaque argument possède un identifiant : `A1`, `A2`, `A3`, etc.

### Attaque

Une attaque représente une contestation entre deux arguments.

Par exemple :

```text
A1 → A2
```

Cela signifie que l'argument A1 attaque l'argument A2.

### Cadre d'argumentation de Dung (AF)

Le modèle de Dung, ou *Argumentation Framework*, représente un débat à partir de deux éléments :
- un ensemble d'arguments ;
- un ensemble d'attaques entre ces arguments.

Il permet d'étudier quels groupes d'arguments sont cohérents et défendables.

### Valeur

Une valeur correspond au principe défendu par un argument.

Dans le scénario médical, les valeurs utilisées sont :
- efficacité ;
- sécurité ;
- qualité de vie.

### Audience

Une audience représente un profil de préférences entre les valeurs.

Par exemple, une audience peut classer les valeurs dans cet ordre :

```text
Efficacité > Sécurité > Qualité de vie
```

Cela signifie que l'efficacité est prioritaire sur la sécurité, qui est elle-même prioritaire sur la qualité de vie.

### Cadre d'argumentation à valeurs (VAF)

Le VAF reprend le cadre de Dung et ajoute :
- une valeur associée à chaque argument ;
- un ordre de préférence entre les valeurs, défini par l'audience.

Cet ajout permet de déterminer si une attaque doit être conservée ou bloquée selon les priorités de l'audience.

---

## 5. Fonctionnement détaillé des modules

### 5.1. `medical_case.py` — Définition du scénario

Ce fichier contient les données utilisées pour les simulations.

Il définit :
- les arguments du cas médical ;
- les attaques entre les arguments ;
- la valeur associée à chaque argument ;
- les différents profils d'audience.

Les données sont séparées du moteur de calcul afin de pouvoir modifier le scénario sans devoir réécrire les fonctions d'argumentation.

**Entrée :** aucune donnée externe nécessaire.

**Sortie :** les éléments qui seront transmis au moteur d'argumentation.

### 5.2. `argumentation.py` — Moteur de calcul

Ce fichier regroupe les fonctions principales du projet.

Il comporte deux ensembles de fonctionnalités.

#### A. Les fonctions du modèle de Dung

Elles permettent de construire et d'analyser un cadre d'argumentation classique.

| Fonction | Rôle |
|---|---|
| `AF(args, attacks)` | Représente un cadre d'argumentation avec ses arguments et ses attaques. |
| `attackers` | Identifie les arguments qui attaquent un argument donné. |
| `is_conflict_free` | Vérifie qu'un ensemble ne contient pas d'arguments qui s'attaquent entre eux. |
| `defeats` | Vérifie si un argument attaque un autre argument. |
| `defends` | Vérifie si un ensemble défend un argument contre ses attaquants. |
| `is_admissible` | Vérifie qu'un ensemble est sans conflit et défend tous ses arguments. |
| `grounded` | Calcule l'extension grounded. |
| `preferred` | Calcule les extensions preferred. |
| `stable` | Calcule les extensions stables. |
| `semi_stable` | Calcule les extensions semi-stables. |
| `ideal` | Calcule l'extension idéale. |
| `summarize` | Regroupe les résultats des différentes sémantiques. |

#### B. Les fonctions du modèle VAF

Elles ajoutent la prise en compte des valeurs et des préférences.

| Fonction | Rôle |
|---|---|
| `VAF(args, attacks, values)` | Associe une valeur à chaque argument et construit le modèle VAF. |
| `audience_rank` | Classe les valeurs selon les priorités de l'audience. |
| `defeats_under_audience` | Vérifie si une attaque est conservée selon l'ordre de préférence. |
| `af_from_audience` | Construit un cadre de Dung à partir des attaques conservées pour une audience donnée. |

**Principe important :** le VAF ne remplace pas les calculs de Dung. Il filtre d'abord les attaques, puis réutilise les fonctions de Dung pour calculer les extensions.

### 5.3. `main.py` — Exécution du projet

Ce fichier coordonne les différentes étapes.

Il :
1. récupère le scénario défini dans `medical_case.py` ;
2. construit le cadre de Dung initial ;
3. calcule les extensions de référence ;
4. construit le modèle VAF ;
5. traite chaque profil d'audience ;
6. filtre les attaques selon les préférences ;
7. recalcule les extensions ;
8. affiche les résultats pour permettre leur comparaison.

Il ne contient pas les règles mathématiques des modèles : celles-ci sont définies dans `argumentation.py`.

### 5.4. `test_argumentation.py` — Tests automatiques

Ce fichier vérifie le fonctionnement des fonctions du moteur.

Les tests portent notamment sur :
- les règles de défense et d'admissibilité ;
- le calcul de plusieurs sémantiques ;
- le fonctionnement du VAF ;
- le traitement de deux arguments associés à une même valeur ;
- le filtrage des attaques selon les préférences ;
- la validation des profils d'audience.

Les tests permettent de détecter une erreur de programmation, mais ne valident pas la pertinence médicale des arguments ou des attaques.

---

## 6. Déroulement d'une simulation

Voici le parcours complet d'une simulation :

**Étape 1 — Définir le débat**

Le fichier `medical_case.py` fournit les arguments, les attaques et les valeurs.

**Étape 2 — Calculer le modèle de référence**

Le programme construit un cadre de Dung. Toutes les attaques initialement définies sont prises en compte.

**Étape 3 — Définir les priorités**

Le programme récupère un profil d'audience, qui classe les valeurs de la plus prioritaire à la moins prioritaire.

**Étape 4 — Filtrer les attaques**

Pour chaque attaque, le programme compare la valeur de l'argument attaquant à celle de l'argument ciblé.

La règle utilisée est :

```text
L'attaque X → Y est conservée si :

rang(valeur de X) <= rang(valeur de Y)
```

Le rang `0` correspond à la valeur la plus prioritaire.

Si les deux arguments ont la même valeur, l'attaque est conservée.

**Étape 5 — Construire le cadre adapté à l'audience**

Les attaques conservées forment un nouveau cadre de Dung, propre au profil de préférences étudié.

**Étape 6 — Calculer les extensions**

Les fonctions de Dung sont réutilisées pour identifier les ensembles d'arguments obtenus.

**Étape 7 — Comparer les résultats**

Le programme affiche les résultats du modèle de référence et ceux des différentes audiences.

---

## 7. Comment interpréter les résultats ?

Le programme calcule plusieurs sémantiques :

| Sémantique | Signification simplifiée |
|---|---|
| Grounded | Ensemble obtenu par une construction prudente et progressive. |
| Preferred | Ensembles admissibles maximaux. Il peut y en avoir plusieurs. |
| Stable | Ensemble qui attaque tous les arguments qui n'en font pas partie. |
| Semi-stable | Ensemble admissible dont la portée est maximale. |
| Ideal | Plus grand ensemble admissible commun à toutes les extensions preferred. |

Une extension est un ensemble d'arguments retenu par une règle formelle.

Elle ne signifie pas qu'un traitement est médicalement meilleur ou qu'il doit être choisi.

La comparaison entre les audiences permet d'observer l'influence des priorités sur les attaques conservées et sur les résultats de l'argumentation.

---

## 8. Limites et points de vigilance

- Le scénario médical est fictif.
- Les arguments, les attaques et les valeurs sont des choix de modélisation qui doivent être justifiés.
- Les profils d'audience sont définis à titre d'exemple ; ils ne correspondent pas à des préférences réellement recueillies auprès de patients.
- Deux audiences peuvent produire les mêmes résultats si le graphe d'attaques ne permet pas de les distinguer.
- Le programme est conçu pour un prototype de petite taille. L'énumération des sous-ensembles d'arguments peut devenir coûteuse lorsque leur nombre augmente.
- Les tests vérifient la cohérence du code, pas la validité clinique du scénario.

---

## 9. Exécution du projet

Depuis le dossier `groupe-f2-vaf`, ouvrir un terminal PowerShell.

Créer et activer un environnement virtuel :

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Lancer la simulation :

```powershell
python src/main.py
```

Lancer les tests :

```powershell
python -m unittest discover -s tests -v
```

---

## 10. À retenir

L'architecture repose sur une séparation entre les données, les règles de calcul et l'exécution :

- `medical_case.py` définit le cas étudié.
- `argumentation.py` contient les règles de Dung et de VAF.
- `main.py` organise les simulations et affiche les résultats.
- `test_argumentation.py` vérifie le fonctionnement du moteur.

**Le lien central du projet est le suivant : le VAF utilise les priorités de valeurs pour filtrer les attaques, puis le modèle de Dung analyse le débat obtenu.**

