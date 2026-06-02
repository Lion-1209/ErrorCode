# ErrorCode

English | [中文](README.md)

A unified error code definition library for embedded C programming.

## Overview

This library provides a structured error code system for standardizing return value handling in embedded C projects. The layered design gives each error code a clear module affiliation and error category, making issue localization and debugging straightforward.

## Error Code Design

### Design Principles

1. **Layered Structure**: `Module(8bit) | Category(4bit) | Specific Error(12bit)`
2. **Zero is Success**: `0` indicates success, non-zero indicates an error
3. **Uniqueness**: Each error code is globally unique
4. **Extensibility**: Ample module and error code space is reserved

### Error Code Format

```
0xMCCCEEEE
│ │   │
│ │   └─ EEEE: Specific error (12 bit, 0x000-0xFFF)
│ └───── CC:   Category      (4 bit, 0x0-0xF)
└─────── M:    Module        (8 bit, 0x00-0xFF)
```

### Module Definitions

| Module | Value | Description |
|--------|-------|-------------|
| `APP_ERR_MODULE_SYSTEM`   | 0x00 | General system |
| `APP_ERR_MODULE_COMM`     | 0x01 | Communication |
| `APP_ERR_MODULE_PROTOCOL` | 0x02 | Protocol handling |
| `APP_ERR_MODULE_UART`     | 0x03 | UART driver |
| `APP_ERR_MODULE_TIMER`    | 0x04 | Timer |
| `APP_ERR_MODULE_IO`       | 0x05 | I/O control |
| `APP_ERR_MODULE_MEMORY`   | 0x06 | Memory management |
| `APP_ERR_MODULE_TASK`     | 0x07 | Task scheduling |
| `APP_ERR_MODULE_SENSOR`   | 0x08 | Sensor |
| `APP_ERR_MODULE_USER`     | 0xFF | User-defined |

### Category Definitions

| Category | Value | Description |
|----------|-------|-------------|
| `APP_ERR_CATEGORY_SUCCESS`  | 0x0 | Success |
| `APP_ERR_CATEGORY_PARAM`    | 0x1 | Parameter error |
| `APP_ERR_CATEGORY_STATE`    | 0x2 | State error |
| `APP_ERR_CATEGORY_TIMEOUT`  | 0x3 | Timeout |
| `APP_ERR_CATEGORY_BUSY`     | 0x4 | Busy / resource occupied |
| `APP_ERR_CATEGORY_NOMEM`    | 0x5 | Out of memory |
| `APP_ERR_CATEGORY_IO`       | 0x6 | I/O error |
| `APP_ERR_CATEGORY_CHECKSUM` | 0x7 | Checksum error |
| `APP_ERR_CATEGORY_PROTOCOL` | 0x8 | Protocol error |
| `APP_ERR_CATEGORY_HARDWARE` | 0x9 | Hardware error |
| `APP_ERR_CATEGORY_INTERNAL` | 0xA | Internal error |

## API Reference

### Type Definition

```c
typedef uint32_t app_err_t;
```

### Utility Macros

| Macro | Description |
|-------|-------------|
| `APP_ERR_SUCCESS(code)` | Check if the code indicates success |
| `APP_ERR_FAILED(code)` | Check if the code indicates failure |
| `APP_ERR_GET_MODULE(code)` | Extract the module identifier |
| `APP_ERR_GET_CATEGORY(code)` | Extract the category identifier |

### Functions

| Function | Description |
|----------|-------------|
| `app_Err_ToString(errCode)` | Get the error code name string |
| `app_Err_GetDescription(errCode)` | Get the error description string |
| `app_Err_IsModule(errCode, module)` | Check if the error belongs to a given module |
| `app_Err_IsCategory(errCode, category)` | Check if the error belongs to a given category |
| `app_Err_IsRecoverable(errCode)` | Check if the error is recoverable |

## Common Error Codes

### System Errors

| Error Code | Value | Description |
|------------|-------|-------------|
| `APP_ERR_OK` | 0x00000000 | Success |
| `APP_ERR_NULL_PTR` | 0x00100101 | Null pointer |
| `APP_ERR_INVALID_PARAM` | 0x00100102 | Invalid parameter |
| `APP_ERR_NOT_INIT` | 0x00200101 | Not initialized |
| `APP_ERR_ALREADY_INIT` | 0x00200102 | Already initialized |
| `APP_ERR_NOT_SUPPORTED` | 0x00100103 | Not supported |
| `APP_ERR_NO_MEMORY` | 0x00500101 | Out of memory |
| `APP_ERR_TIMEOUT` | 0x00300101 | Timeout |
| `APP_ERR_BUSY` | 0x00400101 | Resource busy |
| `APP_ERR_FAIL` | 0x00A00101 | Generic failure |
| `APP_ERR_UNKNOWN` | 0x00A00102 | Unknown error |

### Communication Errors

| Error Code | Description |
|------------|-------------|
| `APP_ERR_COMM_NOT_INIT` | Communication not initialized |
| `APP_ERR_COMM_TX_BUSY` | Transmit busy |
| `APP_ERR_COMM_QUEUE_FULL` | Transmit queue full |
| `APP_ERR_COMM_PAYLOAD_TOO_LARGE` | Payload too large |
| `APP_ERR_COMM_INVALID_UART` | Invalid UART port |
| `APP_ERR_COMM_SEND_FAIL` | Send failed |

