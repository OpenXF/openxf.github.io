---
title: CPU (Central Processing Unit)
aliases:
  - "Central Processing Unit"
  - "Центральный процессор"
  - "процессор"
  - "ЦПУ"
tags:
  - "#cpu"
  - "#processor"
  - "#hardware"
  - "#architecture"
  - "#glossary"
path:
  - "hardware/cpu"
---

## 📌 CPU

**CPU** (Central Processing Unit, центральный процессор) — основной вычислительный элемент компьютерной системы, выполняющий программы за счёт последовательного или параллельного исполнения команд. CPU осуществляет обработку данных, управление работой других устройств, взаимодействует с памятью и вводом-выводом. Современные CPU — сложные многозадачные и многоядерные устройства с глубокой микроархитектурой, оптимизированные для максимальной производительности и энергоэффективности.

---

## 🧠 Как работает

### Общая архитектура CPU

CPU состоит из нескольких ключевых компонентов, обеспечивающих полный цикл исполнения инструкций:

1. **Устройство управления (CU, Control Unit):**
   - Координирует работу процессора, управляет последовательностью выполнения инструкций.
   - Распознаёт команды, генерирует управляющие сигналы для остальных блоков.

2. **Арифметико-логическое устройство (ALU):**
   - Выполняет арифметические и логические операции (сложение, вычитание, логические И, ИЛИ, сдвиги и др.).
   - Взаимодействует с регистрами и шиной данных.

3. **Регистр общего назначения (GPR):**
   - Быстрая память внутри CPU для хранения промежуточных данных.
   - Обеспечивает мгновенный доступ для ALU и CU.

4. **Счётчик команд (PC, Program Counter):**
   - Хранит адрес следующей инструкции для исполнения.
   - Обеспечивает последовательность выполнения программ.

5. **Декодер инструкций:**
   - Преобразует машинный код в управляющие сигналы, распознаёт типы и параметры инструкций.

6. **Кэш-память:**
   - Высокоскоростная память, хранящая часто используемые данные и инструкции, снижая задержки доступа к основной памяти.

7. **Шины и интерфейсы:**
   - Передают данные и управляющие сигналы между компонентами CPU и внешними устройствами.

---

### Цикл исполнения инструкции

- **Fetch (выборка):** загрузка инструкции из памяти по адресу PC.
- **Decode (декодирование):** анализ инструкции и определение необходимых операций.
- **Execute (выполнение):** выполнение операции ALU или вызов других модулей.
- **Memory Access (доступ к памяти):** чтение или запись данных, если требуется.
- **Write Back (запись результата):** сохранение результата операции в регистры.

---

### Микроархитектура

- Современные CPU используют конвейерную обработку, разделяя цикл исполнения на несколько стадий.
- Используют техники суперскалярности, параллельного исполнения и предсказания ветвлений.
- Включают специализированные блоки: FPU (числительный сопроцессор), SIMD-модули, блоки предсказания ветвлений.

---

### Большая блок-схема CPU

