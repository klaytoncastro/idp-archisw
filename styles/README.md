# Arquitetura de API e Estilos de Comunicação 

## 1. Visão Geral

A arquitetura de uma API é um elemento essencial no design e desenvolvimento de software moderno. Ela define como os sistemas de software interagem, impactando profundamente a eficiência, a flexibilidade e a robustez de uma aplicação. A escolha de um estilo arquitetural para APIs envolve não apenas uma decisão técnica, mas também uma avaliação cuidadosa dos objetivos de negócio, das exigências de escalabilidade e dos requisitos de manutenção e evolução do sistema. Cada estilo arquitetural apresenta suas vantagens e limitações, que devem ser compreendidas no contexto da aplicação pretendida.


## 2. Estilos de Arquitetura 

A seguir, descrevemos seis estilos arquiteturais comumente utilizados para a construção de APIs: REST, SOAP, GraphQL, gRPC, WebSockets e MQTT. Cada um desses estilos possui uma abordagem e peculiaridades, adaptando-se de formas variadas a diferentes contextos de uso, desde sistemas de alta disponibilidade até dispositivos IoT.

### 2.1. Representational State Transfer (REST)

REST é um estilo arquitetural criado para proporcionar uma forma leve e eficiente de comunicação entre sistemas. Baseado no protocolo HTTP, REST utiliza operações padronizadas como GET, POST, PUT e DELETE, permitindo que recursos sejam manipulados de forma consistente e previsível. O REST é especialmente valorizado por sua simplicidade e capacidade de escalabilidade.
- **Origens e Conceitos**: Proposto por Roy Fielding em 2000, o REST foi uma alternativa mais simples e robusta aos modelos XML complexos, como o SOAP. Sua estrutura é centrada no conceito de "recursos" identificados por URLs, oferecendo um padrão claro para a construção de APIs.
- **Aplicabilidade e Limitações**: REST é amplamente utilizado em sistemas distribuídos pela sua simplicidade, mas limita-se ao paradigma HTTP, o que pode representar um desafio em casos que exijam alta performance e baixa latência.

### 2.2. Simple Object Access Protocol (SOAP)

SOAP é um protocolo de comunicação que opera sobre uma estrutura XML, ideal para ambientes corporativos e governamentais onde a segurança e a integridade dos dados são cruciais. Este protocolo garante a conformidade com padrões rigorosos e é frequentemente utilizado em transações financeiras e aplicações de missão crítica.
- **Características e Padrões**: SOAP utiliza uma estrutura padronizada, composta por cabeçalhos, corpo e fault, que garante consistência na troca de mensagens. É compatível com múltiplos protocolos de transporte, incluindo HTTP e SMTP, e é frequentemente integrado com serviços de segurança, como o WS-Security.
- **Usos Comuns e Considerações**: A complexidade do SOAP torna-o mais adequado para ambientes onde a segurança é uma prioridade, ainda que o peso de sua estrutura possa dificultar a comunicação em sistemas menos robustos.

### 2.3. GraphQL

GraphQL foi desenvolvido para proporcionar uma abordagem flexível à recuperação de dados, permitindo que o cliente especifique exatamente quais informações deseja receber. Isso minimiza o problema de "over-fetching" e "under-fetching" característico de APIs REST, especialmente em casos onde dados complexos estão inter-relacionados.
- **Vantagens e Desafios**: Com uma estrutura declarativa, GraphQL permite que o cliente defina os dados necessários em uma única chamada. No entanto, seu uso requer controle rigoroso de segurança, pois oferece acesso mais granular aos dados.
- **Contexto de Aplicação**: GraphQL é ideal para aplicações com dados complexos e interconectados, sendo amplamente utilizado em interfaces de usuário ricas, onde a flexibilidade de consulta é um diferencial importante.

### 2.4. gRPC

