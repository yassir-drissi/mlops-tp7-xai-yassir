# Model Card - fraude (version servie par l'alias production)

## Modele
Nom et version dans le registre MLflow : fraude, version 1 (alias production)
Famille et reglages : RandomForestClassifier, 500 arbres, profondeur 16, scikit-learn 1.9.1
Run d'origine (MLflow) : 2de30b05 (rf-500-16, TP2)

## Objectif et usage prevu
Estimer la probabilite qu'une transaction bancaire soit frauduleuse, pour signaler ou bloquer un paiement en vue d'une revue humaine.

## Usages exclus
Ne pas bloquer definitivement un compte sans revue humaine. Ne pas utiliser sur des donnees reelles sans reevaluation (le modele est entraine sur des donnees synthetiques).

## Donnees
Fichier data/transactions.csv, 5000 lignes, 568 fraudes (11,4 %), empreinte md5 : cf9f6f86c141e7df32c61715c493483f
Origine des donnees (reelles ou synthetiques) : synthetiques, generees avec une graine fixe (TP2)

## Performances (jeu de test, 1 250 transactions)
F1 : 0,8669   precision : 0,8837   rappel : 0,8507   AUC : 0,9872
Matrice de confusion : [[1101, 15], [20, 114]] (114 fraudes detectees sur 134)

## Methodes d'explicabilite
Globale : importance par permutation (10 melanges) et SHAP TreeExplainer sur 400 transactions
Locale : SHAP (waterfall) et LIME (2 000 voisins, graine 0)
Variables les plus influentes : montant (permutation 0,593), freq_24h (0,269), anciennete (0,268), nuit (0,196)

## Comparaison avec un modele interpretable
Logistique avec log10(montant) : F1 0,8593, 113 fraudes sur 134, contre 0,8669 et 114 pour la foret. Ecart dans le bruit du jeu de test (134 fraudes).

## Limites et risques connus
Donnees synthetiques, seulement 4 variables, 134 fraudes en test. LIME est instable : les contributions de anciennete et freq_24h changent de signe selon la graine. SHAP decrit le modele, pas une cause reelle (correlation, pas causalite). L'effet du montant est non lineaire (seuil vers 1 000 MAD).

## Recommandations d'utilisation et de surveillance
Revue humaine avant tout blocage definitif. Surveiller la derive de montant, nuit et freq_24h, et reevaluer F1 et rappel a chaque reentrainement. Communiquer a un client uniquement les causes stables (montant, nuit).

## Tracabilite
Run des explications (experience tp7-explicabilite) : xai-fraude-v1, run 3d2f17fb
Auteurs : Yassir Drissi   Date : 09/11/2026   Prochaine revue : au prochain reentrainement