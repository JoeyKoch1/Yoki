# Yoki

Yoki is a C++ library for interacting with the Counter-Strike 2 process. It provides abstractions for process management, memory access, pattern scanning, and common engine interfaces.

## Features

- Process attachment and lifetime management
- Read and write process memory
- Pattern scanning with wildcard support
- Module and interface lookup
- Entity list traversal
- Local player and weapon access
- Low level utilities for Windows process interaction

## Requirements

- Windows 10 or Windows 11
- C++17 or later
- CMake 3.20 or later
- A current Counter-Strike 2 offset dump such as `cs2-dumper`

## Building

```bash
git clone https://github.com/JoeyKoch1/yoki.git
cd yoki

cmake -B build
cmake --build build
```

## Example

```cpp
#include <yoki/yoki.hpp>

int main() {
    yoki::process game("cs2.exe");

    auto local =
        game.read<uintptr_t>(game.base() + offsets::dwLocalPlayer);

    int health =
        game.read<int>(local + offsets::m_iHealth);

    return 0;
}
```

## Basic Usage

Open the target process and query loaded modules.

```cpp
yoki::process game("cs2.exe");

auto client = game.module("client.dll");
auto engine = game.module("engine2.dll");
```

Read memory.

```cpp
int value = game.read<int>(address);
```

Write memory.

```cpp
game.write(address, value);
```

Pattern scan.

```cpp
auto address = game.pattern(
    "client.dll",
    "48 8B ?? ?? ?? ?? ?? 48 89"
);
```

## Notes

Yoki does not include game offsets. Keep offsets up to date with an external project such as `cs2-dumper`.

## License

MIT
