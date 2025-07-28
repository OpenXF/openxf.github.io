---
title: Pipeline
aliases:
  - "Instruction Pipeline"
  - "конвейер"
  - "конвейерная обработка"
  - "pipeline"
tags:
  - "#pipeline"
  - "#cpu"
  - "#microarchitecture"
  - "#performance"
  - "#glossary"
path:
  - "hardware/pipeline"
---

## 📌 Pipeline

**Pipeline** (конвейер) — техника микроархитектурной организации процессора, при которой выполнение инструкции разбивается на последовательность этапов (стадий), позволяющих обрабатывать несколько инструкций одновременно, каждая на своём этапе. Это повышает производительность и пропускную способность процессора, увеличивая количество инструкций, обрабатываемых за такт (IPC).

---

## 🧠 Как работает

### Основная идея

Вместо последовательного полного выполнения инструкции, процессор разделяет её выполнение на несколько стадий (например, Fetch, Decode, Execute, Memory, Writeback). Каждая стадия выполняет свою часть работы над инструкцией. На каждом такте конвейер смещает инструкции на следующий этап, позволяя параллельно работать с несколькими инструкциями на разных стадиях.

### Классическая 5-ступенчатая архитектура MIPS

1. **IF (Instruction Fetch)** — выборка инструкции из памяти по адресу в [[PC]].
2. **ID (Instruction Decode & Register Fetch)** — декодирование инструкции и чтение регистров.
3. **EX (Execute)** — выполнение арифметической/логической операции или вычисление адреса.
4. **MEM (Memory access)** — обращение к памяти (чтение/запись данных).
5. **WB (Write Back)** — запись результата операции обратно в регистры.

### Схема простого конвейера

```mermaid
flowchart LR
    IF["IF: Instruction Fetch"]
    ID["ID: Instruction Decode"]
    EX["EX: Execute"]
    MEM["MEM: Memory Access"]
    WB["WB: Write Back"]

    IF --> ID --> EX --> MEM --> WB
````

### Пример работы

- На такте 1: инструкция 1 — на IF
    
- На такте 2: инструкция 1 — на ID, инструкция 2 — на IF
    
- На такте 3: инструкция 1 — EX, инструкция 2 — ID, инструкция 3 — IF  
    ... и так далее.
    

### Проблемы и решения

- **Hazards (Конфликты):** ситуации, когда одна инструкция зависит от результата другой, или происходит обращение к памяти/ресурсам, вызывающие задержки.
    
    - **Data hazard:** зависимость данных между инструкциями.
        
    - **Control hazard:** неопределённость перехода (ветвления).
        
    - **Structural hazard:** конфликт за аппаратные ресурсы.
        
- **Техника преодоления конфликтов:**
    
    - **Forwarding (Data bypassing):** передача результатов напрямую между стадиями.
        
    - **Stalls (Bubble insertion):** временная приостановка конвейера.
        
    - **Branch prediction:** предсказание переходов для уменьшения задержек.
        
    - **Out-of-order execution:** выполнение инструкций вне исходного порядка с сохранением семантики.
        

### Расширенная схема с несколькими инструкциями

```mermaid
gantt
    title Pipeline execution timeline
    dateFormat  X
    axisFormat  %d
    section IF
    Inst1 :done, 1, 1
    Inst2 :done, 2, 1
    Inst3 :done, 3, 1
    Inst4 :done, 4, 1
    Inst5 :done, 5, 1
    section ID
    Inst1 :done, 2, 1
    Inst2 :done, 3, 1
    Inst3 :done, 4, 1
    Inst4 :done, 5, 1
    Inst5 :done, 6, 1
    section EX
    Inst1 :done, 3, 1
    Inst2 :done, 4, 1
    Inst3 :done, 5, 1
    Inst4 :done, 6, 1
    Inst5 :done, 7, 1
    section MEM
    Inst1 :done, 4, 1
    Inst2 :done, 5, 1
    Inst3 :done, 6, 1
    Inst4 :done, 7, 1
    Inst5 :done, 8, 1
    section WB
    Inst1 :done, 5, 1
    Inst2 :done, 6, 1
    Inst3 :done, 7, 1
    Inst4 :done, 8, 1
    Inst5 :done, 9, 1