```mermaid
flowchart TB
    subgraph CPU["Central Processing Unit"]
        PC["Program Counter (PC)"]
        InstructionCache["Instruction Cache (L1 I-Cache)"]
        InstructionDecoder["Instruction Decoder"]
        ControlUnit["Control Unit (CU)"]
        RegisterFile["Register File (GPR)"]
        ALU["Arithmetic Logic Unit (ALU)"]
        FPU["Floating Point Unit (FPU)"]
        LoadStoreUnit["Load/Store Unit"]
        DataCache["Data Cache (L1 D-Cache)"]
        L2Cache["L2 Cache"]
        BranchPredictor["Branch Predictor"]
        PipelineControl["Pipeline Control"]
        MMU["Memory Management Unit (MMU)"]
        BusInterface["Bus Interface Unit"]
    end

    PC --> InstructionCache
    InstructionCache --> InstructionDecoder
    InstructionDecoder --> ControlUnit
    ControlUnit --> ALU
    ControlUnit --> FPU
    ControlUnit --> LoadStoreUnit
    ALU --> RegisterFile
    FPU --> RegisterFile
    LoadStoreUnit --> DataCache
    DataCache --> L2Cache
    BranchPredictor --> InstructionDecoder
    PipelineControl --> InstructionDecoder
    PipelineControl --> ALU
    PipelineControl --> FPU
    PipelineControl --> LoadStoreUnit
    MMU --> DataCache
    MMU --> InstructionCache
    BusInterface --> L2Cache
    BusInterface --> SystemBus["System Bus / Memory"]
````

---

## ⚙️ Где применяется

- В персональных компьютерах, ноутбуках, серверах.
    
- В мобильных устройствах (смартфоны, планшеты).
    
- В встраиваемых системах, микроконтроллерах, IoT устройствах.
    
- В высокопроизводительных вычислительных системах (HPC).
    
- В игровых консолях, мультимедийных устройствах.
    

---

## ✅ Преимущества современных CPU

- Высокая скорость обработки благодаря конвейерам и параллелизму.
    
- Универсальность для широкого спектра задач.
    
- Поддержка сложных вычислений, мультимедийных и криптографических операций.
    
- Расширяемость за счёт многоядерности.
    
- Оптимизация энергопотребления и тепловыделения.
    

---

## ❌ Недостатки

- Сложность архитектуры и разработки.
    
- Ограничение по тепловому пакету и энергопотреблению.
    
- Вызовы при масштабировании частоты из-за физических ограничений.
    
- Накладные расходы на управление кэшами, конвейерами, ветвлениями.
    

---

## 🔗 Связанные технологии

[[ALU]], [[CU]], [[Cache]], [[Pipeline]], [[FPU]], [[MMU]], [[Register File]], [[Branch Prediction]], [[Superscalar]], [[Multicore]], [[Hyper-Threading]], [[Out-of-order Execution]], [[Memory Hierarchy]], [[Bus]]

---

## Резюме

CPU — сердце любой вычислительной системы, реализующее обработку инструкций, управление вычислениями и взаимодействие с памятью и периферией. Современные CPU — сложные высокопроизводительные устройства с глубокой микроархитектурой и поддержкой параллельного исполнения. Их эффективность напрямую влияет на производительность и энергоэффективность всей системы.

---

## Примеры кода

### C: работа с регистрами CPU (эмуляция)

```c
#include <stdio.h>

typedef struct {
    unsigned int eax, ebx, ecx, edx;
} CPU_Registers;

void add(CPU_Registers* cpu, unsigned int a, unsigned int b) {
    cpu->eax = a + b;
}

int main() {
    CPU_Registers cpu = {0};
    add(&cpu, 5, 10);
    printf("EAX: %u\n", cpu.eax); // 15
    return 0;
}
```

### Системный вызов: получение информации о CPU в Linux

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/sysinfo.h>

int main() {
    printf("CPU count: %ld\n", sysconf(_SC_NPROCESSORS_ONLN));
    return 0;
}
```

### Ассемблер: простая арифметика на [[x86]]

```assembly
section .data
    a dd 5
    b dd 10

section .text
    global _start
_start:
    mov eax, [a]      ; загрузить a в EAX
    add eax, [b]      ; сложить b
    mov [a], eax      ; сохранить результат
    mov eax, 1        ; syscall: exit
    int 0x80
```

### Пример простейшего ALU на Verilog

```verilog
module simple_alu(
    input [3:0] a,
    input [3:0] b,
    input [1:0] op,
    output reg [3:0] result
);
    always @(*) begin
        case(op)
            2'b00: result = a + b;
            2'b01: result = a - b;
            2'b10: result = a & b;
            2'b11: result = a | b;
            default: result = 4'b0000;
        endcase
    end
endmodule
```

### Псевдокод выполнения инструкции

```text
fetch:
    instruction = memory[PC]
    PC = PC + 1

decode:
    parse instruction into opcode and operands

execute:
    perform operation (e.g., ALU, memory access)

memory_access:
    if load/store then access memory

write_back:
    write results into registers
```

---

**Источники:**  
Hennessy & Patterson "Computer Architecture", Intel and AMD Manuals, ARM Architecture Reference, osdev.org, Wikipedia, IEEE papers, CPU vendor documentation.