### Protocol Errors

| Error Code | Description |
|------------|-------------|
| `APP_ERR_PROTO_FRAME_TIMEOUT` | Frame timeout |
| `APP_ERR_PROTO_CRC_ERROR` | CRC error |
| `APP_ERR_PROTO_INVALID_STATE` | Invalid state |
| `APP_ERR_PROTO_BUFFER_OVERFLOW` | Buffer overflow |
| `APP_ERR_PROTO_INVALID_FRAME` | Invalid frame format |
| `APP_ERR_PROTO_INCOMPLETE_FRAME` | Incomplete frame |
| `APP_ERR_PROTO_ESCAPE_ERROR` | Escape error |

### UART Errors

| Error Code | Description |
|------------|-------------|
| `APP_ERR_UART_NOT_INIT` | UART not initialized |
| `APP_ERR_UART_TX_BUSY` | Transmit busy |
| `APP_ERR_UART_TX_TIMEOUT` | Transmit timeout |
| `APP_ERR_UART_RX_ERROR` | Receive error |
| `APP_ERR_UART_DMA_ERROR` | DMA error |
| `APP_ERR_UART_INVALID_CH` | Invalid channel |
| `APP_ERR_UART_BUFFER_FULL` | Buffer full |

### Sensor Errors

| Error Code | Description |
|------------|-------------|
| `APP_ERR_SENSOR_NOT_RESPONDING` | Sensor not responding |
| `APP_ERR_SENSOR_INVALID_DATA` | Invalid data |
| `APP_ERR_SENSOR_CHECKSUM` | Checksum failed |
| `APP_ERR_SENSOR_NOT_FOUND` | Sensor not found |

## Usage Examples

### Basic Usage

```c
#include "appErrorCode.h"

app_err_t my_function(void *param)
{
    if (param == NULL) {
        return APP_ERR_NULL_PTR;
    }

    if (some_condition) {
        return APP_ERR_TIMEOUT;
    }

    return APP_ERR_OK;
}

// Caller
app_err_t ret = my_function(ptr);
if (APP_ERR_FAILED(ret)) {
    printf("Error: %s - %s\n",
           app_Err_ToString(ret),
           app_Err_GetDescription(ret));
}
```

### Module and Category Inspection

```c
app_err_t ret = some_comm_function();

if (APP_ERR_FAILED(ret)) {
    // Check if it's a communication module error
    if (app_Err_IsModule(ret, APP_ERR_MODULE_COMM)) {
        printf("Communication module error\n");
    }

    // Check if it's a timeout error
    if (app_Err_IsCategory(ret, APP_ERR_CATEGORY_TIMEOUT)) {
        printf("Timeout occurred, retrying...\n");
    }

    // Check if recoverable
    if (app_Err_IsRecoverable(ret)) {
        retry();
    }
}
```

### Custom Error Codes

```c
// Define a custom module and error codes
#define MY_ERR_MODULE 0x09000000U

#define MY_ERR_CUSTOM_FAIL (MY_ERR_MODULE | APP_ERR_CATEGORY_INTERNAL | 0x001)

app_err_t my_custom_function(void)
{
    return MY_ERR_CUSTOM_FAIL;
}
```

## Adding a New Module

Follow this template to add error codes for a new module:

```c
/* In appErrorCode.h */

// 1. Define the module identifier (use an unoccupied module number)
#define APP_ERR_MODULE_MYNEW 0x09000000U

// 2. Define specific error codes
#define APP_ERR_MYNEW_ERROR1 (APP_ERR_MODULE_MYNEW | APP_ERR_CATEGORY_STATE | 0x001)
#define APP_ERR_MYNEW_ERROR2 (APP_ERR_MODULE_MYNEW | APP_ERR_CATEGORY_TIMEOUT | 0x001)

/* In appErrorCode.c — add entries to s_errorTable */
{APP_ERR_MYNEW_ERROR1, "APP_ERR_MYNEW_ERROR1", "Description here"},
{APP_ERR_MYNEW_ERROR2, "APP_ERR_MYNEW_ERROR2", "Description here"},
```

## Recoverable Errors

The following errors are considered recoverable (retry is recommended):

- `APP_ERR_TIMEOUT` — General timeout
- `APP_ERR_BUSY` — Resource busy
- `APP_ERR_COMM_TX_BUSY` — Communication transmit busy
- `APP_ERR_COMM_QUEUE_FULL` — Communication queue full
- `APP_ERR_UART_TX_BUSY` — UART transmit busy
- `APP_ERR_UART_TX_TIMEOUT` — UART transmit timeout
- `APP_ERR_PROTO_FRAME_TIMEOUT` — Protocol frame timeout

## Files

| File | Description |
|------|-------------|
| [appErrorCode.h](appErrorCode.h) | Error code definitions (header) |
| [appErrorCode.c](appErrorCode.c) | Error code implementation |

## License

This project is licensed under the terms found in the [LICENSE](LICENSE) file.
