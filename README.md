# LED Button Music Machine

Schematic for a small music box with four lit push buttons, a proximity sensor, songs stored on an SD card, and a speaker driven by a mono amplifier. It is built around an nRF52 board.

The schematic was redrawn from a hand sketch of the breadboard layout. Named net tags (like `BTN1` or `SD_SCK`) stand in for long wires. Every tag connects to all other tags with the same name.

This is the first circuit diagram that I drew, I drew it on work experience roughly between Jan 2022 - Mar 2022

## Files

| File                                                                 | What it is                                                                                                               |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| [LED Button Music Machine.html](LED%20Button%20Music%20Machine.html) | Interactive schematic page with the pin map and wiring checklist. Open it in a browser. It supports light and dark mode. |
| [Circuit.pdf](Circuit.pdf)                                           | One-page printable copy of the schematic (Rev A).                                                                        |

## Parts

- **Microcontroller:** nRF52 board, uses the P0.xx pins
- **Push buttons ×4 (B1–B4):** each has a switch to GND and an LED fed from 3V3 through a 220 Ω resistor
- **Proximity sensor:** IR, runs on 3V3
- **SD card module:** SPI, runs on 3V3
- **Amplifier:** mono class-D, runs on +5V
- **Speaker:** 4–8 Ω
- **Power:** 5 V from USB or DC-in

## Pin map

| Net    | MCU pin             | Connects to                       | Basis   |
| ------ | ------------------- | --------------------------------- | ------- |
| BTN1   | P0.08               | Button 1 switch, other leg to GND | assumed |
| BTN2   | P0.09               | Button 2 switch, other leg to GND | assumed |
| BTN3   | P0.06               | Button 3 switch, other leg to GND | assumed |
| BTN4   | P0.07               | Button 4 switch, other leg to GND | assumed |
| PROX   | P0.13               | Proximity sensor output           | assumed |
| AUDIO  | P0.11               | Amplifier input                   | assumed |
| —      | P0.10               | Spare                             | assumed |
| SD\_\* | SCK, MOSI, MISO, CS | SD card reader (SPI)              | sketch  |
| +5V    | VIN                 | Supply in, amplifier VIN          | sketch  |
| 3V3    | 3V3                 | SD card, prox sensor, button LEDs | sketch  |

"Sketch" means the connection came straight from the hand drawing. "Assumed" means the pin is on the sketch but the exact wire could not be traced. Check those against your code, or swap them to match.

## Check before wiring

1. **Buttons switch to ground.** Set each button pin as an input with the internal pull-up on. It reads LOW when pressed.
2. **LED voltage.** Arcade buttons are often 5 V or 12 V with a resistor already inside. If yours are, drop the 220 Ω and feed them from the matching rail instead of 3V3.
3. **LEDs are always on.** To light a button only when it is pressed or a song plays, give each LED its own GPIO through its resistor.
4. **Prox sensor on 3V3.** Many IR sensors also run on 5 V, but then their output can be 5 V, which can damage a 3.3 V nRF pin.
5. **Amp on 5V.** The amplifier draws the most current, so feed it from +5V, not the board's 3V3 pin.
6. **One shared ground.** The supply, board, sensor, SD card and amp grounds must all connect.
