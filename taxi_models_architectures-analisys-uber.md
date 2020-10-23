###################################
# Uber Architecture:
###################################

Link reference: https://medium.com/@narengowda/uber-system-design-8b2bc95e2cfe

> Era monolítico

> Python e usou SQLAlchemy (inicialmente para uma quantidade modesta de solicitações)

> Após 2014, a arquitetura evoluiu para uma arquitetura orientada a serviços com cerca de 100s de serviços

> Cada táxi ativo continua enviando latitude para o servidor a cada 5 segundos uma vez.

>  A alocação precisa ser rastreada. Um veículo, por exemplo, pode ter três assentos, mas dois deles estão ocupados. (Melhoria para nosso projeto)


> A terra é uma esfera. É difícil fazer resumo e aproximação baseados puramente em longitude e latitude. Assim, o Uber divide a Terra em pequenas células usando a biblioteca do Google S2. Cada célula possui um ID de célula exclusivo.
S2 pode dar a cobertura para uma forma. Se você deseja desenhar um círculo com um raio de 1 km centrado em Londres, o S2 pode dizer quais células são necessárias para cobrir completamente a forma

> A solução para o dimensionamento é o nó js com ringpop , é um protocolo RPC mais rápido com fofocas usando o protocolo SWIM junto com um anel hash consistente

> O Apache Kafka é usado como o hub de dados


----> Como funcionam os mapas e o roteamento?
Antes de o Uber iniciar operações em uma nova área, definimos e embarcamos em uma nova região para nossa pilha de tecnologias de mapas. Dentro desta região do mapa, definimos sub-regiões rotuladas com graus A, B, AB e C, da seguinte forma:
A - maior demanda (representa 90%)
B - área rural
AB -  união
C - rodovias

#Projeto geoespacial:
A terra é uma esfera. É difícil fazer resumo e aproximação baseados puramente em longitude e latitude.
Assim, o Uber divide a Terra em pequenas células usando a biblioteca do Google S2. Cada célula possui um ID de célula exclusivo.


> você pode usar algoritmos simulados de IA ou Dijkstra simples também para encontrar a melhor rota.
Um exemplo simples que você pode tentar em casa é o algoritmo de pesquisa do Dijkstra , que se tornou a base dos algoritmos de roteamento mais modernos atualmente.
O OSRM é baseado em hierarquias de contração . Os sistemas baseados em hierarquias de contração alcançam desempenho rápido - demorando apenas alguns milissegundos para calcular uma rota - pré-processando o gráfico de roteamento.


#Bancos de dados?
Muitos bancos de dados diferentes são usados. Os sistemas mais antigos foram escritos no Postgres.
Redis é muito usado. Alguns estão por trás do Twemproxy. Alguns estão atrás de um sistema de cluster personalizado.

Armazenamento de dados de viagem em Schemaless

MySQL: Construído sobre esses requisitos
adicione capacidade linearmente adicionando mais servidores (escalável horizontalmente)
disponibilidade de gravação com buffer usando Redis
Os gatilhos devem funcionar quando houver uma alteração na instância
Sem tempo de inatividade para qualquer operação (expansão de armazenamento, backup, adição de índices, adição de dados e assim por diante).


Riak (no sql - key-value)


###################################
# Uber Business Model:
###################################

Link reference: https://pt.slideshare.net/funk97/ubers-business-model?next_slideshow=1


#################################
# Spring Taxi Dispatch Simulator
#################################

Link reference: https://github.com/carsonluuu/Real-Time-Taxi-Dispatch-Simulator