# Belgium house & apartment prices by municipality (2010–2026)

**English** · [Français](#français)

Quarterly **median sale prices of houses and apartments for every Belgian municipality** (INS/NIS code), from **Q1 2010 to Q1 2026**, in a tidy long CSV.
The figures are the medians published by **Statbel** (Belgian statistical office, FPS Economy), as taken over and reshaped by **[Levier](https://levier.be/)**.

- Live file (recomputed on each request): https://levier.be/api/donnees-ouvertes/csv
- Documentation page: https://levier.be/donnees-ouvertes (NL: https://levier.be/open-data)
- Licence of this file: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — credit **“Levier”** and link to **https://levier.be/**
- Upstream source: Statbel, [CC BY 4.0](https://statbel.fgov.be/en/cc-40) — “Source: Statbel”

This is **not** a scrape of property listings: no ads, no property addresses, no photos.

## Files

| File | Content |
|---|---|
| `data/levier-prix-communes-belgique.csv` | The dataset (snapshot of 8 October 2026, see below) |
| `LICENSE` | Official legal code of Creative Commons Attribution 4.0 International |
| `CITATION.cff` | Citation metadata (GitHub “Cite this repository”) |

## Snapshot in this repository

| | |
|---|---|
| Downloaded | 8 October 2026 from https://levier.be/api/donnees-ouvertes/csv |
| SHA-256 | `c832122d75bce06e258a409f79982c617e0755d3a265c3ebf4be004c5022b79a` |
| Size | 1,443,005 bytes, UTF-8 (no BOM), LF line endings, comma-separated, header row |
| Data rows | 22,991 (16,657 `maison` + 6,334 `appartement`) |
| Municipality codes | 437 INS codes (431 with at least one house row, 202 with at least one apartment row) |
| Period | Q1 2010 – Q1 2026: 65 quarters, none missing |
| Latest quarter (Q1 2026) | 373 rows (246 houses, 127 apartments) |
| Duplicates | none on (`code_ins`, `annee`, `trimestre`, `categorie`) |

The live file is regenerated on every request from Levier’s current copy of the Statbel data, so these counts can change. Use the SHA-256 above to identify this exact version.

## Columns

| Column | Type | Description |
|---|---|---|
| `code_ins` | string (5 digits) | Municipality code INS/NIS (REFNIS). Read it as text. |
| `commune_fr` | string | Municipality name in French, in UPPERCASE |
| `commune_nl` | string | Municipality name in Dutch (empty in 954 rows, see notes) |
| `annee` | integer | Year (2010–2026) |
| `trimestre` | integer | Quarter (1–4) |
| `periode` | string | Label `Q{trimestre} {annee}`, e.g. `Q2 2010` |
| `categorie` | string | `maison` (house) or `appartement` (apartment) |
| `prix_median_eur` | integer | Median sale price in euros, as published by Statbel |
| `nombre_transactions` | integer | Number of transactions behind the median |

Real example row:

```
44084,"AALTER","Aalter",2010,2,Q2 2010,maison,187500,23
```

## Method and notes

- **Source**: Statbel real estate price statistics ([“Prix de l’immobilier” / real estate prices](https://statbel.fgov.be/fr/themes/construction-logement/prix-de-limmobilier), [Statbel open data](https://statbel.fgov.be/en/open-data)). Statbel’s downloads include a quarterly file per municipality for 2010–2026. The house/apartment categories follow Statbel’s definitions.
- **Statbel method** (summary of Statbel’s published methodology): prices come from notarial deeds registered by FPS Finances (General Administration of Patrimonial Documentation); only the resale market is covered (new builds are excluded); both private-treaty and public sales are included; the price is the agreed sale price without taxes and fees; medians are published only for aggregates with at least 16 transactions with a valid price. Consistently, the smallest `nombre_transactions` in this snapshot is 16.
- **Changes made by Levier** (as required by the Statbel licence): data reshaped to a long format (one row = one municipality × one quarter × one category); a row is written **only if the Statbel median exists**, so `prix_median_eur` and `nombre_transactions` are never empty; French and Dutch names added from Levier’s postal reference; `periode` label added.
- **Missing medians are missing rows**, not empty cells. Do not read the absence of a row as zero sales.
- **Empty `commune_nl`**: 954 cells, for 27 old INS codes whose Dutch name is not in Levier’s postal reference (codes no longer in the current list of municipalities, e.g. `11007`, `11056`, `23023`, `23024`, `23032`, `37007`, `37015`, `37018`). All other columns of those rows are filled. In this snapshot, these 27 codes have no row after 2023.
- **Municipal mergers**: old and new INS codes can both appear over time. Check the code history before building long series for merged municipalities.
- Medians are not averages and are not adjusted for inflation or property size. Small transaction counts give volatile medians: filter on `nombre_transactions` if needed.

## Quick start (Python / pandas)

```python
import pandas as pd

url = "https://levier.be/api/donnees-ouvertes/csv"  # or "data/levier-prix-communes-belgique.csv"
df = pd.read_csv(url, dtype={"code_ins": str})

# Quarterly median house price in one municipality (Aalter, INS 44084)
aalter = df[(df.code_ins == "44084") & (df.categorie == "maison")]
print(aalter[["periode", "prix_median_eur", "nombre_transactions"]].tail())

# Wide table: one column per category
wide = df.pivot_table(index=["code_ins", "annee", "trimestre"],
                      columns="categorie", values="prix_median_eur")
```

## Licence

- **This CSV**: Creative Commons Attribution 4.0 International ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), full text in `LICENSE`). You may copy, share, redistribute and adapt it, including commercially, provided you credit **“Levier”** with a link to **https://levier.be/**.
- **Upstream data**: Statbel keeps its own licence ([CC BY 4.0, Statbel terms](https://statbel.fgov.be/en/cc-40)): mention “Source: Statbel”, link to the licence and state any changes. Statbel does not endorse Levier or this repository.

## How to cite

> Levier (2026). *Belgium house & apartment prices by municipality, Q1 2010–Q1 2026* [dataset, version of 8 October 2026]. https://levier.be/ — data: https://levier.be/donnees-ouvertes. Source: Statbel (Directorate-General Statistics – Statistics Belgium). Licence CC BY 4.0.

Short form for charts and articles: **Source: Levier (https://levier.be/), from Statbel data, CC BY 4.0.**

---

## Français

Prix **médians de vente des maisons et des appartements pour chaque commune belge** (code INS), par trimestre, du **1er trimestre 2010 au 1er trimestre 2026**, dans un CSV au format long.
Ce sont les médianes publiées par **Statbel** (SPF Économie), telles que **[Levier](https://levier.be/)** les a reprises et mises en forme.

- Fichier en ligne (recalculé à chaque requête) : https://levier.be/api/donnees-ouvertes/csv
- Page de documentation : https://levier.be/donnees-ouvertes (NL : https://levier.be/open-data)
- Licence de ce fichier : [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr) — citer **« Levier »** et faire un lien vers **https://levier.be/**
- Source amont : Statbel, [CC BY 4.0](https://statbel.fgov.be/fr/cc-40) — « Source : Statbel »

Ce n’est **pas** un extrait des annonces : pas d’annonces, pas d’adresses de biens, pas de photos.

### Fichiers

| Fichier | Contenu |
|---|---|
| `data/levier-prix-communes-belgique.csv` | Le jeu de données (instantané du 8 octobre 2026) |
| `LICENSE` | Texte juridique officiel de la licence Creative Commons Attribution 4.0 International |
| `CITATION.cff` | Métadonnées de citation (bouton GitHub « Cite this repository ») |

### Instantané de ce dépôt

| | |
|---|---|
| Téléchargé | le 8 octobre 2026 depuis https://levier.be/api/donnees-ouvertes/csv |
| SHA-256 | `c832122d75bce06e258a409f79982c617e0755d3a265c3ebf4be004c5022b79a` |
| Taille | 1 443 005 octets, UTF-8 sans BOM, fins de ligne LF, séparateur virgule, ligne d’en-tête |
| Lignes de données | 22 991 (16 657 `maison` + 6 334 `appartement`) |
| Codes commune | 437 codes INS (431 avec au moins une ligne maison, 202 avec au moins une ligne appartement) |
| Période | T1 2010 – T1 2026 : 65 trimestres, aucun manquant |
| Dernier trimestre (T1 2026) | 373 lignes (246 maisons, 127 appartements) |
| Doublons | aucun sur (`code_ins`, `annee`, `trimestre`, `categorie`) |

Le fichier en ligne est régénéré à chaque requête à partir de la reprise Statbel du moment : ces comptes peuvent bouger. Le SHA-256 ci-dessus identifie cette version précise.

### Colonnes

| Colonne | Type | Description |
|---|---|---|
| `code_ins` | texte (5 chiffres) | Code INS/REFNIS de la commune, à lire comme du texte |
| `commune_fr` | texte | Nom français de la commune, en MAJUSCULES |
| `commune_nl` | texte | Nom néerlandais (vide dans 954 lignes, voir les notes) |
| `annee` | entier | Année (2010–2026) |
| `trimestre` | entier | Trimestre (1–4) |
| `periode` | texte | Libellé `Q{trimestre} {annee}`, par ex. `Q2 2010` |
| `categorie` | texte | `maison` ou `appartement` |
| `prix_median_eur` | entier | Prix médian de vente en euros, tel que publié par Statbel |
| `nombre_transactions` | entier | Nombre de transactions derrière la médiane |

### Méthode et remarques

- **Source** : statistiques Statbel des prix de l’immobilier ([page « Prix de l’immobilier »](https://statbel.fgov.be/fr/themes/construction-logement/prix-de-limmobilier), [open data Statbel](https://statbel.fgov.be/fr/open-data)). Parmi les téléchargements de Statbel figure un fichier par trimestre et par commune pour 2010–2026. Les catégories maison/appartement suivent les définitions de Statbel.
- **Méthode Statbel** (résumé de la méthodologie publiée par Statbel) : les prix proviennent des actes de vente enregistrés par le SPF Finances (Administration générale de la Documentation patrimoniale) ; seul le marché secondaire est couvert (les constructions neuves sont exclues) ; les ventes de gré à gré et les ventes publiques sont incluses ; le prix est le prix de vente convenu, hors droits et frais ; les médianes ne sont publiées qu’à partir de 16 transactions avec prix valable. En cohérence, le plus petit `nombre_transactions` de cet instantané est 16.
- **Modifications apportées par Levier** (comme l’exige la licence Statbel) : passage au format long (une ligne = une commune × un trimestre × une catégorie) ; une ligne n’est écrite **que si la médiane Statbel existe**, donc `prix_median_eur` et `nombre_transactions` ne sont jamais vides ; ajout des noms FR et NL depuis le référentiel postal de Levier ; ajout du libellé `periode`.
- **Une médiane absente est une ligne absente**, pas une cellule vide. L’absence de ligne ne veut pas dire zéro vente.
- **`commune_nl` vide** : 954 cellules, pour 27 anciens codes INS dont le nom néerlandais n’est pas dans le référentiel postal de Levier (codes absents de la liste actuelle des communes, par ex. `11007`, `11056`, `23023`, `23024`, `23032`, `37007`, `37015`, `37018`). Les autres colonnes de ces lignes sont remplies. Dans cet instantané, ces 27 codes n’ont aucune ligne après 2023.
- **Fusions de communes** : anciens et nouveaux codes INS peuvent coexister dans le temps. Vérifiez l’historique des codes avant de construire de longues séries pour une commune fusionnée.
- Une médiane n’est pas une moyenne ; elle n’est corrigée ni de l’inflation ni de la taille des biens. Peu de transactions donnent des médianes instables : filtrez sur `nombre_transactions` si besoin.

### Démarrage rapide (Python / pandas)

Voir l’exemple de la section anglaise ci-dessus : `pd.read_csv(url, dtype={"code_ins": str})`, en gardant bien `code_ins` en texte.

### Licence

- **Ce CSV** : Creative Commons Attribution 4.0 International ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr), texte complet dans `LICENSE`). Vous pouvez le copier, le partager, le redistribuer et l’adapter, y compris à des fins commerciales, à condition de citer **« Levier »** avec un lien vers **https://levier.be/**.
- **Données amont** : Statbel conserve sa propre licence ([CC BY 4.0, conditions Statbel](https://statbel.fgov.be/fr/cc-40)) : mentionner « Source : Statbel », lier la licence et indiquer les modifications. Statbel ne cautionne ni Levier ni ce dépôt.

### Citer ce jeu de données

> Levier (2026). *Prix des maisons et appartements par commune en Belgique, T1 2010–T1 2026* [jeu de données, version du 8 octobre 2026]. https://levier.be/ — données : https://levier.be/donnees-ouvertes. Source : Statbel (Direction générale Statistique – Statistics Belgium). Licence CC BY 4.0.

Forme courte pour graphiques et articles : **Source : Levier (https://levier.be/), d’après Statbel, CC BY 4.0.**