gRPC, desenvolvido pelo Google, é um framework de comunicação eficiente para sistemas distribuídos que utiliza HTTP/2 e Protocol Buffers para alcançar baixa latência e alta performance. Seu design o torna particularmente adequado para arquiteturas de microserviços.
- **Estrutura e Vantagens**: Ao suportar chamadas RPC e diferentes tipos de streaming (unidirecional, bidirecional), o gRPC oferece flexibilidade na comunicação e é compatível com várias linguagens, tornando-o uma escolha robusta para sistemas distribuídos de alta performance.
- **Considerações Práticas**: gRPC é ideal para aplicações onde o desempenho é crucial, embora sua complexidade possa ser excessiva em sistemas que não demandam operações em tempo real.

### 2.5. WebSockets

WebSockets fornecem uma comunicação bidirecional em tempo real entre cliente e servidor, mantendo uma conexão persistente que evita o polling frequente. Esse protocolo é essencial em aplicações que exigem atualizações constantes, como chats e sistemas de monitoramento.
- **Características**: Diferente de HTTP, o WebSocket permite uma comunicação contínua e reativa, respondendo a eventos de forma imediata e interativa.
- **Usos Típicos**: Utilizado em sistemas que demandam alta responsividade e baixa latência, como notificações e monitoramento, WebSockets são populares em plataformas de comunicação instantânea e monitoramento de processos.

### 2.6. MQTT (Message Queuing Telemetry Transport)

MQTT é um protocolo leve e otimizado para dispositivos de Internet das Coisas (IoT), sendo ideal para redes instáveis ou de baixa largura de banda. Ele utiliza um modelo de publicação/assinatura que facilita a disseminação eficiente de dados em grandes redes de dispositivos.
- **Funcionalidades e Contexto**: A simplicidade e a leveza do MQTT o tornam ideal para sensores e dispositivos de IoT, onde a estabilidade da rede pode ser um desafio.
- **Principais Aplicações**: Amplamente adotado em automação residencial e sistemas de IoT, o MQTT é essencial em contextos onde a comunicação eficiente e econômica é necessária.

---

## 3. API Gateway

Em arquiteturas modernas, particularmente as baseadas em microserviços, o API Gateway serve como um ponto central de entrada para todas as requisições. Ele permite o gerenciamento unificado de autenticação, controle de tráfego, roteamento de requisições e monitoramento, simplificando a complexidade de gerenciar múltiplos serviços individuais. Assim, o API Gateway atua como um intermediário, transformando, autentificando e direcionando as requisições para os serviços de backend apropriados. Nesse cenário, o projeto Kong oferece uma plataforma de API Gateway de código aberto, projetada para atuar como uma camada intermediária entre clientes e serviços baseados em APIs, e se destaca por sua capacidade de abstrair e gerenciar a comunicação entre diferentes serviços. 

### 3.1. Características

O Kong oferece uma série de funcionalidades que facilitam a criação e a operação de APIs escaláveis e seguras:
- **Autenticação e Segurança**: Suporta autenticação com tokens JWT, OAuth, ACLs e IP Restriction, além de SSL dinâmico.
- **Controle de Tráfego e Rate Limiting**: Permite limitar o número de requisições por unidade de tempo, protegendo o backend de sobrecargas.
- **Transformação de Requisições**: Suporta modificações nos parâmetros e cabeçalhos das requisições para adequá-las às necessidades dos serviços backend.
- **Monitoramento e Logs**: Integra-se com ferramentas de monitoramento, como Prometheus, Datadog e ELK, possibilitando uma visão completa sobre o uso das APIs.

O Kong possui uma **edição para comunidade (CE)**, que é open-source, e uma **edição empresarial (EE)**, que inclui recursos adicionais para grandes empresas, como um portal de desenvolvedores, escalabilidade avançada e suporte 24/7.

### 3.2. Arquitetura do Kong

A arquitetura do Kong é composta por duas camadas principais:

- **Kong Server**: Responsável pelo roteamento e processamento das requisições. Ele conta com uma camada pública para gerenciar requisições e uma camada privada para configurar APIs e plugins.

