# STM32F446RE FreeRTOS Serial LED Control

## 1. Project Overview

This project demonstrates a Serial Command-Controlled LED system using the STM32F446RE Nucleo development board and FreeRTOS. The user can control an LED by sending `ON` and `OFF` commands through a Serial Monitor.

The project uses two FreeRTOS tasks for LED control and status monitoring. It demonstrates UART communication, GPIO control, multitasking, message queues, and mutex synchronization.

## 2. Hardware Requirements

- STM32F446RE Nucleo development board
- STM32 expansion board (optional)
- USB cable
- Computer or laptop

## 3. Software Requirements

- STM32CubeIDE
- FreeRTOS (CMSIS-RTOS V2)
- PuTTY or Tera Term Serial Monitor
- STM32 HAL Library

## 4. Step-by-Step Implementation

### Step 1: Create a New Project

1. Open STM32CubeIDE.
2. Select **File → New → STM32 Project**.
3. Search for `NUCLEO-F446RE` in Board Selector.
4. Select the board.
5. Enter the project name `FreeRTOS_Serial_LED`.
6. Click Finish.

### Step 2: Configure GPIO

1. Open the `.ioc` configuration file.
2. Select pin PA5.
3. Configure it as GPIO_Output.
4. Set the user label to `LED`.
5. Set the initial output level to Low.

PA5 is used to control the onboard LD2 LED.

### Step 3: Configure UART

1. Go to Connectivity → USART2.
2. Select Asynchronous mode.
3. Configure the following parameters:

| Parameter | Value |
|---|---|
| Baud Rate | 115200 |
| Word Length | 8 bits |
| Stop Bits | 1 |
| Parity | None |
| TX | PA2 |
| RX | PA3 |

USART2 is used to communicate with the computer through the ST-LINK Virtual COM Port.

### Step 4: Enable FreeRTOS

1. Open Middleware → FreeRTOS.
2. Enable CMSIS-RTOS V2.
3. Configure TIM6 as the HAL time base.
4. Create two tasks.

| Task | Priority | Stack Size |
|---|---|---|
| CommandTask | Above Normal | 512 words |
| StatusTask | Low | 512 words |

### Step 5: Configure Message Queue

Create a FreeRTOS message queue with the following settings:

- Queue Name: `ledQueue`
- Queue Size: 4
- Item Size: `uint8_t`
- Allocation: Dynamic

The queue transfers the LED state from CommandTask to StatusTask.

### Step 6: Create UART Mutex

Create a mutex named `uartMutex`.

The mutex prevents both FreeRTOS tasks from transmitting UART messages simultaneously.

### Step 7: Implement CommandTask

CommandTask receives serial commands from the user.

- `ON` turns the LED ON.
- `OFF` turns the LED OFF.
- Invalid commands generate an error message.
- The updated LED state is sent to the FreeRTOS message queue.

### Step 8: Implement StatusTask

StatusTask periodically monitors the LED state.

1. Receive the latest LED state from the message queue.
2. Check whether the LED is ON or OFF.
3. Display the LED status on the Serial Monitor.
4. Repeat every 2 seconds using `osDelay(2000)`.

### Step 9: Build and Upload

1. Save the project.
2. Click Project → Build Project.
3. Verify that compilation completes without errors.
4. Connect the STM32F446RE board using USB.
5. Upload the firmware using ST-LINK.

### Step 10: Open Serial Monitor

1. Open PuTTY or Tera Term.
2. Select the ST-LINK Virtual COM Port.
3. Set baud rate to 115200.
4. Configure 8 data bits, no parity, and 1 stop bit.
5. Open the serial connection.

### Step 11: Test the Project

Enter the following commands in the Serial Monitor:

| Command | Expected Result |
|---|---|
| `ON` | LED turns ON |
| `OFF` | LED turns OFF |
| Invalid command | Displays Invalid Command |

## 5. Expected Serial Output

```text
Serial LED Control Ready
Enter ON or OFF

Status: LED is OFF

LED ON
Status: LED is ON

LED OFF
Status: LED is OFF
```

## 6. FreeRTOS Concepts Used

- **Multitasking:** Two tasks operate under the FreeRTOS scheduler.
- **UART Communication:** Receives user commands and sends status messages.
- **GPIO Control:** Controls the LED through PA5.
- **Message Queue:** Transfers LED state between tasks.
- **Mutex:** Prevents simultaneous UART transmissions.
- **Task Delay:** Provides periodic status updates.

## 7. Applications

- Embedded system multitasking demonstrations
- UART-based device control
- FreeRTOS learning and experimentation
- GPIO control using serial commands
- Real-time embedded system development

## 8. Conclusion

This project demonstrates serial command-based LED control using the STM32F446RE microcontroller and FreeRTOS. Two tasks are used to process user commands and display the LED status periodically. The project provides practical understanding of UART communication, GPIO control, task scheduling, message queues, and mutex synchronization.
