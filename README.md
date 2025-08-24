# STM32 Text to Morse Code Converter

![STM32 Project](https://img.shields.io/badge/MCU-STM32F103C8T6-blue.svg)
![Language](https://img.shields.io/badge/Language-C-red.svg)
![Framework](https://img.shields.io/badge/Framework-STM32CubeHAL-green.svg)

This project transforms an STM32F103C8T6 "Blue Pill" board into a real-time Morse code generator. Text is sent from a computer via a UART interface, and the STM32 blinks an LED connected to pin `PC15` to represent the corresponding Morse code.

An Arduino UNO is cleverly used as a simple USB-to-Serial (UART) converter, allowing for easy input using the Arduino IDE's built-in Serial Monitor.

## Features

-   **Real-time Conversion:** Instantly converts typed text into visible Morse code.
-   **UART Communication:** Receives text data over a standard serial connection.
-   **Visual Output:** Uses a single LED for clear Morse code representation.
-   **Word Separation:** Correctly implements longer pauses for spaces between words.
-   **Easy to Use:** Leverages the common Arduino Serial Monitor for straightforward text input.

## Hardware Requirements

| Component                          | Quantity | Purpose                                        |
| ---------------------------------- | :------: | ---------------------------------------------- |
| STM32F103C8T6 (Blue Pill) Board    |    1     | The main microcontroller running the logic.    |
| Arduino UNO Board                  |    1     | Used as a USB-to-Serial (UART) adapter.        |
| ST-LINK V2 Programmer              |    1     | To flash the code onto the STM32.              |
| LED (any color)                    |    1     | Visual output for the Morse code.              |
| Breadboard                         |    1     | To assemble the circuit.                       |
| Jumper Wires                       | Several  | To connect the components.                     |

## Software and Tools

-   **STM32CubeIDE:** To compile and flash the firmware onto the STM32.
-   **Arduino IDE:** Used only for its Serial Monitor to send text to the board.
-   **ST-LINK V2 Drivers:** Required for our computer to recognize the programmer.

## Wiring and Connections

There are two parts to the wiring: connecting the STM32 to the Arduino for communication and connecting the LED to the STM32 for output.

### 1. STM32 to Arduino (for UART Communication)

To use the Arduino UNO as a USB-to-Serial adapter, we bypass its main microcontroller.

| STM32 Pin | Arduino UNO Pin |
| :-------: | :-------------: |
|   `PA10` (RX1)   |   `Pin 0` (RX)    |
|    `PA9` (TX1)   |   `Pin 1` (TX)    |
|    `GND`     |      `GND`      |


### 2. STM32 to LED

The LED is connected to the onboard LED pin `PC15` of the Blue Pill board.

| STM32 Pin | Connection                     |
| :-------: | ------------------------------ |
|   `PC15`    | `LED Anode (+)`              |
|    `GND`    | `LED Cathode (-)`            |

## How It Works

1.  The user types a message into the Arduino IDE's Serial Monitor and hits Enter.
2.  The Arduino UNO's onboard USB-to-Serial chip converts the USB data into UART signals and sends it out its TX pin.
3.  The STM32's UART receiver (`PA10`) reads these signals.
4.  The firmware running on the STM32 receives the string into a buffer.
5.  The main loop parses the string character by character.
6.  For each character, the program looks up the corresponding Morse code pattern.
7.  The program then toggles the GPIO pin `PC15` with specific delays (`HAL_Delay`) to blink the connected LED according to the pattern.
8.  A space character is translated into a longer "inter-word" pause.


## Morse Code Timings

The timings for the Morse code signals are configured as follows:

-   **Dot:** 500 ms ON
-   **Dash:** 1500 ms ON
-   **Inter-Character Gap:** 1500 ms OFF (Pause between letters)
-   **Inter-Word Gap:** 3500 ms OFF (Pause for a space character)
