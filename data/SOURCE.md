# Provenance du jeu de données `student-mat.csv`

**Copie figée le 2026-10-10.** Le fichier est **identique octet pour octet** à celui du ZIP
officiel UCI. Aucune conversion, et le séparateur `;` d'origine est conservé : c'est un
défaut voulu, montré dans la vidéo. Ne pas le régénérer sans raison : la vidéo annoncera des
chiffres exacts qui doivent rester vrais.

| Fichier | SHA-256 |
|---|---|
| `student-mat.csv` (56 993 octets) | `e47f9ee225e1ee6e69b7564e6dac7123e80b8486677fe111f351964cef5dec80` |
| `student.txt` (documentation des colonnes, 3 206 octets) | `f8d3e734e237071312790ca667330c66d75e706317f8d5e2125c479d70e962c1` |

## Source

- **Jeu de données :** Student Performance, volet mathématiques (`student-mat.csv`).
  Le volet portugais (`student-por.csv`) n'est pas utilisé.
- **Source primaire :** UCI Machine Learning Repository
  <https://archive.ics.uci.edu/dataset/320/student+performance>
- **Téléchargé depuis :** <https://archive.ics.uci.edu/static/public/320/student+performance.zip>,
  qui contient `student.zip`, qui contient le CSV.
- **Auteurs :** Paulo Cortez et Alice Silva, deux établissements secondaires portugais
  (Gabriel Pereira « GP » et Mousinho da Silveira « MS »).
- **Article d'origine :** Cortez, P. et Silva, A. (2008), *Using data mining to predict
  secondary school student performance*, FUBUTEC 2008, p. 5-12 (référence complète : spec 10 §3
  et README du dépôt).
- **Licence :** CC BY 4.0. ✅ Vérifiée en ligne le 2026-10-10 sur la page UCI, avec le
  **DOI 10.24432/C5TG7T**. Mis en ligne sur UCI le 26/11/2014.

## ⚠️ Attribution obligatoire

La licence CC BY 4.0 **impose** l'attribution. Comme pour le projet 1, le crédit doit
apparaître dans le README du dépôt, dans la description YouTube et, de préférence, sur une
diapositive.

> **Données :** *Student Performance*, Paulo Cortez, UCI Machine Learning Repository (2008),
> <https://doi.org/10.24432/C5TG7T>.
> **Licence :** Creative Commons Attribution 4.0 International (CC BY 4.0),
> <https://creativecommons.org/licenses/by/4.0/>
> **Modifications apportées :** fichier redistribué sans modification. Dans le notebook :
> création de la variable cible `echec` (`G3 < 10`), suppression de colonnes et encodage
> binaire des variables oui/non.

## Mesures — 2026-10-10, module `csv`, sans pandas

### Structure

- **395 lignes × 33 colonnes**, séparateur `;`. Un `pd.read_csv` sans `sep=";"` renverrait
  une seule colonne.
- 0 case vide, 0 doublon strict.
- `G1` et `G2` sont **entre guillemets** dans le fichier (`"5"`), contrairement à `G3`.
  ☐ Vérifier sur Colab que pandas les lit bien comme des entiers.

### Variable cible : `echec = G3 < 10`

- **130 échecs / 265 réussites**, soit 32,9 % / 67,1 %. Un modèle naïf qui prédit
  « réussite » pour tout le monde a donc un taux de bonnes prédictions de 67,1 %.
- `r(G1, G3)` = 0,801 · `r(G2, G3)` = 0,905 · `r(absences, G3)` = 0,034.

### Les 38 zéros de `G3`

- **38 élèves ont `G3 = 0`.** Aucune note finale n'est comprise entre 1 et 3.
- Pour ces 38 élèves : `G1` va de 4 à 12 (7 ont `G1 ≥ 10`), 13 ont aussi `G2 = 0`, et
  **les 38 ont `absences = 0`**.
- Chez les 357 autres élèves, 21,6 % ont 0 absence. Parmi eux, les échecs (note de 1 à 9)
  ont 9,6 absences en moyenne et 13 % sont à 0 ; les réussites ont 5,2 absences en moyenne.
- **Ce qu'on peut dire :** profil extrêmement atypique sous une hypothèse d'indépendance.
  **Ce qu'on ne peut pas dire :** que ce sont des valeurs manquantes. La documentation UCI
  annonce des notes de 0 à 20 et aucune valeur manquante, sans rien dire de plus.
  L'hypothèse « examen non passé ou abandon » reste plausible mais **non démontrée**.
- Ces 38 élèves représentent **29 % des échecs**.

### Colonnes candidates (2026-10-11 : `schoolsup` et `higher` retirées du modèle, spec 10 §3)

⚠️ Taux calculés sur les 395 lignes : ils décrivent le fichier, mais **ne servent pas** à
choisir les variables, et ne s'annoncent pas dans la vidéo (exploration sur l'apprentissage).

| Colonne | Valeurs | Taux d'échec par modalité |
|---|---|---|
| `school` | GP 349 · MS 46 | GP 32 % · MS 37 % |
| `age` | 15 à 22 (20 ans et plus : 5 élèves) | — |
| `studytime` | 1 : 105 · 2 : 198 · 3 : 65 · 4 : 27 (déjà encodée par tranches) | 35 · 35 · 25 · 26 % |
| `failures` | 0 : 312 · 1 : 50 · 2 : 17 · 3 : 16 | **25 · 52 · 82 · 75 %** |
| `schoolsup` | no 344 · yes 51 | 31 % · 45 % |
| `higher` | no **20** · yes 375 | 65 % · 31 % (groupe « no » très petit) |
| `internet` | no 66 · yes 329 | — |
| `G1` | 3 à 19 | — |

⚠️ **Écart entre la documentation et les données :** `student.txt` décrit `failures` comme
« n si 1 ≤ n < 3, sinon 4 », mais le fichier ne contient aucun 4, et sa valeur maximale est 3.
On signale ce conflit entre documentation et fichier **sans l'interpréter** (« 3 ou plus »
reste une hypothèse, relecture de l'assistant du 2026-10-11).

### Règle simple « échec au 1er trimestre → échec final » (`G1 < 10`)

Sur les 395 élèves : 142 ont `G1 < 10`, dont 104 échouent au final. Sur les 253 autres,
26 échouent quand même. La règle donne donc **≈ 83,8 % de bonnes prédictions**, un rappel
de l'échec de 80 % (104/130) et une précision de 73 % (104/142).
⚠️ Mesuré sur **toutes** les lignes. À remesurer sur l'ensemble de test, comme le modèle.

## Chiffres du modèle — à mesurer sur Colab, aucun pour l'instant

Indications de l'expert externe (sa propre validation croisée avec 9 variables, `absences`
comprise, donc **hors protocole**) : environ 86 % de bonnes prédictions, précision de
l'échec 81 %, rappel de l'échec 75 %. **Rien de tout cela ne s'annonce dans la vidéo.**
Seules comptent les sorties Colab du protocole figé dans la spec 10, §5.
