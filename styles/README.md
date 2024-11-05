## 1. Integração de Serviços REST e gRPC em um Sistema de Inventário

**Descrição da Tarefa:** Crie um sistema de inventário que utiliza uma API REST para o acesso a dados do MongoDB e uma API gRPC para atualizações de inventário de alta frequência. 

**Desafio de Integração:** Configure o Kong para rotear requisições REST para o serviço de leitura e gRPC para o serviço de atualização, ambos expostos em uma única URL pública.

gRPC Server (para atualizações):

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
REST Server (para consultas):
```


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

## 2. Monitoramento em Tempo Real com WebSockets e REST para Logs de Aplicação

**Descrição da Tarefa:** Desenvolva um serviço de logs de aplicação que usa WebSockets para atualizações em tempo real (para um cliente de monitoramento) e REST para consultas a logs históricos armazenados no Cassandra.

**Desafio de Integração:** Configure o Kong para manter a conexão WebSocket aberta para atualizações em tempo real e, ao mesmo tempo, rotear requisições REST para consultas mais antigas.

### WebSocket Server

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

## 3. Sistema de Consulta com GraphQL e Notificações em Tempo Real

**Descrição da Tarefa:** Crie um sistema de consulta de dados com GraphQL usando MongoDB como banco de dados e um serviço de notificações em WebSocket para atualizações ao vivo (por exemplo, para notificar novos registros).

**Desafio de Integração:** Utilize o Kong para expor uma única URL que gerencie tanto consultas GraphQL como notificações WebSocket, com foco em configurações de roteamento e controle de tráfego para as requisições GraphQL.

### GraphQL Server

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
WebSocket Server (para Notificações):
```

### Notity:

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

## 4. Centralização de Mensagens em Tempo Real com Kafka e WebSocket
**Descrição da Tarefa:** Desenvolva um sistema de mensagens em tempo real em que eventos gerados (como ações de usuários) são publicados em um tópico Kafka e consumidos por um serviço WebSocket para exibição em dashboards.
**Desafio de Integração:** Configure o Kong para rotear as requisições de clientes para o WebSocket e gerenciar a comunicação com o Kafka para escalar a entrega dos eventos.

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
