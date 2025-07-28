---
aliases:
  - "DPDK"
  - "Data Plane Development Kit"
  - "userspace пакетная обработка"
tags:
  - "#network #performance #packet-processing"
path:
  - "компьютерные/сети/ускорение"
---

## 📌 DPDK  
Data Plane Development Kit [[DPDK]] — набор библиотек и драйверов для высокопроизводительной обработки сетевых пакетов в пространстве пользователя, минуя стандартный стек [[kernel]].

## 🧠 Как работает  
[[DPDK]] использует специальные [[PMD]] (Poll Mode Drivers), напрямую взаимодействующие с [[NIC]] через [[PCIe]] с использованием:

- [[HugePages]] — для эффективного управления памятью  
- [[NUMA]]-оптимизации — распределение потоков и буферов  
- [[Poll-based IO]] — постоянный опрос буфера, без прерываний  
- [[Zero-copy]] — минимизация операций копирования

Пакеты обрабатываются в ring-буферах (`rte_ring`) и memory pool'ах (`rte_mempool`) внутри одного процесса или между потоками.

## ⚙️ Где применяется

| Сфера               | Примеры использования                   |
| ------------------- | --------------------------------------- |
| [[NFV]]             | [[vRouter]], [[vFirewall]], [[vSwitch]] |
| [[Telecom]]         | EPC, 5G Core, IMS                       |
| [[DDoS]] Protection | [[XDP]] + [[DPDK]] гибридные схемы      |
| Traffic Generator   | [[Pktgen]], [[TRex]]                    |
| [[Load Balancing]]  | [[Katran]], [[HAProxy]] с DPDK backend  |

## 💻 Пример (инициализация EAL и запуск порта)

```c
rte_eal_init(argc, argv);                       // инициализация среды
port_id = 0;
rte_eth_dev_configure(port_id, rx_queues, tx_queues, &port_conf);
rte_eth_rx_queue_setup(port_id, ...);
rte_eth_tx_queue_setup(port_id, ...);
rte_eth_dev_start(port_id);                     // запуск порта
````

## 📐 Архитектура DPDK

```
[App Thread]
   ↓
[DPDK API]
   ↓
[PMD Driver] → [NIC]
         ↖     |
     [HugePages, Mempool]
```

## 🧩 Взаимодействие с железом

- Прямой доступ к регистрам [[NIC]] (через [[UIO]] / [[VFIO]])
    
- Использование [[Polling]] вместо [[Interrupts]]
    
- Поддержка [[SR-IOV]], [[Multiqueue]], [[RSS]]
    

### ✅ Преимущества

- Пропускная способность: миллионы пакетов в секунду (pps)
    
- Минуется [[kernel]] — низкая задержка
    
- Подходит для сетевых функций и [[SDN]]
    

### ❌ Недостатки

- Требует полной изоляции CPU и памяти (Core Pinning, [[HugePages]])
    
- Нет взаимодействия с обычными сокетами без [[AF_XDP]] или [[vhost]]
    
- Необходимы поддерживаемые драйверы и [[NIC]]