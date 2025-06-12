# CPUmon

![GitHub Tag](https://img.shields.io/github/v/tag/the-krew/cpumon?style=for-the-badge&label=release)

cpumon is a lightweight Go library for CPU monitoring on Linux systems. It provides real-time CPU usage, frequency, and model information using `/proc` and `/sys` interfaces.

## Features

- Reads per-core and aggregate CPU usage from `/proc/stat`
- Retrieves CPU frequency and model information from `/proc/cpuinfo`
- Simple API for listing and refreshing CPU data
- Optional internal logging with configurable log level and output
- Designed for Linux servers and embedded systems
- Built-in `OnUpdate` callback for event-driven updates

## Installation

Add cpumon to your project using:

```sh
go get github.com/the-krew/cpumon
```

## Usage

```go
package main

import (
    "fmt"
    "github.com/the-krew/cpumon"
    "github.com/kopytkg/golog"
)

func main() {
    // Configure internal logging
    cpumon.FILELOG = golog.DISABLED
    cpumon.CLILOG = golog.DISABLED
    cpumon.LOGLEVEL = golog.SILENT

    // Create API instance
    api := cpumon.NewCPUAPI()

    // Access CPU information
    fmt.Println("Model:", api.CPU.ModelName)
    fmt.Println("Frequencies (MHz):", api.CPU.Freq)
    fmt.Println("Usage (%):", api.CPU.Usage)

    // Set OnUpdate callback to handle refresh events
    api.OnUpdate = func() {
        // Example: print updated usage
        fmt.Println("Updated CPU usage:", api.CPU.Usage)
    }

    // Keep the program running to see updates
    select {}
}
```

## License

See [LICENSE](https://github.com/The-Krew/cpumon?tab=CC0-1.0-1-ov-file) for details.

