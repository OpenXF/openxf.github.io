---
title: SDIO (Secure Digital Input Output)
aliases:
  - "Secure Digital Input Output"
  - "SDIO"
  - "интерфейс SDIO"
  - "SDIO интерфейс"
tags:
  - "#sdio"
  - "#interface"
  - "#hardware"
  - "#storage"
  - "#glossary"
path:
  - "hardware/interface/sdio"
---

## 📌 SDIO

**SDIO** (Secure Digital Input Output) — интерфейс расширения стандарта Secure Digital (SD), позволяющий подключать периферийные устройства (например, Wi-Fi модули, GPS, камеры, кардридеры) к хост-контроллеру через стандартный слот SD-карты. SDIO поддерживает как передачу данных, так и управление устройством через унифицированный протокол.

---

## 🧠 Как работает

### Основные характеристики

- Совместимость с SD-картами по физическому разъёму и электрическим сигналам.
- Поддержка 1-, 4-битной и высокоскоростной передачи (до нескольких сотен Мбит/с).
- Протокол команд и ответов схож с SD, но с расширением для управления периферией.
- Устройства SDIO могут поддерживать несколько функций (multi-function devices).
- Хост взаимодействует с устройством через стандартные регистры и блоки данных.

### Физический интерфейс

- Линии передачи данных:  
  - CMD — линия команд  
  - CLK — тактовый сигнал  
  - DAT0..DAT3 — линии данных (один или четыре бита)
- Совместимость с SD, возможность работы в режиме SPI (ограниченно).

### Логическая схема SDIO

```mermaid
flowchart TB
    Host["Host Controller (MCU/SoC)"]
    CMD["CMD Line"]
    CLK["Clock Line"]
    DAT0["Data Line DAT0"]
    DAT1["Data Line DAT1"]
    DAT2["Data Line DAT2"]
    DAT3["Data Line DAT3"]
    SDIO_Device["SDIO Device (Wi-Fi, GPS, Camera)"]

    Host -->|CLK| SDIO_Device
    Host -->|CMD| SDIO_Device
    Host <-->|DAT0..3| SDIO_Device

	style SDIO_Device stroke:#fff,stroke-width:5px,font-weight:bold
````

---

## ⚙️ Где применяется

- Расширение возможностей мобильных устройств и встраиваемых систем.
    
- Подключение Wi-Fi, Bluetooth, GPS, камер и других периферийных модулей.
    
- Кардридеры с поддержкой SD и SDIO.
    
- Встроенные системы, IoT-устройства с компактным интерфейсом.
    
- Устройства с ограниченным количеством пинов и требованием универсальности.
    

---

## ✅ Преимущества

- Стандартизованный физический и протокольный интерфейс.
    
- Возможность подключения многофункциональных периферийных устройств.
    
- Высокая скорость передачи данных.
    
- Совместимость с существующими слотами SD.
    
- Поддержка управления устройством и передачи данных по одной шине.
    

---

## ❌ Недостатки

- Более сложный протокол, чем у обычных SD-карт.
    
- Требования к программной поддержке со стороны драйверов.
    
- Ограничения по длине линии и помехоустойчивости.
    
- Не все устройства и контроллеры поддерживают SDIO.
    
- В некоторых случаях ограничена максимальная пропускная способность.
    

---

## 🔗 Связанные технологии

[[SD Card]], [[SPI]], [[MMC]], [[I2C]], [[MCU]], [[SoC]], [[Wi-Fi Module]], [[GPS]], [[Camera Interface]], [[Host Controller]], [[Memory Card]]

---

## Резюме

SDIO — расширение стандарта SD, позволяющее подключать разнообразные периферийные устройства через универсальный разъём и протокол. Используется в мобильных и встраиваемых системах для добавления функций без изменения аппаратной платформы. Обеспечивает баланс между скоростью, универсальностью и компактностью интерфейса.

---

### Примеры кода

#### Linux: просмотр SDIO устройств

```bash
ls /sys/bus/sdio/devices/
dmesg | grep sdio
```

#### C (Linux kernel): регистрация SDIO драйвера

```c
#include <linux/mmc/sdio_func.h>

static int my_sdio_probe(struct sdio_func *func, const struct sdio_device_id *id) {
    // Инициализация устройства
    return 0;
}

static void my_sdio_remove(struct sdio_func *func) {
    // Очистка
}

static const struct sdio_device_id my_sdio_ids[] = {
    { SDIO_DEVICE(0x1234, 0x5678) }, // Vendor, Device ID
    { /* end: all zeroes */ },
};

static struct sdio_driver my_sdio_driver = {
    .name = "my_sdio_driver",
    .id_table = my_sdio_ids,
    .probe = my_sdio_probe,
    .remove = my_sdio_remove,
};

module_sdio_driver(my_sdio_driver);
```

---

**Источники:**  
SD Association Specifications, osdev.org, Linux kernel docs, Wikipedia, embedded.com, device manufacturer app notes.