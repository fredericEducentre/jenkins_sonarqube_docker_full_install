# jenkins_sonarqube_docker_full_install


Créer les containers jenkins et sonarqube sous le même réseau :

```
docker compose up
```

Configurer Jenkins puis Sonarqube

Ajouter deux nodes sur Jenkins (agent node & agent php)

Créer des images à partir des dockerfiles :

```
cd agent-nodes
docker build . -t jenkins-agent-node
docker run --init --name jenkins-agent-node jenkins-agent-node -url http://<jenkins-server-IP>:8085 <secret> <agent name>
```

Créer des images à partir des dockerfiles

```
cd agent-php
docker build . -t jenkins-agent-php
docker run --init --name jenkins-agent-php jenkins-agent-php -url http://<jenkins-server-IP>:8085 <secret> <agent name>
```