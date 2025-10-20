RefactoSphere : Dashboard d'Analyse de Code Assisté par IA

RefactoSphere est une application web full-stack conçue pour analyser la qualité du code de projets existants (Java, Python, JS) et fournir des suggestions de refactoring intelligentes en utilisant l'IA.

Ce projet a été développé en auto-formation pour maîtriser l'architecture microservices, l'intégration d'IA dans le cycle de développement, et les technologies spécifiques requises pour le stage "Aide au développement avec l'IA" (Java, Python, JS/TS, Elasticsearch).

🚀 Fonctionnalités Clés

    Upload de Projet : Analysez n'importe quel projet en uploadant un fichier .zip.

    Analyse Statique Multi-Langage : Utilise les meilleurs outils de l'industrie (Checkstyle, Pylint, ESLint) pour détecter les "code smells".

    Suggestions de Refactoring par IA : Intègre un modèle de langage (LLM) pour fournir des explications et des exemples de code corrigé pour chaque problème identifié.

    Dashboard de Métriques : Visualisez la "santé" de votre projet avec des graphiques interactifs (complexité, duplication, répartition des problèmes).

    Recherche Plein Texte : Tous les rapports sont indexés dans Elasticsearch, permettant une recherche rapide et puissante sur l'ensemble des analyses.

🛠️ Architecture Technique

Ce projet est construit sur une architecture microservices pour séparer clairement les responsabilités :

    Frontend (UI) - React (Typescript)

        Gère toute l'interaction utilisateur.

        Communique uniquement avec le Backend Java.

        Utilise Chart.js pour la data-visualisation.

    Backend (API Principale) - Java (Spring Boot)

        Sert de chef d'orchestre.

        Expose une API REST pour le frontend.

        Gère l'upload et la décompression des projets.

        Appelle le service Python pour lancer les analyses.

        Prend les résultats et les indexe dans Elasticsearch.

    Service d'Analyse (IA) - Python (FastAPI)

        Le "cerveau" de l'application.

        Expose une API interne (appelée par le service Java).

        Lance les outils d'analyse statique (Checkstyle, Pylint...).

        Interroge une API d'IA (ex: OpenAI) pour générer les suggestions de refactoring.

    Base de Données - Elasticsearch

        Stocke tous les résultats d'analyse (projets, fichiers, problèmes, suggestions IA) sous forme de documents JSON.

        Permet des requêtes complexes et des agrégations pour alimenter le dashboard.

Diagramme de flux

Utilisateur -> [React Frontend] -(Upload .zip)-> [Java Backend]
                                                      |
                                                      v
                                  [Service Python IA] <-(Demande d'analyse)-
                                          |
                                          v
                                    (Analyse + IA)
                                          |
                                          v
      [Elasticsearch] <-(Indexation des résultats)- [Java Backend]
            ^
            |
(Requête rapport)
            |
[React Frontend] <-(Données du rapport)- [Java Backend]

⚙️ Installation et Démarrage

(Note : Tous les services peuvent être lancés en parallèle)

Prérequis

    Java 17+ (JDK)

    Python 3.10+

    Node.js 18+

    Docker (pour Elasticsearch)

    Une clé d'API OpenAI (ou un modèle local configuré)

1. Lancer Elasticsearch

Le moyen le plus simple est d'utiliser Docker :
Bash

docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:8.x.x

2. Lancer le Service d'Analyse (Python)

Bash

cd refactosphere-python-ai
pip install -r requirements.txt
# (N'oubliez pas de créer un .env avec votre clé API_IA)
uvicorn main:app --reload

3. Lancer le Backend (Java)

Bash

cd refactosphere-java-backend
./mvnw spring-boot:run

4. Lancer le Frontend (React)

Bash

cd refactosphere-react-ui
npm install
npm start

Ouvrez http://localhost:3000 dans votre navigateur.
