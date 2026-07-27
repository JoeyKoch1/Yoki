# Yoki

A lightweight C++ library that simplifies Counter-Strike 2 game hacking by handling low-level process interaction so you can focus on building features.

## Features

- **Process management** — attach to `cs2.exe`, resolve base address, handle process lifecycle
- **Memory operations** — read/write process memory with clean RAII wrappers around `ReadProcessMemory` / `WriteProcessMemory`
- **Signature scanning** — AOB pattern scanning with wildcard support (`??`) for offset resolution across game updates
- **Interface traversal** — walk the entity list, get local player, enumerate weapons and players
- **VAC evasion helpers** — indirect syscalls, string obfuscation, and module hiding primitives

## Quick Start

```cpp
#include <yoki/yoki.hpp>

int main() {
    yoki::process cs2("cs2.exe");

    auto local = cs2.read<uintptr_t>(cs2.base() + offsets::dwLocalPlayer);
    int health = cs2.read<int>(local + offsets::m_iHealth);

    return 0;
}
```

## Requirements

- Windows 10 / 11
- C++17 or later
- CMake 3.20+
- A CS2 development offset dump (e.g., from `cs2-dumper`)

## Building

```bash
git clone https://github.com/JoeyKoch1/yoki.git
cd yoki
cmake -B build
cmake --build build
```

## Usage

Attach to the target process and read/write memory through the library's safe wrappers. Offset values are not bundled — use an external offset dump to keep them current with each CS2 patch.

```cpp
yoki::process game("cs2.exe");

uintptr_t client = game.module("client.dll");
uintptr_t engine = game.module("engine2.dll");
```

## License

MIT
