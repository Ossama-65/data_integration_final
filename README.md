Food Inspection Violation
Description
Ce projet consiste à analyser les données d'inspection alimentaire en utilisant Kafka, HDFS, Spark et SQLite. Il suit un pipeline de traitement complet, incluant la collecte de données, leur stockage, leur nettoyage, le calcul des métriques et leur versionnement.

Le projet est conçu pour montrer comment traiter de grandes quantités de données en intégrant plusieurs technologies et en assurant une gestion des données robuste.

Architecture
Kafka : Utilisé pour la gestion des flux de données (producer/consumer) à partir du fichier Food_Facility_RestaurantInspections.csv.
HDFS : Stockage distribué pour les fichiers CSV Geocoded_FoodFacilities.csv et Food_Facility_RestaurantInspections_split.csv.
SQLite : Base de données relationnelle légère pour stocker les données traitées.
Spark : Pour la transformation et le traitement des données, notamment la jointure et le nettoyage des données.
Versionnement : Utilisation d'un script pour archiver les résultats finaux.
Fonctionnalités
Création de topics Kafka pour gérer les données en temps réel.
Transformation des données avec Spark et stockage dans HDFS.
Nettoyage et jointure des données pour produire des résultats cohérents.
Calcul des métriques analytiques.
Versionnement des données finales pour traçabilité et reproductibilité.
Prérequis
Outils nécessaires :

Python 3.9+
Docker et Docker Compose
Spark
Hadoop (HDFS)
SQLite
Installer les dépendances :

bash
Copier le code
pip install -r requirements.txt
Démarrer Docker Compose (pour Kafka et HDFS) :

bash
Copier le code
docker-compose up -d
Structure du projet
plaintext
Copier le code
├── bdd/
│   ├── setup_db.py          # Configuration initiale de la base SQLite
├── config/
│   ├── kafka_config.json    # Configuration des topics Kafka
├── data/
│   ├── Food_Facility_RestaurantInspections.csv
│   ├── FoodFacilityRestaurantInspectionViolations.csv
│   ├── Geocoded_FoodFacilities.csv
│   ├── clean_joined_data.py # Script pour nettoyer les données après la jointure
├── hdfs/
│   ├── hdfs_to_sqlite.py    # Script pour envoyer les données HDFS dans SQLite
│   ├── joinall.py           # Script pour effectuer la jointure finale
│   ├── read_geocoded_facilities.py
│   ├── read_inspections.py
├── kafka_scripts/
│   ├── create_topic.py      # Création de topic Kafka
│   ├── producer.py          # Envoi des données vers Kafka
│   ├── consumer.py          # Lecture des données depuis Kafka
├── metrics/
│   ├── calculate_metrics.py # Calcul des métriques analytiques
├── versionning/
│   ├── save_version.sh      # Script pour archiver les résultats finaux
│   ├── version_XXXXXX.zip   # Archive des résultats versionnés
├── myenv/                   # Environnement virtuel Python
├── requirements.txt         # Liste des dépendances
├── docker-compose.yml       # Configuration Docker pour Kafka et HDFS
├── README.md                # Documentation du projet
Étapes pour Exécuter le Projet
1. Initialiser Kafka et HDFS
Démarrer Kafka et HDFS avec Docker Compose :
bash
Copier le code
docker-compose up -d
2. Créer un Topic Kafka
Exécuter le script create_topic.py pour créer un topic :
bash
Copier le code
python kafka_scripts/create_topic.py
3. Producer et Consumer Kafka
Envoyer les données vers Kafka :
bash
Copier le code
python kafka_scripts/producer.py
Consommer les données depuis Kafka :
bash
Copier le code
python kafka_scripts/consumer.py
4. Stocker les Fichiers dans HDFS
Copier les fichiers CSV dans HDFS :
bash
Copier le code
hdfs dfs -put data/Geocoded_FoodFacilities.csv /data
hdfs dfs -put data/Food_Facility_RestaurantInspections_split.csv /data
5. Traiter les Données avec Spark
Nettoyer et joindre les données :
bash
Copier le code
python hdfs/joinall.py
python data/clean_joined_data.py
6. Calculer les Métriques
Exécuter le script de calcul des métriques :
bash
Copier le code
python metrics/calculate_metrics.py
7. Versionner les Données Finales
Archiver les résultats dans un fichier ZIP :
bash
Copier le code
bash versionning/save_version.sh
Résultats
Tables SQLite : Données finales nettoyées et enrichies accessibles dans kafka_data.db.
Métriques Analytiques : Résultats détaillés des inspections et violations alimentaires.
Archives Versionnées : Fichiers CSV finaux stockés dans le dossier versionning/.
Problèmes Courants et Solutions
Problème avec MarkupSafe ou Werkzeug :

Rétrograder vers des versions compatibles :
bash
Copier le code
pip install werkzeug==2.0.3 markupsafe==2.0.1
Connexion refusée avec HDFS :

Vérifier la configuration réseau et remplacer localhost par l'IP du conteneur dans core-site.xml.
Kafka ne démarre pas :

Redémarrer Docker Compose :
bash
Copier le code
docker-compose down && docker-compose up -d
Auteurs
Ossama : Développeur principal du projet.
Licence
Ce projet est sous licence MIT.
