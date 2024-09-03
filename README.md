# jenkins_sonarqube_docker_full_install

Si vous êtes sur linux commencer par :
```
sudo sysctl -w vm.max_map_count=262144
```

Créer les containers jenkins et sonarqube sous le même réseau :

```
docker compose up -d
```

Configurer Jenkins

Aller sur http://localhost:8085/

```
docker logs jenkins_container
```

Configurer SonarQube

Aller sur http://localhost:9000/

Ajouter deux nodes sur Jenkins (agent_node & agent_php)

```
http://localhost:8085/manage/computer/
```

Créer une image d'un agent jenkins disposant de nodeJS à partir du dockerfile :

```
cd agent-node
docker build . -t jenkins-agent-node
docker run --init --name jenkins_agent_node_container --network devops -v /var/run/docker.sock:/var/run/docker.sock jenkins-agent-node -url http://jenkins_container:8080 <secret> <agent name>
```

ou utiliser une image existante :

```
docker run -d --network devops --name jenkins_agent_node_container -v /var/run/docker.sock:/var/run/docker.sock --init fredericeducentre/jenkins_agent_node -url http://jenkins_container:8080 <secret> agent_node
```

Créer une image d'un agent jenkins disposant de php à partir du dockerfile :

```
cd ../agent-php
docker build . -t jenkins-agent-php
docker run --init --name jenkins_agent_php_container --network devops jenkins-agent-php -url http://jenkins_container:8080 <secret> agent_php
```
