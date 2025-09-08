## Mini 4-bit XOR Cipher with Barrel Shifter

## 📌 What is this Project?

A 4-bit encryption/decryption circuit built with XOR logic and a 4-bit barrel shifter.

Designed for the TinyTapeout platform (fits within one tile).

Demonstrates basic cryptography in hardware using simple gates and multiplexers.

## 🎯 Why is it Used?

As an educational demo of how encryption can be implemented in digital logic.

To show the principles of confusion (XOR with key) and diffusion (bit rotation).

As a TinyTapeout tapeout candidate for learning ASIC design flows.

## ⚙️ How it Works

Inputs (ui_in):

[3:0] → Data (plaintext when encrypting, ciphertext when decrypting).

[5:4] → 2-bit Key (also used as rotate amount).

[6] → Mode Select (0 = Encrypt, 1 = Decrypt).

[7] → Enable.

Process Flow:

Encryption (enc_dec=0):

Data → MUX → XOR with expanded key {k1,k0,k1,k0}.

Result → Barrel Shifter (rotate left by key value).

MUX → Output (ciphertext).

Decryption (enc_dec=1):

Cipher → Barrel Shifter (rotate right by key value).

Result → MUX → XOR with expanded key.

MUX → Output (plaintext).

Outputs (uo_out):

[3:0] → Result (ciphertext/plaintext).

[4] → Done (mirrors enable).

[7:5] → Debug {enc_dec, key[1], key[0]}.

## 🧾 Example Truth Table
| Data | Key | Mode | XOR Result | Shift | Output |
| ---- | --- | ---- | ---------- | ----- | ------ |
| 1011 | 10  | Enc  | 0001       | L2    | 0100   |
| 0100 | 10  | Dec  | 0001       | R2    | 1011   |
| 1111 | 01  | Enc  | 1010       | L1    | 0101   |
| 0101 | 01  | Dec  | 1010       | R1    | 1111   |

## 🧪 How to Test

Connect switches to inputs (ui_in).

SW0–SW3 → Data

SW4–SW5 → Key

SW6 → Encrypt/Decrypt select

SW7 → Enable

Connect LEDs to outputs (uo_out).

LED0–LED3 → Result (cipher/plaintext)

LED4 → Done flag

LED5–LED7 → Debug outputs

Encrypt: Set Data + Key, mode=0, Enable=1 → observe cipher on LEDs.

Decrypt: Feed cipher as new input, same key, mode=1, Enable=1 → LEDs restore original data.
