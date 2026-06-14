# Traitement Big Data avec HDFS et Apache Spark

Ce dépôt contient les fichiers de configuration et les travaux pratiques permettant de déployer un environnement de traitement de données massives basé sur Hadoop HDFS et Apache Spark. La plateforme est entièrement conteneurisée avec Docker Compose et fournit un écosystème complet pour le stockage distribué, le traitement de données et l'analyse à grande échelle.

---

## Architecture du Système

L'environnement est composé de plusieurs conteneurs Docker organisés en deux couches principales.

### Couche de Stockage (HDFS)

**NameNode**

* Gère l’espace de noms du système de fichiers.
* Stocke les métadonnées des fichiers et des blocs.
* Coordonne toutes les opérations sur les fichiers.

**DataNode**

* Stocke physiquement les blocs de données.
* Gère les opérations de lecture et d’écriture.
* Transmet les informations des blocs au NameNode.

### Couche de Traitement (Apache Spark)

**Spark Master**

* Gère les ressources du cluster.
* Planifie et distribue les tâches de calcul.

**Spark Workers**

* Exécutent les tâches Spark en parallèle.
* Réalisent les calculs distribués.

**Spark History Server**

* Conserve les journaux des applications terminées.
* Fournit des informations sur les performances et l'exécution des traitements.

### Environnement de Développement

**PySpark Jupyter Notebook**

* Environnement interactif pour l’exploration et le développement de solutions Big Data.
* Intègre Python, PySpark et les bibliothèques courantes de science des données.

---

## Services Disponibles

| Service              | Port Hôte | Description                                 |
| -------------------- | --------- | ------------------------------------------- |
| Jupyter Lab          | 8888      | Environnement de développement interactif   |
| HDFS NameNode        | 9870      | Surveillance et exploration du système HDFS |
| Spark Master         | 8080      | Gestionnaire du cluster Spark               |
| Spark Worker 1       | 8081      | Surveillance du premier nœud de calcul      |
| Spark Worker 2       | 8082      | Surveillance du second nœud de calcul       |
| Spark History Server | 18081     | Historique des applications Spark           |

---

## Prérequis

Avant de démarrer le projet, assurez-vous d'avoir installé :

* Docker
* Docker Compose
* Git

Configuration matérielle recommandée :

* Minimum 8 Go de RAM
* 4 cœurs CPU
* 15 Go d’espace disque disponible

---

## Déploiement du Cluster

### Démarrage de l’Environnement

```bash
docker-compose up -d
```

### Vérification des Conteneurs

```bash
docker ps
```

### Arrêt de l’Environnement

```bash
docker-compose down
```

---

## Accès aux Services

Après le déploiement, les interfaces suivantes sont accessibles :

| Interface            | URL                    |
| -------------------- | ---------------------- |
| Jupyter Lab          | http://localhost:8888  |
| HDFS NameNode        | http://localhost:9870  |
| Spark Master         | http://localhost:8080  |
| Spark History Server | http://localhost:18081 |

---

## Travail Pratique 1 : Manipulation de HDFS

Ce premier TP est consacré à la découverte du système de fichiers distribué Hadoop (HDFS).

### Objectifs

* Créer des répertoires dans HDFS.
* Importer des jeux de données dans le système distribué.
* Explorer les fichiers à l’aide des commandes HDFS.
* Analyser la distribution physique des blocs de données.

### Principales Commandes

Création d’un répertoire :

```bash
hdfs dfs -mkdir /data
```

Importation d’un fichier :

```bash
hdfs dfs -put dataset.csv /data
```

Liste des fichiers :

```bash
hdfs dfs -ls /data
```

Affichage du contenu :

```bash
hdfs dfs -cat /data/dataset.csv
```

Inspection des blocs :

```bash
hdfs fsck /data/dataset.csv -files -blocks -locations
```

### Compétences Acquises

* Compréhension du stockage distribué.
* Découverte de l’architecture HDFS.
* Analyse du découpage des fichiers en blocs.
* Étude du mécanisme de réplication des données.

---

## Travail Pratique 2 : Traitement Distribué avec Apache Spark

Ce second TP introduit le traitement distribué des données à l’aide des DataFrames Spark et de Spark SQL.

### Chargement des Données

Le jeu de données est chargé depuis HDFS dans un DataFrame Spark.

```python
df = spark.read.csv(
    "hdfs://namenode:9000/data/dataset.csv",
    header=True,
    inferSchema=True
)
```

### Nettoyage des Données

Les opérations de prétraitement comprennent :

* Suppression des valeurs manquantes.
* Filtrage des enregistrements invalides.
* Correction des incohérences éventuelles.

### Analyses Réalisées

Le notebook effectue plusieurs analyses :

* Statistiques descriptives.
* Agrégations par catégorie.
* Analyses temporelles.
* Études de corrélation.
* Évaluation des performances.

### Utilisation de Spark SQL

Une vue temporaire est créée afin d’exécuter des requêtes SQL directement sur les données distribuées.

```python
df.createOrReplaceTempView("dataset")
```

Exemple de requête :

```sql
SELECT category,
       COUNT(*) AS total_records
FROM dataset
GROUP BY category;
```

### Visualisation des Résultats

Les données agrégées sont converties en DataFrame Pandas afin de produire :

* Des diagrammes en barres.
* Des courbes temporelles.
* Des graphiques de dispersion.
* Des histogrammes de distribution.

### Exportation des Données

Les données finales sont enregistrées au format Parquet.

```python
df.write.mode("overwrite").parquet(
    "/data/output/processed_dataset.parquet"
)
```

---

## Résultats Attendus

À l’issue de ce projet, l’étudiant doit être capable de :

* Déployer un cluster Hadoop et Spark.
* Manipuler les données dans HDFS.
* Traiter des volumes importants de données avec Spark.
* Réaliser des analyses distribuées.
* Exécuter des requêtes SQL sur des données massives.
* Exporter des données optimisées au format Parquet.
* Superviser les performances du cluster via les interfaces web.

---

## Auteur

Laboratoire Big Data et Systèmes Distribués

Projet Académique : Hadoop HDFS et Apache Spark
