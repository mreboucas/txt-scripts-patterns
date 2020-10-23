TIME-LINE
maven      docker   docker
PACKAGE -> BUILD -> RUN

--Package maven application
mvn package

--Build docker application (-t tag) -
sudo docker build -t NAME_APP .

--Obrigatório explicitar a porta
--Run docker non background - with port tunneling
sudo docker run -p 1234:8080 NAME_APP

--Run docker in background - with port tunneling
#d -> run backgroud (deamon)
sudo docker run -p 1234:8080 -d NAME_APP_IMAGE

--View docker containers
sudo docker ps

--View docker containers (todos -a)
sudo docker ps -a

--Stop docker containers
sudo docker stop ID_PROCESS

--View images docker
docker images
or
docker image ls

--View images docker (to force: -f)
docker image rm IMAGE_ID

--Check is docker is up (active)
systemctl is-active docker


##Register local server docker images
Colocar {"insecure-registries" : ["172.25.100.75:3000"]} no arquivo abaixo
sudo gedit /etc/docker/daemon.json

systemctl restart docker
sudo systemctl restart docker

sudo docker login 172.25.100.75:3000

após login, as credencias ficam salvas em: home/marceloreboucas/.docker/config.json

#Restart container ID
docker restart MY_CONTAINER_ID's
#link: https://docs.docker.com/engine/reference/commandline/restart/
Exemple:
reiniciei o kafka assim:
docker ps
docker restart ID_KAFKA ID_ZOKEEPER

#Restart docker compose
docker-compose restart ID_SERVICE (não deu certo com esse)

#Up and Down docker-compose
$ docker-compose up -d
$ ./run_tests
$ docker-compose down

#Send a tag to server
docker tag 'IMAGE_ID' 172.25.100.75:3000/PROJECT_NAME/MICROSERVICE_NAME

Ex.:
docker tag 'TAG_ID' 172.25.100.75:3000/sgc/procurados

#Pull app from server
#docker pull SERVER_LINK/PROJECT_NAME/MICROSERVICE_NAME

#Push app to server
#docker push SERVER_LINK/PROJECT_NAME/MICROSERVICE_NAME

Ex.:
docker pull 172.25.100.75:3000/sgc/procurados

#Download portainer dockerhubdock
docker run -p 9000:9000 portainer/portainer

#Delete all containers (preserva as imagens)
docker container prune

#Inspect secret docker image
sudo docker exec IMAGE_ID cat "PATH_SECRET"

Ex.:
sudo docker exec be90e6b7a174 cat /run/secrets/secret-db-sip
sudo docker exec be90e6b7a174 cat /run/secrets/secret-db-mongo-pca

#Exec sh para banco de dados
docker exec -it [nome do container com o banco de dados] sh


##############################
--DOCKER HUB
##############################
--Baixar images from docker hub
--name: IMAGE NAME
--e: ENVIROMENT VARIABLE
-d: run in baground
docker run --name NAME_IMAGE -e POSTGRES_PASSWORD=root -d postgres

#Ex Postgressql.:
docker run --name postgressql -e POSTGRES_PASSWORD=root -d postgres

#Ex. Portianer
docker run -p 9000:9000 portainer/portainer

#Ex.: spring eureka
docker pull springcloud/eureka

Ex to run docker:
docker run -d -p 8761:8761 springcloud/eureka

Access local host:
http://localhost:8761/


-----------------------------
--GENERAL CONCEPTS/COMANDS - Youtube
-----------------------------
Ao executar o docer run, se a imagem não tiver disponível, o docker vai no dockerhub baixar e depos executar.
docker run -ti
-t: terminal (console)
-i (iteratividade)
-d: deamon (background)

> voltar para o container
docker attach ID_IMAGEM 

> start/stop/pause/unpause ps:
docker start ID_IMAGE
docker stop ID_IMAGE
docker pause ID_IMAGE
docker unpause ID_IMAGE

> Checar o consumo do container
docker stats ID_IMAGEM

