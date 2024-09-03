# jenkins_sonarqube_docker_full_install

Si vous êtes sur linux commencez par :
```
sudo sysctl -w vm.max_map_count=262144
```

## 1. Créer les containers Jenkins et Sonarqube sous le même réseau

```
docker compose up -d
```

a. Configurer Jenkins

Aller sur http://localhost:8085/

```
docker logs jenkins_container
```

b. Configurer SonarQube

Aller sur http://localhost:9000/

## 2. Ajouter des agents sur Jenkins :

```
http://localhost:8085/manage/computer/
```

En fonction de votre projet, installez l'agent nécessaire.

### Agent nodeJS

Créer une image d'un agent jenkins disposant de nodeJS à partir du dockerfile :

```
cd agent-node
docker build . -t jenkins-agent-node
docker run --init --name jenkins_agent_node_container --network devops -v /var/run/docker.sock:/var/run/docker.sock jenkins-agent-node -url http://jenkins_container:8080 <secret> <agent name>
```

Ou utiliser une image existante (plus rapide) :

```
docker run -d --network devops --name jenkins_agent_node_container -v /var/run/docker.sock:/var/run/docker.sock --init fredericeducentre/jenkins_agent_node -url http://jenkins_container:8080 <secret> agent_node
```

Voici une aide pour mettre en place l'intégration continue (continous integration) et la livraison continu (continous delivery) sur un projet utilisant nodeJS :

De façon général pour créer une pipeline de CI-CD répondez d'abord à ces questions :
- Est-ce que mon code est sur un dépot de code (Github/Gitlab/...) ?
- Est-ce que mon code a besoin d'être compilé ?
- Est-ce que mon code a des tests unitaires ?
- Quels sont les langages (RUBY/JAVA/JS/PHP/...) utilisés dans mon projet ?
- Est-ce que mon serveur/hébergeur dispose de Docker ?
- Est-ce que je souhaite containeriser mon code ? Si oui, ai-je un dockerfile ?
- Est-ce que je souhaite par la suite automatiser le déploiement ?

Prenez ce projet exemple React disposant d'un Jenkinsfile : https://github.com/fredericEducentre/zodiacJS (pensez à personnaliser l'étape delivery avec votre jeton d'accès personnel (PAT) docker hub)

Créer un projet sur Sonarqube - http://localhost:9000/projects/create?mode=manual
Créer une pipeline sur Jenkins - http://localhost:8085/view/all/newJob
Une fois que la CI-CD à réussi, déployez sur un server l'image avec Docker
Exemple :
```
docker run --name zodiac_js_container -p 3000:80 fredericeducentre/zodiac_js
```
Allez sur http://localhost:3000

### Agent php

Créer une image d'un agent jenkins disposant de php à partir du dockerfile :

```
cd ../agent-php
docker build . -t jenkins-agent-php
docker run --init --name jenkins_agent_php_container --network devops jenkins-agent-php -url http://jenkins_container:8080 <secret> agent_php
```
