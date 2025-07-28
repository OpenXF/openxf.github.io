---
aliases:
  - "XDP"
  - "eXpress Data Path"
  - "низкоуровневая фильтрация пакетов"
tags:
  - "#linux #network #ebpf #performance"
path:
  - "компьютерные/ядро/eBPF"
---

## 📌 XDP  
eXpress Data Path [[XDP]] — подсистема в Linux [[kernel]], использующая [[eBPF]] для сверхбыстрой обработки сетевых пакетов на самых ранних этапах поступления, до попадания в сетевой стек.

## 🧠 Как работает  
[[XDP]] внедряется в драйвер сетевой карты и позволяет запускать [[eBPF]]-программы **до** того, как пакет обработан стеком [[TCPIP|TCP/IP]]. Это даёт:

- Минимальную задержку (без копирования в [[RAM]])  
- Мгновенную фильтрацию или модификацию пакетов  
- Возврат действий: `XDP_PASS`, `XDP_DROP`, `XDP_TX`, `XDP_REDIRECT`

Интеграция идёт через `ip link` или [[bpftool]]. Программа может читать/записывать данные из [[BPF Maps]], использовать [[Verifier]] и быть загруженной через [[libbpf]].

## ⚙️ Где применяется

| Область              | Примеры                              |
| --------------------- | ------------------------------------ |
| DDoS-митиграция       | [[Cloudflare XDP]], Facebook [[XDP]] |
| IDS/IPS системы       | [[Suricata eBPF]], [[Snort XDP]]     |
| Платформенные CNI     | [[Cilium]], [[Katran]]               |
| Балансировка нагрузки | [[XDP]] Load Balancer, [[Maglev]]    |
| Хостовая защита       | [[Tetragon]], [[XDP]] firewall       |

## 💻 Пример (минимальный XDP-drop)

```c
SEC("xdp")
int xdp_drop(struct xdp_md *ctx) {
    return XDP_DROP;
}
````

```bash
clang -O2 -target bpf -c xdp_drop.c -o xdp_drop.o
ip link set dev eth0 xdp obj xdp_drop.o sec xdp
```

## 🧩 Архитектура XDP

```
[ NIC Driver ]
      ↓
[ XDP Hook ] ←→ [eBPF Program]
      ↓
[ Linux TCP/IP Stack ]
```

## 📐 Возврат значений XDP-программы

|Значение|Действие|
|---|---|
|`XDP_DROP`|Удалить пакет|
|`XDP_PASS`|Пропустить в стек|
|`XDP_TX`|Отправить обратно через тот же NIC|
|`XDP_REDIRECT`|Передать на другой интерфейс или CPU|
|`XDP_ABORTED`|Ошибка выполнения|

### ✅ Преимущества

- Ультранизкая задержка, обход ядра и стеков
    
- Позволяет строить user-space firewall, [[DDoS]] mitigation
    
- Работает на уровне драйвера, близко к железу
    

### ❌ Недостатки

- Требует поддержки от [[NIC]] Driver
    
- Отладка сложнее, чем в userspace
    
- Ограничения по сложным eBPF-программам (проверка [[Verifier]])