> Checar os processos que o container está usand
odocker top ID_IMAGEM

> Checar os logs que o container está usando
docker logs ID_IMAGEM

> Remover container
docker rm ID_IMAGEM
 -> Se tiver em uso (-f):
	docker rm -f ID_IMAGEM

> Remover images
q: images_id
docker rmi $(docker images -a -q | grep -i 'PATTERN')

> Retornar apenas o id do container que já foi inicializado, mas que está parado
docker ps -aq

> Retornar apenas o id do container que está rodando
docker ps -q

> Trabalando com volumes (-v) - DEU PROBLEMA PARA ACHAR O VOLUNME
docker run -ti -v /volume ubuntu /bin/bash

df -h para visualizar o volume criado

ls - também visualiza o volume

O que for alterado nesse diretório /volume, irá repercutir no volume docker

>Buscando o volume criado
docker ps
docker inspect -f {{.Mounts}} CONTAINER_ID

-> criando volume especificando o local da criação
	               DIRETORIO CRIACAO    :/VOLUME CRIADO NOME
docker run -ti -v /root/primeiro_dockerfile/:/volume/ debian

entrar no /volume: cd /volume
criar o file: touch Dockerfile
Quando cria no volume, ele repercute para a img do docker.

> Inspect docker container logs
docker logs --details ID_CONTAINER


>> Inspect network adress
sudo docker network inspect ingress


> listar volumes docker por CONTAINTER_ID

--->> listar os ids dos containers
	docker ps -q

--->> listar o volume pelo id
	docker inspect -f '{{ .Mounts }}'  CONTAINTER_ID
>> Listar os overlays (networks)
docker networks ls
-> Docker commit - criar nova imagem

	sudo docker commit ID_CONTAINER ubuntu/nginx

Ao executar o commit, estamos criando uma nova imagem baseada na
inicial. A nomenclatura utilizada ubuntu/nginx é o nome da nova imagem.
Verifique com o comando docker images :

	docker images

-> subir container que vai morrer quando sair do terminal (--rm)
--rm (apaga container)
--rmi (apaga container e imagens)
sudo docker run -it --rm -p 8080:80 ID_IMAGE	

-> remover todos os containers pelo id
--rm (containeres)
--rmi (containeres e imagens)
sudo docker rm $(docker ps ‐qa)

-> gerar .jar a partir da imagem (SAVE)

 sudo docker save IMAGEM > /tmp/IMAGEM.tar

-> gerar  imagem a partir do .jar (LOAD)

-> Buscando imagens no dockerhub
  sudo docker search NAME_IMAGE

-> credenciais docker nexus images
home/seu_usuario/.docker/config.json
add o conteúdo
{
  "insecure-registries": [
    "172.25.100.75:3000"
  ]
}

-> Evitar dar o sudo nos comandos docker
sudo usermod -aG docker $(whoami)

################################
Run images docker with volumes
################################

--MONGO
mkdir ~/data
sudo docker run -d -p 27017:27017 -v ~/data:/data/db mongo


REDIS - TESTE
docker run --name REDIS_SERVER -p 6379:6379 --volume /data/redis-server:/data -d redis redis-server --appendonly yes

####################################################################################################
##                                       SERVIDORES - COTIC
####################################################################################################

##Servidor COTIC
172.25.100.75

##Servidor COTIC - DESEN
172.25.100.157

#Brunos Machine
172.25.10.87

############################
Mongo
database:
172.25.100.157
user: user_pca


####################################################################################################
##                                       DOCKER SWARM
####################################################################################################


####################################################################################################
##                                       CREATE A LOCAL TAG AND SEND TO REPOSITORY
####################################################################################################


Create a tag local from docker pull and send to nexus (local repository)

https://help.sonatype.com/repomanager3/formats/docker-registry/pushing-images

docker tag redis 172.25.100.75:3000/redis:6.0.8



####################################################################################################
##                                GENERATE KEYPASSWORD
####################################################################################################

 openssl rand -base64 64