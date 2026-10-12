# Machine Learning en Python : projet de classification complet

Code et données de la vidéo YouTube **« Machine Learning en Python : projet de
classification complet (débutant, avec code) »**.

On y repère, dès la fin du premier trimestre, les élèves qui risquent d'échouer en
mathématiques en fin d'année (note finale sous 10/20). On utilise une régression
logistique, de bout en bout : chargement, nettoyage, choix des variables selon la date de
prédiction, trois ensembles (apprentissage, validation, test), réglage du seuil et
comparaison avec une règle simple fondée sur la note du 1er trimestre.

## Lancer le notebook : rien à installer

[![Ouvrir dans Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/formation-ia-data/classification-eleves-python/blob/main/projet_classification_ml.ipynb)

Cliquez sur le bouton : le notebook s'ouvre dans **Google Colab**, directement dans le
navigateur. Il suffit d'un compte Google. Ni Python ni Anaconda à installer.

Exécutez ensuite les cellules une à une, de haut en bas (`Maj + Entrée`).

## Contenu

| Fichier | Rôle |
|---|---|
| `projet_classification_ml.ipynb` | Le notebook de la vidéo |
| `data/student-mat.csv` | Le jeu de données : 395 élèves, 33 colonnes, séparateur `;` |

Le notebook lit le CSV depuis le tag **`v1`** de ce dépôt. Le fichier servi reste donc
identique à celui de la vidéo, même si le dépôt évolue.

## ⚠️ Limites du modèle

Les données portent sur **395 élèves de deux lycées publics portugais**, en
mathématiques, pendant l'année scolaire **2005-2006**. Le modèle décrit des
**associations** dans cet échantillon. Il n'établit aucune cause, et rien ne dit qu'il
vaut ailleurs sans vérification.

C'est un **signal** qu'un enseignant examinerait pour proposer un soutien, pas une
décision. Les variables personnelles et familiales (sexe, métier des parents, etc.) sont
volontairement écartées, mais cela ne rend pas le modèle neutre : l'établissement ou les
échecs passés peuvent servir d'indicateurs indirects et reproduire des inégalités
présentes dans les données.

## Données et licence

**Données :** *Student Performance*, UCI Machine Learning Repository —
Cortez, P. (2008). Student Performance [Dataset]. <https://doi.org/10.24432/C5TG7T>

**Article d'origine :** Cortez, P. et Silva, A. (2008). *Using Data Mining to Predict
Secondary School Student Performance*. In A. Brito et J. Teixeira (dir.), Proceedings of
the 5th FUture BUsiness TEChnology Conference (FUBUTEC 2008), p. 5-12, Porto, EUROSIS,
ISBN 978-9077381-39-7.

**Licence des données :** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

**Modifications :** le fichier `data/student-mat.csv` est redistribué sans modification
(identique à celui de l'archive UCI). Le notebook crée la variable cible `echec`
(`G3 < 10`), ne garde que certaines colonnes et encode en 0/1 les variables à deux
modalités. Une analyse de sensibilité refait le calcul sans les 38 élèves dont la note
finale vaut 0 ; le fichier, lui, reste complet.
