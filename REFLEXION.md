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


## TP4

### 1. Comment désigner officiellement le modèle à utiliser ?

On enregistre le modèle dans le Registry de MLflow sous le nom `churn-model`,
puis on place l'alias `champion` sur la version choisie. L'adresse
`models:/churn-model@champion` désigne alors le modèle officiel, sans qu'on ait
à connaître le numéro de version ni l'identifiant du run. Changer de modèle
officiel se fait en une ligne : on déplace l'alias.

### 2. Comment une application peut-elle utiliser le modèle ?

Elle n'a pas besoin de connaître le modèle, ni Python, ni scikit-learn : elle
envoie une requête web à l'API. Avec `POST /predict`, elle transmet les
informations d'un client en JSON et reçoit en réponse la prédiction
(`churn: 0` ou `1`). Si les données sont invalides, l'API répond par une
erreur 422 au lieu de planter.

### 3. Que se passe-t-il quand on change de champion ?

On déplace l'alias vers une autre version, puis on redémarre l'API, qui charge
le modèle une seule fois au démarrage. Le code de l'API et celui des
applications clientes ne changent pas : seul le pointeur bouge. Aucune version
n'est supprimée, donc un retour en arrière (rollback) reste possible en
remettant l'alias sur l'ancienne version.