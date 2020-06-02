[![Build Status](http://54.89.194.154:8080/buildStatus/icon?job=jenkins-ci)](http://54.89.194.154:8080/job/jenkins-ci/)

*************JENKINS-CI***********************
1- Telechargement des pluguins
	* docker-build-step: pour build une image via jenkins
	* docker-compose Build Step: pour executé une image docker 
	* GitHub Integration: recupéré un repository sur github via jenkins
	* Embeddable Build status: pour le badget

2- Creaction des credential
    Definir les identifiant pour ce connecté soit sur docker hub, git ou une connexion sur une machine distant

3- Configuré jenkins afin qu'il communique avec github
Manager Jenkins > configur system > docker builder : unix:///var/run/docker.sock 

4- Crée le job
General : Github project > project url : mettre le lien du github de votre project
Source Code Management > Git
Build Triggers > Github hook trigger for GitScm polling (declanchement du build)
Build > add build step > Execute docker command > create/build/image > chemin du dockerfile et tag

*etape suivant run du l'image
Dans add build step > execute shell
#!/bin/bash
docker-compose up -d
sleep 5s
if [ "$(curl -X GET http://172.18.0.1:80/health)" = "ok" ]; then echo "test OK"; exit 0; else echo "test KO"; exit 1; fi

**Etape suivant poussé
add build step > execute command docker > push image 
	name: nom de limage, tag: nom tag, docker registre URL: https://index.docker.io/v2/ ( est url de docker registrie, si ce un registre prive on le met)
        et en fin mettre les identifiant

5-Le webhooks 
 * administre jenkins > configuration systèm > GitHub: GitHub Servers > advance > coché ==> Specify another hook URL for GitHub configuration puis copie le lien
 * Aller sur votre compte github > settings > webhooks > add webhooks > playload URL : colé le lien
6- Aller sur le project: jenkins_ci > Embeddeble Build Status > Markdown > unprotected (compie le lien et colle dans le README)