```

---

## ⚙️ Где применяется

- Во всех современных [[CPU]] и [[MCU]] — от простых однопоточных до многоядерных процессоров.
    
- В [[GPU]] для обработки множества потоков параллельно.
    
- В цифровой обработке сигналов ([[DSP]]) и специализированных ускорителях.
    
- В конвейерных контроллерах, сетевых устройствах, микросхемах обработки потоков данных.
    
- В микроархитектуре большинства RISC и CISC процессоров (MIPS, ARM, x86, RISC-V).
    

---

## ✅ Преимущества

- Существенное увеличение пропускной способности процессора без роста тактовой частоты.
    
- Увеличение IPC (инструкций на такт).
    
- Эффективное использование ресурсов процессора, повышение общей производительности.
    
- Гибкость расширения (увеличение числа стадий, суперскалярность).
    
- Улучшение энергетической эффективности за счёт оптимизации цикла исполнения.
    

---

## ❌ Недостатки

- Усложнение архитектуры: требуется дополнительные блоки управления, буферы, предсказатели ветвлений.
    
- Появление конфликтов (hazards), требующих затрат на их разрешение (stalls, forwarding).
    
- Возрастание латентности выполнения отдельных инструкций.
    
- Повышенные требования к проектированию и отладке.
    
- Увеличение площади и энергопотребления микросхемы.
    

---

## 🔗 Связанные технологии

[[CPU]], [[Microarchitecture]], [[Hazard]], [[Forwarding]], [[Branch Prediction]], [[Superscalar]], [[Out-of-order Execution]], [[Pipeline#Stages]], [[Cache]], [[ALU]], [[Register File]]

---

## Резюме

Pipeline — ключевая микроархитектурная технология, позволяющая выполнять несколько инструкций параллельно на разных стадиях обработки. Она существенно повышает производительность процессоров и других вычислительных устройств, снижая простой оборудования и увеличивая throughput. Для эффективной работы требуется борьба с конфликтами, управление ветвлениями и сложный контроллер, что увеличивает сложность и стоимость разработки, но оправдано значительным ростом производительности.

---

### Примеры кода

#### Пример программной модели 5-ступенчатого конвейера (на C)

```c
#include <stdio.h>

typedef enum {IF, ID, EX, MEM, WB} Stage;

typedef struct {
    int instruction;
    Stage stage;
} PipelineRegister;

#define PIPELINE_DEPTH 5

int main() {
    PipelineRegister pipeline[PIPELINE_DEPTH] = {0};
    int instructions[] = {10, 20, 30, 40, 50};
    int cycle = 0, num_instructions = 5;

    while (cycle < num_instructions + PIPELINE_DEPTH - 1) {
        printf("Cycle %d: ", cycle + 1);
        for (int i = PIPELINE_DEPTH - 1; i > 0; i--) {
            pipeline[i] = pipeline[i - 1];
        }
        pipeline[0].instruction = (cycle < num_instructions) ? instructions[cycle] : 0;
        pipeline[0].stage = IF;

        for (int i = 0; i < PIPELINE_DEPTH; i++) {
            if (pipeline[i].instruction != 0)
                printf("[Instr %d @ stage %d] ", pipeline[i].instruction, i + 1);
        }
        printf("\n");
        cycle++;
    }
    return 0;
}
```

---

**Источники:**  
Hennessy & Patterson "Computer Architecture", Intel SDM, ARM Architecture Reference Manual, osdev.org, habr.com, Wikipedia, IEEE papers, MIPS architecture manuals.