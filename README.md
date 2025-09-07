# 🗣️💾  Interface d’interrogation SQL en langage naturel (ICV → SQL)

**Objectif.** Construire un système qui comprend des requêtes en **langage naturel (FR)** et les **traduit en SQL** pour interroger une base de films : extraction **Intention–Concepts–Valeurs (ICV)**, **prédiction d’intentions** (*SELECT* / *WHERE*), **génération** puis **exécution** de la requête SQL. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}

---

## 📌 Vue d’ensemble du système

- **Lexique automatique** extrait depuis `base_films_500.csv` (acteurs, réalisateur, genre, année, titre) pour reconnaître les **valeurs** des **concepts**. :contentReference[oaicite:2]{index=2}  
- **Analyse lexicale** : détection (concept, valeur) dans la requête ; gestion multi-acteurs et années → `BETWEEN` si deux années détectées. :contentReference[oaicite:3]{index=3}  
- **Classification des intentions**
  - **SELECT** : pipeline `CountVectorizer + Perceptron` pour prédire les colonnes à retourner. :contentReference[oaicite:4]{index=4}
  - **WHERE** : **règles** (acteur(s), genre, réalisateur, année, `BETWEEN`). :contentReference[oaicite:5]{index=5}  
- **Génération & exécution SQL** via **SQLite** (base en mémoire) ; restitution des résultats. :contentReference[oaicite:6]{index=6}

---

## 🧪 Résultats (évaluations)

### Quantitatif (corpus fournis)
- **SQL générée vs SQL de référence** (normalisée avant comparaison) : **4809 / 6762** → **71,12 %**. :contentReference[oaicite:7]{index=7}  
- **Exactitude du résultat (query→value)** sur `queries_french_para_eval_values.json` : **1691 / 1695** → **99,76 %**. :contentReference[oaicite:8]{index=8}  
- Consignes d’évaluation et liens des corpus d’éval : voir énoncé. :contentReference[oaicite:9]{index=9}

### Qualitatif (20 requêtes réelles)
- Protocole demandé : 20 requêtes, notation {0,1,3}, comparaison avec le quantitatif + analyse d’erreurs. :contentReference[oaicite:10]{index=10}

**Conclusion.** Le système est robuste sur requêtes bien formées (forte exactitude des réponses), plus sensible aux fautes/variations libres ; pistes : correction orthographique légère, règle `titre LIKE '%mot%'`, amélioration des cas abstraits. :contentReference[oaicite:11]{index=11}

---

## 🗂️ Données & corpus

- **Base films** : `base_films_500.csv` (source indiquée dans l’énoncé). :contentReference[oaicite:12]{index=12}  
- **Apprentissage / paraphrases** : `queries_french_para.json` (ICV, FR + paraphrases). :contentReference[oaicite:13]{index=13}  
- **Évaluation (inclus dans ce dépôt)**  
  - `data/queries_french_para_eval.json` — requêtes FR + **SQL de référence**. :contentReference[oaicite:14]{index=14}  
  - `data/queries_french_para_eval_values.json` — **paires (query → value)** pour évaluer la **réponse**, pas seulement la SQL. (inclus)  

> Les chemins vers `base_films_500.csv` et le corpus d’apprentissage sont paramétrables dans le notebook/script.



