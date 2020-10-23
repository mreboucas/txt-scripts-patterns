

docker service create --name server -p 8761:8761 --network pcaNetwork 172.25.100.75:3000/pca/server
docker service create --name SCALE -p 30029:8090  -e url=http://172.25.100.243:80 -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c -e PORT=30029 -e PUBLISH_IP=172.25.100.132  172.25.100.75:3000/pca/scale:0.0.3
docker service create --name GTLOCK -p 30020:8090 172.25.100.75:3000/pca/gtlock:0.0.1
docker service create --name AUDIT -p 30021:8090  -e PORT=30021 -e PUBLISH_IP=172.25.100.132 172.25.100.75:3000/pca/audit
docker service create --name SIC -p 30001:8090 -e PORT=30001 -e PUBLISH_IP=172.25.100.132 172.25.100.75:3000/pca/sic
docker service create --name DETRAN -p 30011:8090  -e PORT=30011 -e PUBLISH_IP=172.25.100.132 172.25.100.75:3000/pca/detran

// add secret
docker service create --name USER-PCA -p 30024:8090 --secret secret-db-pca --network pcaNetwork -e PORT=30024 -e PUBLISH_IP=172.25.100.132 -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=e9c246ba9cf479b6e10331c0e4178bca -e appKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c -e secret=2DBA2DB7BD668932D25643C36CFCF -e listEmail="policiacivil.ce.gov.br, sspds.ce.gov.br, sap.ce.gov.br, cb.ce.gov.br, supesp.ce.gov.br, pefoce.ce.gov.br, pm.ce.gov.br" -e listEmailTemporary="gmail.com, hotmail.com, ymail.com" -e institution=3 172.25.100.75:3000/pca/user-pca:0.0.6

// REDESIM
docker service create --name redesim-access  172.25.100.75:3000/redesim/redesim-access
docker service create --name redesim-certificate  172.25.100.75:3000/redesim/redesim-certificate
docker service create --name redesim-rejected 172.25.100.75:3000/redesim/redesim-rejected
docker service create --name redesim-sync 172.25.100.75:3000/redesim/redesim-sync
docker service create --name redesim-payment  172.25.100.75:3000/redesim/redesim_payment

//TRACKING
docker service create --name TRACKING -p 30034:8080 -e PUBLISH_IP=172.25.100.243 -e PORT=30034 172.25.100.75:3000/pca/tracking
docker service create --name TRACKING-GEAR -p 30032:8090 --network pcaNetwork  -e PUBLISH_IP=172.25.100.243 -e PORT=30032 172.25.100.75:3000/pca/tracking-gear
docker service create --name SCALE-WEBSOCKET -p 30031:8060  -e url=http://172.25.100.243:80 -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c -e PORT=30029 -e PUBLISH_IP=172.25.100.132  172.25.100.75:3000/pca/scale-websocket:0.0.2
docker service create --name SCALE-TRACKING -p 30033:8090 --network pcaNetwork  -e url=http://172.25.100.243:80 -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c -e PORT=30033 -e PUBLISH_IP=172.25.100.132  172.25.100.75:3000/pca/scale:0.0.3

https://rota.netlify.app/

//SIGERH (SEPLAG) - Vitinho/Felipe
sudo docker service create --name SIGERH -p 30040:8080 --constraint node.role==manager 172.25.100.75:3000/sigerh/sigerh-sspds/dev:0.0.1-SNAPSHOT 


############################################################################################

--Listar os serviços
docker services ls

--Listar os nós do swarm
docker node ls

--Remover os serviços do docker
docker service rm ID_SERVICE

------------------
-- Servidores
------------------
172.25.100.243
172.25.100.132
172.25.100.88

--Subir container em um nó especídfico ou não.
node.id==atribuir
node.id!=evitar

node.role==manager|worker

#######################################
--SCALE-TRACKING
sudo docker service create --name SCALE-TRACKING -p 30033:8060 --network pcaOverlay  -e url=http://172.25.100.243:80/ -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c -e PORT=30033 -e PUBLISH_IP=172.25.100.132  172.25.100.75:3000/pca/scale:0.0.3

--CVLI-CVP
sudo docker service create --name CVLI-CVP -p 30666:8090 --network pcaOverlay -e PORT=30666 -e PUBLISH_IP=172.25.100.132 --constraint node.id==qmq5o9dwzkfo0opgdp5tnwamr 172.25.100.75:3000/rotas/wscvp-cvli:wscvp-cvli-0.1.1

--machine's bugs

--Audit
docker service create --name AUDIT -p 30021:8090  -e PORT=30021 -e PUBLISH_IP=172.25.100.132 --constraint node.id==aj74oxjw4quwfl4hvu8q7u08f 172.25.100.75:3000/pca/audit:latest

--Scale
docker service create --name SCALE -p 30029:8090  -e PORT=30029 -e PUBLISH_IP=172.25.100.132 -e url=http://172.25.100.243:80 -e url_siaa=http://172.25.100.213:31423/api/siaa -e appServerKey=d74ae4c7edc6bb42b1f3c403d6f1bd1c 172.25.100.75:3000/pca/scale:0.0.2

--SIC
docker service create --name SIC -p 30001:8090 --constaint node.role==manager -e PORT=30001 -e PUBLISH_IP=172.25.100.132 172.25.100.75:3000/pca/sic

docker service create --name SIC -p 30001:8090 --constaint node.role==worker -e PORT=30001 --network pcaOverlay -e PUBLISH_IP=172.25.100.85  172.25.100.75:3000/pca/sic/dev:0.6.4


>>>>>Buscar serviço pelo nome:
sudo docker service ls | grep -i sic

>>>>>Remover service
sudo docker service rm ID_SERVICE

Link to create a service:
https://docs.docker.com/engine/reference/commandline/service_create/