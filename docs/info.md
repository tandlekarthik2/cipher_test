<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This design implements a simple XOR-based stream cipher on a 4-bit input word.
It uses a 2-bit key, which is internally expanded to 4 bits by repeating the two bits: { k1, k0, k1, k0 }

The cipher output is generated as: out[3:0] = data_in[3:0] XOR key_exp[3:0]

Because XOR is a symmetric operation, the same hardware performs both encryption and decryption.

The circuit also includes control and debug signals:

ui_in[6]: Encrypt/Decrypt select (for debug only).

ui_in[7]: Enable signal (also drives the Done flag).

uo_out[7:5]: Debug outputs showing control states.

## How to test

The circuit can be tested in Wokwi or on TinyTapeout silicon.

Steps:

Connect DIP switches to ui_in[7:0]:

ui_in[3:0] → 4-bit Data input.

ui_in[5:4] → 2-bit Key.

ui_in[6] → Encrypt/Decrypt select.

ui_in[7] → Enable.

Connect LEDs to uo_out[7:0]:

uo_out[3:0] → Cipher output (4-bit).

uo_out[4] → Done (lights up when Enable=1).

uo_out[7:5] → Debug outputs.

Set the Data and Key using DIP switches.

Toggle the Enable switch high (1).

Observe the Cipher output on the LEDs.

Verify by applying the same operation again (cipher ⊕ key = original data).

Testcase Truth Table

Inputs → Outputs
| Data (ui\_in\[3:0]) | Key (ui\_in\[5:4]) | Key Expanded (4b) | Enc/Dec (ui\_in\[6]) | Enable (ui\_in\[7]) | Cipher Out (uo\_out\[3:0]) | Done (uo\_out\[4]) | Debug (uo\_out\[7:5]) |
| ------------------- | ------------------ | ----------------- | -------------------- | ------------------- | -------------------------- | ------------------ | --------------------- |
| 0000                | 00                 | 0000              | 0                    | 1                   | 0000                       | 1                  | 010                   |
| 0000                | 01                 | 0101              | 0                    | 1                   | 0101                       | 1                  | 010                   |
| 0000                | 10                 | 1010              | 0                    | 1                   | 1010                       | 1                  | 010                   |
| 0000                | 11                 | 1111              | 0                    | 1                   | 1111                       | 1                  | 010                   |
| 1010                | 01                 | 0101              | 0                    | 1                   | 1111                       | 1                  | 010                   |
| 1010                | 10                 | 1010              | 0                    | 1                   | 0000                       | 1                  | 010                   |
| 1111                | 01                 | 0101              | 0                    | 1                   | 1010                       | 1                  | 010                   |
| 1111                | 10                 | 1010              | 0                    | 1                   | 0101                       | 1                  | 010                   |
| 1111                | 11                 | 1111              | 0                    | 1                   | 0000                       | 1                  | 010                   |


## External hardware

For demonstration on a breadboard or FPGA development board, the following external hardware is suggested:

8-bit DIP switch array → to provide input data, key, and control signals.

8 LEDs (with current-limiting resistors) → to display output bits, status, and debug signals.

Optional: Push-button to reset or step through test vectors.

In simulation (Wokwi), these are represented as virtual DIP switches and an LED bar.
