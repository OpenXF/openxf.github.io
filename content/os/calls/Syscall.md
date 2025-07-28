---
title: Syscall (System Call)
aliases:
  - "System Call"
  - "Syscall"
  - "системный вызов"
  - "системные вызовы"
  - "syscalls"
tags:
  - "#syscall"
  - "#os"
  - "#api"
  - "#glossary"
path:
  - "os/syscall"
---

## 📌 Syscall

**Syscall** (System Call, системный вызов) — основной механизм обращения прикладного ПО к функциям ядра операционной системы: управление файлами, памятью, устройствами, сетями, процессами. Системный вызов обеспечивает безопасный переход из пользовательского режима ([[User mode]]) в привилегированный режим ([[Kernel mode]]) для выполнения сервисных функций, недоступных напрямую.

---

## 🧠 Как работает

### Механика системного вызова

1. **Пользовательское приложение** готовит параметры для вызова (обычно через регистры или стек).
2. Генерируется особая инструкция процессора (например, `int 0x80`, `syscall`, `svc`), вызывающая аппаратное переключение в режим ядра ([[Trap]]).
3. Ядро ОС получает номер системного вызова (Syscall Number) и параметры, выполняет необходимую функцию (например, [[Read]], [[Write]], [[Open]], [[Close]], [[Fork]], [[Exec]], [[Mmap]]).
4. Результат возвращается обратно в приложение, выполнение продолжается в пользовательском режиме.

### Классический цикл

- **User Mode:** выполняется приложение, генерирует syscall.
- **Trap/Interrupt:** CPU переключает в Kernel Mode.
- **Kernel Mode:** ОС выполняет обработку syscall.
- **Return:** возвращение управления и результата.

### Архитектурные детали

- Номера системных вызовов фиксируются ABI (см. [[Linux#Syscall table]], [[x86#Syscall]], [[ARM#Syscall]]).
- На современных архитектурах есть специализированные инструкции:  
  - [[x86]]: `int 0x80`, `syscall`, `sysenter`
  - [[ARM]]: `svc` (Supervisor Call)
  - [[RISC-V]]: `ecall`
- Стандартный путь: параметры кладутся в определённые регистры (например, [[x86#rax]], [[x86#rdi]], [[x86#rsi]], ...), номер syscall — в отдельный регистр.

### Схема обработки системного вызова:

```mermaid
flowchart TB
    App["User App"]
    Params["Параметры (регистры/стек)"]
    Trap["Инструкция syscall/trap"]
    Kernel["Kernel (ОС)"]
    Handler["Syscall Handler"]
    Result["Результат"]
    App2["User App (продолжение)"]

    App --> Params
    Params --> Trap
    Trap --> Kernel
    Kernel --> Handler
    Handler --> Result
    Result --> App2
````

---

## ⚙️ Где применяется

- Все современные ОС: [[Linux]], [[Windows]], [[macOS]], [[FreeBSD]], [[RTOS]] и др.
    
- Вызовы файловой системы ([[Read]], [[Write]], [[Open]], [[Close]]), управление процессами ([[Fork]], [[Exec]], [[Wait]]), памятью ([[Mmap]], [[Brk]]), IPC ([[Pipe]], [[Socket]], [[Msgqueue]]), сетевые стеки.
    
- Контроль безопасности: sandbox, ограничения по [[SELinux]], [[AppArmor]], [[Seccomp]].
    

---

## ✅ Преимущества

- **Изоляция:** пользовательский код не может напрямую обращаться к критическим ресурсам — безопасность ядра.
    
- **Стандартизация:** унифицированный интерфейс работы с ОС и железом.
    
- **Гибкость:** ядро может контролировать права, фильтровать и логировать вызовы.
    
- **Кроссплатформенность:** слой совместимости для разных CPU/архитектур.
    

---

## ❌ Недостатки

- **Задержки:** каждый переход между режимами (user↔kernel) увеличивает время отклика (context switch, overhead).
    
- **Ограниченная производительность:** массовые syscalls (например, [[read]]/[[write]] в цикле) замедляют I/O heavy приложения.
    
- **Платформозависимость:** таблица номеров и calling convention меняется между платформами.
    
- **Эксплуатация:** возможны атаки через некорректные параметры syscall (например, [[BOF]], [[TOCTOU]], race condition).
    
- **Ограниченная гибкость:** только то, что реализовано в ядре, доступно через syscall-интерфейс.
    

---

## 🔗 Связанные технологии

[[Kernel]], [[User mode]], [[Kernel mode]], [[Trap]], [[Interrupt]], [[x86#Syscall]], [[ARM#Syscall]], [[Linux#Syscall table]], [[Mmap]], [[IO]], [[Pipe]], [[Socket]], [[Seccomp]], [[AppArmor]], [[SELinux]], [[RTOS]]

---

## Резюме

Syscall — фундаментальный механизм взаимодействия приложений и ядра, позволяющий использовать системные ресурсы строго контролируемым образом. Все доступные внешнему коду функции ОС реализованы через syscalls. Переключение режима, защита памяти и контроль доступа обеспечивают безопасность, но влияют на производительность. Для современных OS — это критически важный компонент любой архитектуры.

---

### Примеры кода

#### Прямой syscall на x86-64 Linux (ассемблер)

```assembly
section .data
    msg db "Hello, syscall!", 10
    len equ $-msg

section .text
    global _start

_start:
    mov rax, 1        ; syscall number (sys_write)
    mov rdi, 1        ; file descriptor (stdout)
    mov rsi, msg      ; pointer to message
    mov rdx, len      ; message length
    syscall           ; invoke kernel

    mov rax, 60       ; syscall number (sys_exit)
    xor rdi, rdi      ; exit code 0
    syscall
```

#### C: использование системного вызова через стандартную библиотеку

```c
#include <unistd.h>
#include <sys/syscall.h>

int main() {
    const char* msg = "Hello, syscall!\n";
    syscall(SYS_write, 1, msg, 15); // SYS_write = номер системного вызова
    return 0;
}
```

#### C: чтение списка системных вызовов из /proc

```c
#include <stdio.h>

int main() {
    FILE* f = fopen("/proc/sys/kernel/osrelease", "r");
    char buf[256];
    fgets(buf, sizeof(buf), f);
    printf("OS Release: %s", buf);
    fclose(f);
    return 0;
}
```

---

**Источники:** Linux man-pages, Intel® SDM, ARM Architecture Reference Manual, osdev.org, Википедия, kernel.org, habr.com, документация POSIX, исходники glibc/musl.