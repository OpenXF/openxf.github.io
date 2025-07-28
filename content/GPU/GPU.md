---
title: GPU (Graphics Processing Unit)
aliases:
  - "Graphics Processing Unit"
  - "графический процессор"
  - "GPU"
  - "видеочип"
  - "графический ускоритель"
tags:
  - "#gpu"
  - "#hardware"
  - "#graphics"
  - "#glossary"
path:
  - "hardware/gpu"
---

## 📌 GPU

**GPU** (Graphics Processing Unit, графический процессор) — специализированный высокопараллельный процессор для выполнения графических, визуальных и вычислительных задач. Отличается архитектурой, заточенной под массовые параллельные вычисления, работу с пикселями, вершинами, шейдерами, массивами данных, матрицами, в отличие от универсального [[CPU]]. В современных вычислительных системах используется как для 3D-графики, так и для GPGPU-задач ([[CUDA]], [[OpenCL]], [[Vulkan]] compute).

---

## 🧠 Как работает

### Архитектура GPU

- **Многоядерность:** тысячи (до десятков тысяч) вычислительных ALU/FP-кластеров ([[CUDA core]], [[Stream processor]], [[SM]], [[EU]]).
- **Параллельные движки:** мультипроцессоры с общим планировщиком (Scheduler), контроллеры памяти, текстурные блоки, растеризаторы, блоки геометрии и вывод кадров.
- **Встроенные SRAM-кеши:** [[Cache#L1]], [[Cache#L2]], [[Register File]], Shared Memory, Texture Cache, иногда [[Cache#L3]].
- **Контроллер видеопамяти:** работа с [[GDDR]], [[HBM]], [[DDR]], поддержка мультиканального доступа.
- **Управление контекстами:** отдельные блоки управления задачами, очередями и синхронизацией.
- **Разделение памяти:** VRAM (GPU-only), UMA (Unified Memory Architecture), NVLink, PCIe, [[DMA]], BAR.

### Взаимодействие с системой

- [[CPU]] через [[PCIe]] и [[DMA]] или высокоскоростные шины передаёт команды, данные и получает результаты.
- Видеодрайверы и API ([[OpenGL]], [[DirectX]], [[Vulkan]], [[CUDA]], [[OpenCL]]) формируют задачи, которые поступают в Command Processor GPU.
- Command Processor разбирает задачи, отправляет их на графические и вычислительные кластеры.

#### Подсистемы GPU:

- **Command Processor** — разбор и планирование команд.
- **Shader Cores** — исполнение программ (шейдеров, compute kernels).
- **Texture Units** — работа с текстурами и фильтрацией.
- **ROP (Raster Operation Processors)** — вывод и смешивание пикселей, final frame.
- **Memory Controller** — работа с видеопамятью, кешами, акселерация обмена.
- **Display Engine** — вывод изображения на дисплей.
- **Video Codec Blocks** — аппаратное кодирование/декодирование ([[NVDEC]], [[NVENC]], [[VCE]], [[UVD]]).

### Расширенная схема GPU в системе (с пояснениями)

```mermaid
flowchart TB
    CPU["CPU"]
    PCIe["PCIe"]
    GPU["GPU"]
    VRAM["VRAM (GDDR/HBM)"]
    Display["Display/Monitor"]

    CPU -->|Data, commands| PCIe
    PCIe -->|DMA, commands| GPU
    GPU -->|Frame/Video Out| Display
    GPU -->|VRAM bus| VRAM

    CommandProc["Command Processor"]
    Shader1["Shader Cluster #1"]
    ShaderN["Shader Cluster #N"]
    Tex["Texture Units"]
    ROP["ROP"]
    L2["Cache L2"]
    MC["Memory Controller"]

    GPU --> CommandProc
    CommandProc --> Shader1
    CommandProc --> ShaderN
    Shader1 --> Tex
    ShaderN --> Tex
    Tex --> ROP
    ROP --> L2
    L2 --> MC
    MC --> VRAM
````

#### Расширенная sequence-схема для GPGPU

```mermaid
sequenceDiagram
    participant CPU
    participant GPU
    participant VRAM

    CPU->>GPU: Передача задачи (kernel), параметров
    CPU->>VRAM: Копирование данных
    GPU->>VRAM: Чтение/запись данных (kernels, массивы)
    GPU->>CPU: Передача результата/сигнал завершения
```

---

## ⚙️ Где применяется

- Графические станции, игровые ПК, серверы, [[Data Center]], HPC, [[AI accelerator]], системах реального времени, [[Embedded]], automotive, [[FPGA]] с видеокластерами.
    
- Мобильные устройства (мобильные [[GPU]]: [[Adreno]], [[Mali]], [[PowerVR]], [[Apple GPU]]).
    
- Видеокарты ([[PCIe]]-расширения), встроенные GPU ([[SoC]], [[APU]]).
    
- Не только для 3D-графики, но и для параллельных вычислений ([[Deep Learning]], [[Scientific computing]], [[Physics simulation]]).
    
- Видеоаналитика, рендеринг, виртуализация (GPU Passthrough), VDI, стриминг, видео-кодирование.
    

---

## ✅ Преимущества

- Высокая параллельная производительность — тысячи потоков одновременно.
    
- Специализированные блоки (шейдеры, растеризаторы, матричные множители).
    
- Высокая пропускная способность VRAM ([[GDDR6]], [[HBM2]]).
    
- Масштабируемость (multi-GPU, NVLink, PCIe Gen5).
    
- Аппаратная поддержка видео, AI, графических API.
    
- Эффективность для [[Deep Learning]], [[Big Data]], ML/AI, крипто- и рендер-вычислений.
    
- Относительная независимость от типа CPU и ОС.
    

---

## ❌ Недостатки

- Сложность программирования и дебага (шейдеры, параллелизм, синхронизация).
    
- Высокое энергопотребление и тепловыделение.
    
- Ограничение на тип и объём памяти (VRAM ≠ RAM), неуниверсальность для “обычных” программ.
    
- Большие задержки на передачу данных CPU↔GPU (PCIe bottleneck).
    
- Уязвимость к атакам по сторонним каналам (например, уязвимости через видеопамять, DMA).
    
- Зависимость от драйверов, проприетарных спецификаций (Nvidia, AMD, Intel).
    

---

## 🔗 Связанные технологии

[[CPU]], [[PCIe]], [[GDDR]], [[HBM]], [[SRAM]], [[VRAM]], [[AI accelerator]], [[FPGA]], [[OpenGL]], [[Vulkan]], [[CUDA]], [[OpenCL]], [[DirectX]], [[SoC]], [[Embedded]], [[DMA]], [[NUMA]], [[Memory Bus]], [[DisplayPort]], [[HDMI]], [[NVENC]], [[NVDEC]], [[Tensor Core]], [[Stream processor]], [[SM]], [[APU]]

---

## Резюме

GPU — высокопараллельный вычислительный процессор для графики и вычислений, реализующий архитектуру с тысячами кластеров, сложной памятью и ускорителями спецзадач. Используется везде, где критична параллельная обработка (от 3D-графики до AI и HPC). Отличается собственной иерархией кешей, отдельной VRAM, мощными контроллерами памяти и специализированными API. Главные минусы: сложность, энергопотребление, сложность интеграции и ограничения по памяти.

---

### Примеры кода

#### CUDA: простейшее суммирование на GPU

```cuda
__global__ void sum_array(int *a, int *b, int *c, int n) {
    int idx = threadIdx.x + blockDim.x * blockIdx.x;
    if (idx < n)
        c[idx] = a[idx] + b[idx];
}

int main() {
    // Аллокация, копирование, запуск ядра — опущено для краткости
    sum_array<<<grid, block>>>(a_dev, b_dev, c_dev, n);
}
```

#### OpenCL: сложение массивов на GPU

```c
__kernel void vec_add(__global int* a, __global int* b, __global int* c) {
    int id = get_global_id(0);
    c[id] = a[id] + b[id];
}
```

#### OpenGL GLSL: вычисление цвета пикселя

```glsl
#version 330 core
out vec4 FragColor;
in vec2 TexCoord;
uniform sampler2D tex;

void main() {
    FragColor = texture(tex, TexCoord) * 0.8 + vec4(0.2, 0.2, 0.2, 1.0);
}
```

#### Получение информации о GPU в Linux

```bash
lspci | grep VGA
glxinfo | grep "OpenGL renderer"
nvidia-smi
```

---

**Источники:** NVIDIA GPU Architecture Whitepaper, AMD Graphics Core Next, Intel Xe, Khronos OpenCL/OpenGL/Vulkan, Википедия, osdev.org, habr.com, документация CUDA, спецификации PCIe/GDDR/HBM.