# MCP4728-DAC-HAL-Driver

## Overview

This project provides a driver implementation for the MCP4728 DAC (Digital-to-Analog Converter) using the STM32 Hardware Abstraction Layer (HAL). The driver facilitates communication between an STM32 microcontroller and the MCP4728 device over the I2C interface. It allows for configuration and control of the DAC channels, enabling precise analog output generation. It mimics all the functions given in the waveforms in the MCP4728 datasheet.

## Features

- Supports MCP4728 DAC communication via I2C
- Configurable voltage reference, gain, and power-down modes
- Full support for fast, single, multi, and sequential write operations
- General call commands for device reset and update
- Lightweight and efficient design
- Easy integration with STM32CubeMX generated code

## Prerequisites

- STM32 microcontroller (e.g., STM32F4, STM32F1, STM32L4 series)
- STM32CubeMX (for code generation and configuration)
- STM32 HAL library
- Keil MDK, IAR Embedded Workbench, or STM32CubeIDE (for development)

## Installation

1. **Generate Code with STM32CubeMX:**
   - Open STM32CubeMX and create a new project.
   - Select your STM32 microcontroller model.
   - Configure the I2C peripheral for communication with the MCP4728.
   - Generate the initialization code.

2. **Integrate the MCP4728 Driver:**
   - Copy the generated I2C-related files into your project directory.
   - Ensure the HAL library is included in your project.

3. **Add the MCP4728 Driver Source Files:**
   - Copy the provided MCP4728 driver source files (`mcp4728.c`, `mcp4728.h`) into your project’s source directory.
   - Include `mcp4728.h` in your main application file.


## Usage

### Channel Configuration

Configure the DAC channels by selecting the voltage reference, gain, and DAC values:

```c
ChannelConfig config;
config.vref = 0x0;  // Use VDD as reference voltage
config.gain = MCP4728_GAIN_1;  // Gain of 1x
config.val[0] = 2048;  // 12-bit DAC value for channel A
config.val[1] = 2048;  // 12-bit DAC value for channel B
config.val[2] = 2048;  // 12-bit DAC value for channel C
config.val[3] = 2048;  // 12-bit DAC value for channel D

mcp4728_vrefSelect(&hi2c1, config);
mcp4728_gainSelect(&hi2c1, config);
mcp4728_fastWrite(&hi2c1, config);
```

### Data Transmission

Use the provided API functions to send configuration and DAC values to the MCP4728:

```c
// Perform a single write operation on channel A
mcp4728_singleWrite(&hi2c1, config, MCP4728_CHANNEL_A);

// Perform a multi-write operation starting from channel B
mcp4728_multiWrite(&hi2c1, config, MCP4728_CHANNEL_B);
```

### General Call Commands

Send general call commands to reset or update the MCP4728:

```c
// Send a general reset command
mcp4728_generalCall(&hi2c1, MCP4728_GENERAL_RESET);

// Send a general software update command
mcp4728_generalCall(&hi2c1, MCP4728_GENERAL_SOFTWARE_UPDATE);
```

## API Reference

### Initialization

- `HAL_StatusTypeDef mcp4728_vrefSelect(I2C_HandleTypeDef *i2cHandler, ChannelConfig config)`: Selects the voltage reference for the MCP4728.
- `HAL_StatusTypeDef mcp4728_gainSelect(I2C_HandleTypeDef *i2cHandler, ChannelConfig config)`: Selects the gain for the MCP4728.

### Data Transmission

- `HAL_StatusTypeDef mcp4728_fastWrite(I2C_HandleTypeDef *i2cHandler, ChannelConfig config)`: Performs a fast write operation on the MCP4728.
- `HAL_StatusTypeDef mcp4728_singleWrite(I2C_HandleTypeDef *i2cHandler, ChannelConfig config, uint8_t channel)`: Performs a single write operation on a specified channel.
- `HAL_StatusTypeDef mcp4728_multiWrite(I2C_HandleTypeDef *i2cHandler, ChannelConfig config, uint8_t channel)`: Performs a multi-write operation starting from a specified channel.
- `HAL_StatusTypeDef mcp4728_sequentialWrite(I2C_HandleTypeDef *i2cHandler, ChannelConfig config, uint8_t channel)`: Performs a sequential write operation starting from a specified channel.

### General Call Commands

- `HAL_StatusTypeDef mcp4728_generalCall(I2C_HandleTypeDef *i2cHandler, uint8_t command)`: Sends a general call command to the MCP4728.


---
