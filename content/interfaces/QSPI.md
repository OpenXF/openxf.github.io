---
title: QSPI (Quad SPI)
aliases:
  - "Quad SPI"
  - "QSPI"
  - "четырёхпроводный SPI"
  - "quad serial peripheral interface"
tags:
  - "#spi"
  - "#interface"
  - "#hardware"
  - "#flash"
  - "#glossary"
path:
  - "hardware/interface/qspi"
---

## 📌 QSPI

**QSPI** (Quad Serial Peripheral Interface, четырёхпроводный последовательный интерфейс) — расширение классического SPI, позволяющее передавать данные по четырём линиям данных параллельно вместо одной, что значительно увеличивает пропускную способность последовательного интерфейса. Используется для высокоскоростного обмена с энергонезависимой памятью ([[Flash]]) и другими периферийными устройствами.

---

## 🧠 Как работает

### Отличия от классического SPI

- **Количество линий данных:** вместо одной линии MISO и одной MOSI используется 4 линии данных (IO0..IO3).
- **Режимы передачи:**  
  - Однобитный режим (совместимость с SPI).  
  - Двухбитный (Dual SPI).  
  - Четырёхбитный (Quad SPI) — передача/приём 4 бит за такт.
- **Управление линиями:** стандартные сигналы управления (CS — Chip Select, CLK — Clock) сохраняются.
- **Высокая скорость:** увеличение пропускной способности примерно в 4 раза по сравнению с SPI при той же тактовой частоте.

### Принцип передачи

- По тактовому сигналу CLK синхронизируется передача 4 бит параллельно на линиях IO0–IO3.
- Каждый такт передаёт 4 бита, что позволяет достичь высоких скоростей передачи данных.
- Устройства, поддерживающие QSPI, требуют поддержки соответствующих протоколов.

### Схема QSPI (блок-схема)

```mermaid
flowchart TB
    MCU["MCU / Host Controller"]
    CS["Chip Select (CS)"]
    CLK["Clock (CLK)"]
    IO0["Data Line IO0"]
    IO1["Data Line IO1"]
    IO2["Data Line IO2"]
    IO3["Data Line IO3"]
    Flash["QSPI Flash Memory"]

    MCU -->|CS| Flash
    MCU -->|CLK| Flash
    MCU -->|IO0| Flash
    MCU -->|IO1| Flash
    MCU -->|IO2| Flash
    MCU -->|IO3| Flash

    style Flash stroke:#fff,stroke-width:5px,font-weight:bold
````

---

## ⚙️ Где применяется

- Высокоскоростная работа с внешними энергонезависимыми памятью (NOR Flash, SPI Flash).
    
- Загрузка прошивок, выполнение кода напрямую из внешней памяти (XIP — execute in place).
    
- Системы с ограниченным количеством выводов, требующие высокой пропускной способности.
    
- Мобильные устройства, сетевое оборудование, встраиваемые системы.
    
- Устройства с поддержкой быстрых загрузок, кешей, памяти программ.
    

---

## ✅ Преимущества

- Значительное увеличение скорости передачи по сравнению с классическим SPI.
    
- Совместимость с существующим SPI-протоколом в однобитном режиме.
    
- Уменьшение количества циклов передачи данных, снижение задержек.
    
- Возможность использования XIP — выполнение кода прямо из внешней памяти.
    
- Простота аппаратной реализации по сравнению с параллельными интерфейсами.
    

---

## ❌ Недостатки

- Требует поддержки на уровне периферии и памяти, сложнее реализация по сравнению с классическим SPI.
    
- Более сложная разводка печатных плат из-за дополнительных линий данных.
    
- Ограничения по длине линий и помехоустойчивости на высоких скоростях.
    
- Некоторые микроконтроллеры и устройства не поддерживают QSPI.
    
- Повышенная сложность протокола, требующая поддержки со стороны драйверов.
    

---

## 🔗 Связанные технологии

[[SPI]], [[I2C]], [[Flash]], [[NOR Flash]], [[XIP]], [[MCU]], [[SoC]], [[DMA]], [[Memory Mapped IO]], [[Serial Interface]]

---

## Резюме

QSPI — современное расширение классического SPI, позволяющее передавать данные по четырём линиям одновременно, что значительно увеличивает скорость обмена. Широко используется для быстрого взаимодействия с энергонезависимой памятью в встраиваемых системах и микроконтроллерах. Несмотря на усложнение интерфейса и требований к оборудованию, QSPI обеспечивает существенные преимущества по пропускной способности и эффективности.

---

### Примеры кода

#### C (STM32 HAL): инициализация и чтение из QSPI Flash

```c
#include "stm32f7xx_hal.h"

QSPI_HandleTypeDef hqspi;

void QSPI_Init(void) {
    hqspi.Instance = QUADSPI;
    hqspi.Init.ClockPrescaler = 1;
    hqspi.Init.FifoThreshold = 4;
    hqspi.Init.SampleShifting = QSPI_SAMPLE_SHIFTING_NONE;
    hqspi.Init.FlashSize = 23; // пример: 8 MB
    hqspi.Init.ChipSelectHighTime = QSPI_CS_HIGH_TIME_1_CYCLE;
    hqspi.Init.ClockMode = QSPI_CLOCK_MODE_0;
    hqspi.Init.FlashID = QSPI_FLASH_ID_1;
    hqspi.Init.DualFlash = QSPI_DUALFLASH_DISABLE;
    HAL_QSPI_Init(&hqspi);
}

uint8_t QSPI_ReadByte(uint32_t addr) {
    uint8_t data;
    QSPI_CommandTypeDef cmd = {0};
    cmd.Instruction = 0xEB; // Quad I/O Read command
    cmd.Address = addr;
    cmd.AddressSize = QSPI_ADDRESS_24_BITS;
    cmd.DummyCycles = 6;
    cmd.NbData = 1;
    cmd.DataMode = QSPI_DATA_4_LINES;
    cmd.AddressMode = QSPI_ADDRESS_4_LINES;
    cmd.InstructionMode = QSPI_INSTRUCTION_1_LINE;
    HAL_QSPI_Command(&hqspi, &cmd, HAL_QSPI_TIMEOUT_DEFAULT_VALUE);
    HAL_QSPI_Receive(&hqspi, &data, HAL_QSPI_TIMEOUT_DEFAULT_VALUE);
    return data;
}
```

#### Python: обращение к QSPI Flash через SPI с четырьмя линиями (теоретический пример)

```python
# В реальности часто нужно специализированное оборудование и драйверы
# Для примера — классический SPI с расширением QSPI

class QSPIFlash:
    def __init__(self, spi):
        self.spi = spi

    def read_quad(self, address, length):
        # Отправка команды чтения с использованием 4 линий данных
        # Настройки интерфейса должны поддерживать Quad SPI
        cmd = [0xEB] + [(address >> 16) & 0xFF, (address >> 8) & 0xFF, address & 0xFF] + [0x00]*6  # dummy cycles
        self.spi.write(cmd)
        data = self.spi.read(length)
        return data
```

---

**Источники:**  
STM32 HAL, Micron/Winbond QSPI Flash Datasheets, osdev.org, Wikipedia, manufacturer app notes, embedded.com articles, datasheets SPI/QSPI Flash.