- **Datastore do Kong**: O Kong utiliza um banco de dados externo (como PostgreSQL ou Cassandra) para armazenar suas configurações, além de um cache próprio para melhorar a performance.

O arquivo `docker-compose.yml` desta pasta levanta o Kong como API Gateway, juntamente com um banco de dados PostgreSQL para armazenar as suas configurações. Os contêineres adicionais para os serviços de banco de dados e serviços de mensageria citados como exemplo podem ser aproveitados no repositório [IDP-BigData](https://github.com/klaytoncastro/idp-bigdata).

### 3.2. Acessando o Kong:

- Proxy `HTTP`: `http://localhost:8000`
- Proxy `HTTPS`: `https://localhost:8443`
- Admin `HTTP`: `http://localhost:8001`
- Admin `HTTPS`: `https://localhost:8444`

### 3.4. Configurando Rotas e Serviços

Com o Kong em execução, agora é possível configurar rotas, serviços e plugins via API de administração. Você poderá definir rotas específicas para cada estilo arquitetural de API implementado nas tarefas seguintes, permitindo que o Kong faça o roteamento conforme necessário. Para configurar uma rota no Kong para a API REST, você pode usar um comando `curl` como exemplo:

```bash
curl -i -X POST http://localhost:8001/services/ \
  --data "name=inventory-service" \
  --data "url=http://localhost:5000"

curl -i -X POST http://localhost:8001/services/inventory-service/routes \
  --data "paths[]=/inventory"
```

## 4. Atividade Prática

Em nossa atividade prática, seguem exemplos de tarefas comuns para aplicar a integração de diferentes estilos arquiteturais de API (REST, gRPC, GraphQL, WebSockets, e MQTT) usando o Kong como API Gateway. O Kong permite orquestrar e gerenciar essas diferentes arquiteturas em uma camada de entrada central. 

### 4.1. Integração de Serviços REST e gRPC em um Sistema de Inventário

**Grupo**: Rafael Cândido, Luca Verdade, Lucas Fidalgo, Vinicius

**Objetivo da Tarefa:** Desenvolver um sistema de inventário que utiliza uma API REST para acesso a dados do MongoDB e uma API gRPC para atualizações de alta frequência, utilizando o Kong para rotear requisições de forma unificada.


### gRPC Server (para atualizações):

```python

import grpc
from concurrent import futures
import inventory_pb2_grpc  # Arquivo gerado pelo compilador gRPC
import inventory_pb2
import pymongo

# Conexão com MongoDB
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["inventoryDB"]

class InventoryService(inventory_pb2_grpc.InventoryServiceServicer):
    def UpdateInventory(self, request, context):
        # Atualiza o inventário no MongoDB
        db.products.update_one({"id": request.product_id}, {"$set": {"quantity": request.quantity}})
        return inventory_pb2.InventoryResponse(status="Atualizado com sucesso")

server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
inventory_pb2_grpc.add_InventoryServiceServicer_to_server(InventoryService(), server)
server.add_insecure_port('[::]:50051')
server.start()
server.wait_for_termination()
```

### REST Server (para consultas): 

```python
from flask import Flask, jsonify
import pymongo

app = Flask(__name__)
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["inventoryDB"]

@app.route('/products', methods=['GET'])
def get_products():
    products = list(db.products.find({}, {"_id": 0}))
    return jsonify(products)

if __name__ == '__main__':
    app.run(port=5000)
```

### 4.2. Monitoramento em Tempo Real com WebSockets e REST para Logs de Aplicação

**Grupo**: Távora, Bee, Petrus, Vitor

**Objetivo da Tarefa:** Desenvolver um serviço de logs de aplicação com WebSockets para atualizações em tempo real e REST para consultas a logs históricos no Redis ou Cassandra, utilizando o Kong para gerenciar as conexões.

### WebSocket Server:

```python

import asyncio
import websockets
import json
from cassandra.cluster import Cluster

async def log_updates(websocket, path):
    cluster = Cluster()
    session = cluster.connect('logs')
    query = "SELECT * FROM log_entries LIMIT 10"
    result = session.execute(query)
    
    for row in result:
        await websocket.send(json.dumps({"log_id": row.log_id, "message": row.message}))
    await asyncio.sleep(1)

start_server = websockets.serve(log_updates, "localhost", 6789)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

### REST Server:

```python

from flask import Flask, jsonify
from cassandra.cluster import Cluster

app = Flask(__name__)
cluster = Cluster()
session = cluster.connect('logs')

@app.route('/logs', methods=['GET'])
def get_logs():
    query = "SELECT * FROM log_entries LIMIT 10"
    rows = session.execute(query)
    logs = [{"log_id": row.log_id, "message": row.message} for row in rows]
    return jsonify(logs)

if __name__ == '__main__':
    app.run(port=5001)
```

### 4.3. Sistema de Consulta com GraphQL e Notificações em Tempo Real

**Grupo**: Mateus Batista, Lucas Rabelo, João Henrique

**Objetivo da Tarefa:** Desenvolver um sistema de consulta de dados com GraphQL e MongoDB, incluindo um serviço de notificações em WebSocket para atualizações em tempo real, com o Kong gerenciando as conexões.

### GraphQL Server (para Consultas):

```python
from flask import Flask
from flask_graphql import GraphQLView
from graphene import ObjectType, String, Int, Schema
import pymongo

app = Flask(__name__)
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["inventoryDB"]

class Product(ObjectType):
    id = String()
    name = String()
    quantity = Int()

class Query(ObjectType):
    product = String(id=String())

    def resolve_product(root, info, id):
        return db.products.find_one({"id": id})

schema = Schema(query=Query)
app.add_url_rule('/graphql', view_func=GraphQLView.as_view('graphql', schema=schema, graphiql=True))

if __name__ == '__main__':
    app.run(port=5002)
```

### WebSocket Server (para Notificações):

```python

import asyncio
import websockets

async def notify_updates(websocket, path):
    while True:
        await websocket.send("Nova atualização disponível.")
        await asyncio.sleep(5)

start_server = websockets.serve(notify_updates, "localhost", 6790)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

### 4.4. Centralização de Mensagens em Tempo Real com Kafka e WebSocket

**Grupo**: Matheus Antônio
<!--Leonardo Freitas, Maria Fernanda-->

**Objetivo da Tarefa:** Desenvolver um sistema de mensagens em tempo real que utiliza Kafka para publicação de eventos e WebSocket para exibição em dashboards, com o Kong gerenciando as conexões de clientes.

### Produtor Kafka:

```python

from kafka import KafkaProducer
import json
import time

producer = KafkaProducer(bootstrap_servers='localhost:9092', value_serializer=lambda v: json.dumps(v).encode('utf-8'))

while True:
    data = {"event": "Ação do usuário", "timestamp": time.time()}
    producer.send('user_events', data)
    time.sleep(2)
WebSocket Server (Consumidor Kafka):
```

### Consumidor Kafka: 

```python

import asyncio
import websockets
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer('user_events', bootstrap_servers='localhost:9092', value_deserializer=lambda m: json.loads(m.decode('utf-8')))

async def stream_events(websocket, path):
    for message in consumer:
        await websocket.send(json.dumps(message.value))
        await asyncio.sleep(1)

start_server = websockets.serve(stream_events, "localhost", 6788)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

## 5. Conclusão

A integração de diferentes estilos arquiteturais de API — REST, gRPC, GraphQL, WebSockets e MQTT — usando o Kong como API Gateway para centralizar e gerenciar a comunicação entre serviços, oferece uma visão básica das funcionalidades e limitações de cada estilo arquitetural, destacando o funcionamento de um API Gateway em arquiteturas distribuídas e de microserviços.