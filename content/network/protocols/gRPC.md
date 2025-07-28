---
aliases:
  - "gRPC"
  - "Google Remote Procedure Call"
  - "протокол gRPC"
tags:
  - "#network #rpc #protocol"
path:
  - "программирование/сети/протоколы"
---

## 📌 gRPC  
[[gRPC]] — высокопроизводительный фреймворк удалённого вызова процедур (RPC), разработанный Google, использующий [[HTTP]]/2 и [[Protocol Buffers]] для эффективной и типизированной передачи данных между сервисами.

## 🧠 Как работает  
[[gRPC]] строится на следующих принципах:

- Коммуникация через [[HTTP]]/2 — мультиплексирование, сжатие, push  
- Интерфейс описывается в `.proto` файле на [[Protobuf]]  
- Генерация клиентских и серверных SDK на разных языках  
- Поддержка синхронных и асинхронных вызовов  
- Встроенные типы стриминга:
  - Unary (1:1)
  - Server Streaming (1:N)
  - Client Streaming (N:1)
  - Bidirectional Streaming (N:N)

Архитектура:

```

Client ↔ gRPC ↔ HTTP/2 ↔ Server  
↕ ↕  
Protobuf Protobuf

````

## ⚙️ Где применяется

| Область            | Примеры                                      |
|-----------------------|----------------------------------------------|
| [[Microservices]]  | Взаимодействие между сервисами               |
| [[Service Mesh]]   | [[Envoy]], [[Istio]], [[Linkerd]]            |
| [[IoT]]            | RPC в встраиваемых системах                  |
| [[Mobile Backend]] | RPC между приложением и сервером             |
| [[API]]s           | Быстрая альтернатива [[REST]]                |

## 💻 Пример (gRPC + Python)

```proto
// service.proto
service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}
````

```bash
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. service.proto
```

```python
# server.py
from concurrent import futures
import grpc
import greeter_pb2_grpc

server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
greeter_pb2_grpc.add_GreeterServicer_to_server(GreeterImpl(), server)
server.add_insecure_port('[::]:50051')
server.start()
```

## 🧩 Составляющие gRPC

| Компонент    |Назначение|
|---|---|
| [[Protobuf]] |Формат сериализации данных|
| [[HTTP]]/2   |Транспортный слой|
| Stub         |Автоматически сгенерированный клиент|
| Server       |Обработчик RPC-запросов|

### ✅ Преимущества

- Высокая производительность и сжатие
    
- Строгая типизация и автогенерация кода
    
- Поддержка стриминга, TLS, Load Balancing и Interceptor'ов
    

### ❌ Недостатки

- Требует Protobuf и генерации кода
    
- Менее нагляден, чем [[REST]] (особенно без gRPC UI)
    
- Не поддерживается в браузерах напрямую (нужен [[gRPC-Web]])