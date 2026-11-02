# 🚗 Audit de Données, Imputation Statistique & Tarification Non-Vie (Excel Avancé)

Projet d'analyse, de fiabilisation de données d'assurance automobile et de construction des bases tarifaires (Coût Moyen & Fréquence), réalisé dans le cadre du Master 1 MAS (parcours IMSA) à Aix-Marseille Université.

---

## 🎯 Problématique & Objectifs Métier
Dans le processus de tarification actuarielle en assurance non-vie, la qualité des bases d'apprentissage est un enjeu critique. Ce projet vise à :
1. **Auditer et assainir un portefeuille** de 1 000 contrats auto (`Base_ass`) et 653 sinistres matériels (`Base_sin`).
2. **Détecter et corriger les valeurs aberrantes et manquantes** (erreurs de saisie, anciennetés négatives, chaînes parasites, incohérences d'âge d'obtention du permis).
3. **Construire les deux bases d'études actuarielles** :
   * **Base Coût Moyen (CM) :** Vue par sinistre enrichie des caractéristiques du véhicule et de l'assuré.
   * **Base Fréquence (FQ) :** Vue par contrat avec calcul de l'exposition et comptage du nombre de sinistres ($N \ge 0$).

---

## 🔍 Audit & Traitement des Anomalies (Data Cleaning)
* **Variable Sexe :** Détection de modalités manquantes (lignes vides) et traitement par imputation ou suppression ciblée.
* **Type de véhicule :** Traitement des 10 valeurs manquantes (imputation proportionnelle aux fréquences empiriques).
* **Ancienneté de permis (`Anc.permis`) :**
  * Nettoyage des formats hétérogènes (textes parasites `TOTO`, `Z`, `JE SAIS PAS`, valeurs négatives `-2`, `-3` et aberrantes `123`).
  * Détection d'incohérence multivariée : $\text{Âge} - \text{Ancienneté} < 18\text{ ans}$ (obtention du permis avant la majorité).
  * **Imputation statistique par régression linéaire :**
    $$\text{Anc.permis} = a \times \text{Âge} + b$$
    Modélisation sur l'échantillon sain ($R^2 > 0,98$) pour imputer les valeurs manquantes et aberrantes de manière économétrique.

---

## 🏗️ Construction des Bases d'Études Actuarielles
1. **Base Coût Moyen (`Base_CM` - 652 sinistres rattachés) :**
   * Jointure relationnelle (`RECHERCHEV` / `INDEX-EQUIV` ou jointure interne) entre la table des sinistres et les polices assainies.
   * Détection et exclusion d'un sinistre orphelin (police non répertoriée dans le portefeuille actif).
   * Analyse du coût moyen par zone géographique et typologie de véhicule.
2. **Base Fréquence (`Base_FQ` - 1 000 assurés) :**
   * Agrégation par contrat du nombre de sinistres déclarés (`NB.SI.ENS`).
   * Réconciliation des contrats sans sinistre ($N = 0$) pour respecter la réalité de l'exposition au risque.
   * Calcul de la fréquence de sinistralité par classe d'âge et d'ancienneté.

---

## 🛠️ Outils & Formules Excel Mobilisés
* **Formules de recherche & logiques :** `RECHERCHEV`, `INDEX`, `EQUIV`, `SI`, `ESTNA`, `ET`, `OU`, `COALESCE`.
* **Agrégations & Statistiques conditionnelles :** `SOMME.SI.ENS`, `NB.SI.ENS`, `MOYENNE.SI.ENS`.
* **Modélisation & Tendance :** Fonctions `PENTE`, `ORDONNEE.ORIGINE`, `COEFFICIENT.CORRELATION`, régression linéaire via nuage de points.
* **Restitution décisionnelle :** Tableaux Croisés Dynamiques (TCD) de fréquence/sévérité et graphiques de distribution univariée.

---

## 📁 Contenu du dépôt
* `TP1 Excel Corrigé.xlsx` : Classeur Excel complet et dynamique (onglets d'audit, TCD, bases nettoyées `Base_ass_clean`, `Base_CM` et `Base_FQ`).
* `Enoncé Actuariat 1 - TP 1 (Excel).pdf` : Énoncé du cas d'étude actuariel.
