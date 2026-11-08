# Réflexion — TP2

1. Quel est le meilleur des 3 modèles ? Peut-on le prouver ?
-> Oui — git log montre les 3 commits distincts, chacun avec son
message explicite

2. Peut-on redonner exactement le modèle de l'Essai 1 ?
-> Oui — git checkout <commit> -- churn_model.ipynb

3. Un camarade peut-il reproduire notre travail sur une autre machine ?
-> Oui — git clone donne une copie identique du code et des


# Réflexion — TP3

Question 1 : Quel est le meilleur modèle ? Peux-tu le prouver ?
Réponse : Oui, sur la base d’un critère traçable. Les runs sont classés par f1 dans MLflow, avec résultat daté et conservé. En revanche, ce n’est pas une preuve absolue : le test ne contient que 5 cas positifs, donc un seul client peut changer le classement.

Question 2 : Peux-tu redonner exactement le modèle de l’Essai 1 ?
Réponse : Oui. mlflow.sklearn.load_model("runs:/<id>/model") recharge le modèle exact d’un run précis, contrairement à model.pkl qui avait été écrasé.

Question 3 : Un camarade peut-il reproduire ton travail sur une autre machine ?
Réponse : En partie. Le code (Git), les paramètres et les versions de bibliothèques (requirements.txt du run) sont connus. Restent l’environnement identique (Docker, TP5-6), les données (churn_data.csv ou generate_data.py) et le dossier mlruns/, absent de Git.