# scclog
The successor of C++ logger singleton in Modern C++

Because of legacy issues, we mainly supports C++17 in this project.
If you want to use C++20 or newer features please define _­_­cplusplus >= 202002L

# Considerations
- It is thread-safe when emit single log message, but it is not thread-safe to modify the log level of the logger.
- We prefer to use the syntax of [fmtlib](https://github.com/fmtlib/fmt) for string formatting.
- The variadic templates used by fmtlib can significantly increase compilation time.
    - However, if it can reduce compilation time and have minimal impact on runtime performance, then we would prefer it.
- We have also considered the support of std::format in C++20, but currently, the support of compilers is not yet widespread.
    - So we want to use macro guards to control the version of __cplusplus to be greater than 202002L.

# Usage features

Basic usage

```cpp
#include <scclog/scclog.hpp>

int main() {
    scclog::info("Hello, world!");
    scclog::warn("Hello, world!");
    scclog::error("Hello, world!");
    scclog::debug("Hello, world!");
    scclog::critical("Hello, world!");  // flush the log immediately
    
    // Get the singleton instance of scclog::Engine
    auto& singleton = scclog::Engine();

    singleton.flush();  // flush the log immediately

    // Set the default log level
    singleton.set_default_level(scclog::Level::Debug);
    // Default log level is scclog::Level::Info
    
    // Default pattern is "[%H:%M:%S] [%n] [{level}] {message}"
    // Set pattern
    singleton.set_pattern("[%H:%M:%S] [%n] [{level}] {message}");
    // Please refer to the [strftime](https://en.cppreference.com/w/cpp/chrono/c/strftime) documentation for the syntax of the pattern

    // Logging to file
    singleton.set_file("log.txt");  // Default log to log.txt
    singleton.set_file(scclog::Level::Debug, "debug_log.txt");  // Log debug message to debug_log.txt separately, and other messages are set as default
    // If all level are set separately, the default log file would not be set

    singleton.print_stdout(true);  // Default print to stdout

    return 0;
}
```

