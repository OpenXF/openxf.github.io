---
title: SoC (System on Chip)
aliases:
  - "System on Chip"
  - "SoC"
  - "система на кристалле"
  - "система на чипе"
tags:
  - "#soc"
  - "#hardware"
  - "#arch"
  - "#glossary"
path:
  - "hardware/soc"
---

## 📌 SoC

**SoC** (System on Chip, система на кристалле) — интегральная микросхема, включающая на одном чипе процессорные ядра ([[CPU]], [[GPU]]), оперативную память, контроллеры памяти и шин, периферийные устройства, контроллеры ввода-вывода, интерфейсы коммуникаций ([[PCIe]], [[USB]], [[Ethernet]]), мультимедийные блоки, часто — радиомодули ([[Wi-Fi]], [[Bluetooth]]). SoC полностью заменяет традиционную материнскую плату, объединяя почти всю функциональность компьютера или встраиваемой системы в едином корпусе.

---

## 🧠 Как работает

### Структура SoC

SoC реализует все основные подсистемы вычислительного устройства на одном кристалле:

- **CPU Subsystem**: Одно или несколько процессорных ядер ([[ARM]], [[x86]], [[RISC-V]]), кэш-память (обычно [[Cache#L1]], [[Cache#L2]]), MMU, FPU.
- **GPU/AI accelerator**: Встроенные блоки для графики и нейросетевых задач (Mobile GPU, [[DSP]], NPU, VPU).
- **Memory Controller**: Интегрированные контроллеры [[DDR]], [[LPDDR]], [[SRAM]], [[Flash]].
- **I/O Subsystem**: Аппаратные контроллеры [[USB]], [[SPI]], [[I2C]], [[UART]], [[PCIe]], [[SDIO]], [[Ethernet]].
- **Multimedia**: Кодеки видео/аудио, ISP (Image Signal Processor), MIPI-камера, дисплейный контроллер ([[HDMI]], [[DisplayPort]], [[DSI]]).
- **Radio**: Модули [[Wi-Fi]], [[Bluetooth]], [[GPS]], [[NFC]], [[LTE]] (в мобильных SoC).
- **Power Management**: Встроенные регуляторы питания ([[LDO]], [[DC-DC]]), PMIC.
- **Security**: Secure Boot, TrustZone, аппаратные криптомодули, eFuse, OTP.

### Схема SoC (совместима с Mermaid v8/v9/v10)

```mermaid
flowchart TB
    CPU["CPU Cores"]
    GPU["GPU / AI Accel."]
    DDR["DDR/LPDDR Controller"]
    SRAM["SRAM"]
    Flash["Flash/EEPROM"]
    Bus["High-Speed Bus (AXI/AHB)"]
    Periph["I/O Controllers"]
    USB["USB"]
    SPI["SPI"]
    I2C["I2C"]
    UART["UART"]
    ETH["Ethernet"]
    SDIO["SDIO"]
    WiFi["Wi-Fi"]
    BT["Bluetooth"]
    PMU["Power Mgmt"]
    Sec["Security"]
    Video["Video/Audio Codec"]
    ISP["ISP/Camera"]
    HDMI["HDMI/Display"]

    CPU --> Bus
    GPU --> Bus
    DDR --> Bus
    SRAM --> Bus
    Flash --> Bus
    Periph --> Bus
    Bus --> USB
    Bus --> SPI
    Bus --> I2C
    Bus --> UART
    Bus --> ETH
    Bus --> SDIO
    Bus --> Video
    Bus --> HDMI
    Bus --> ISP
    Bus --> WiFi
    Bus --> BT
    Bus --> Sec
    Bus --> PMU
````

---

## ⚙️ Где применяется

- **Смартфоны, планшеты, смарт-часы, IoT:** основа мобильных и портативных устройств (Qualcomm Snapdragon, Apple A/M, Samsung Exynos, MediaTek, Unisoc, HiSilicon Kirin).
    
- **Встраиваемые системы:** маршрутизаторы, ТВ-приставки, smart home, носимая электроника.
    
- **Автомобильные решения:** ADAS, бортовые компьютеры, медиацентры (NVIDIA Drive, NXP, TI Jacinto).
    
- **Промышленные и робототехника:** ПЛК, контроллеры, edge-компьютинг (NXP i.MX, ST STM32MP1, TI Sitara).
    
- **Персональные компьютеры:** Apple Silicon (M1/M2/M3), Raspberry Pi, Chromebook, ноутбуки.
    
- **Серверы и дата-центры:** облачные SoC (Ampere Altra, Amazon Graviton, Nvidia Grace, Alibaba Yitian).
    

---

## ✅ Преимущества

- **Компактность:** максимальная интеграция, снижение размеров, массы и числа компонентов.
    
- **Энергоэффективность:** оптимизация энергопотребления, единое управление питанием.
    
- **Стоимость:** снижение себестоимости BOM, минимизация разводки и числа внешних чипов.
    
- **Скорость передачи данных:** минимизация задержек за счёт on-chip шин (AXI, AHB, TCM).
    
- **Гибкость:** широкий набор интегрированных периферий, поддержка различных ОС и стеков.
    
- **Безопасность:** аппаратные security-блоки, root-of-trust, secure boot.
    
- **Масштабируемость:** возможно выпускать семейства SoC под разные задачи (Lite/Pro/Max).
    

---

## ❌ Недостатки

- **Ограниченность по апгрейду:** невозможно заменить CPU, GPU или память отдельно.
    
- **Сложность разработки:** требуется опыт в верификации, PPA, floorplanning, validation, EMI.
    
- **Температурные и тепловые ограничения:** ограничивает частоту и производительность.
    
- **Зависимость от вендора:** закрытые спецификации, NDA, ограниченный доступ к исходным кодам.
    
- **Уязвимость к сложным багам:** аппаратные баги сложно исправить, требуют workaround или ревизии чипа.
    
- **Сложности с драйверами и поддержкой ОС:** интеграция новых SoC в ядра Linux/Android требует времени.
    

---

## 🔗 Связанные технологии

[[CPU]], [[GPU]], [[FPGA]], [[AXI]], [[AHB]], [[DDR]], [[LPDDR]], [[SRAM]], [[Flash]], [[EEPROM]], [[USB]], [[I2C]], [[SPI]], [[UART]], [[PCIe]], [[Ethernet]], [[Wi-Fi]], [[Bluetooth]], [[HDMI]], [[DisplayPort]], [[DSP]], [[PMIC]], [[ISP]], [[VPU]], [[NPU]], [[Linux]], [[RTOS]]

---

## Резюме

SoC — основной строительный блок современной электроники: объединяет вычисления, память, интерфейсы, мультимедиа, радио и безопасность на одном кристалле. Даёт максимальную плотность и энергоэффективность при минимальных затратах, но накладывает ограничения по гибкости и апгрейду. Критичен для всего мобильного рынка, IoT, встраиваемых систем, бытовой электроники и новых поколений ПК.

---

### Примеры кода

#### C: низкоуровневое обращение к периферии SoC (MMIO)

```c
#define UART0_BASE 0x3F201000 // адрес UART для Raspberry Pi 3
#define UART0_DR   (*(volatile unsigned int*)(UART0_BASE + 0x00))

void uart_send(char c) {
    while (!(UART0_DR & 0x20)); // Ждём готовности
    UART0_DR = c;               // Пишем символ в UART
}
```

#### Linux: определение SoC через procfs

```bash
cat /proc/cpuinfo | grep Hardware
cat /proc/device-tree/model
```

#### Device Tree для SoC (фрагмент)

```dts
soc {
    compatible = "brcm,bcm2837";
    uart0: serial@3f201000 {
        compatible = "arm,pl011", "arm,primecell";
        reg = <0x3f201000 0x1000>;
        interrupts = <2 25>;
    };
};
```

---

**Источники:** ARM System Architectures, Linux kernel documentation, Silicon Vendor datasheets (Qualcomm, MediaTek, Broadcom, Apple, NXP, ST, TI), Википедия, osdev.org, habr.com, chipestimate.com, AnandTech, IEEE Xplore.