## Mini 4-bit XOR Cipher with Barrel Shifter
This project implements a compact encryption and decryption circuit designed for the TinyTapeout platform. The design demonstrates how simple digital logic blocks such as XOR gates, multiplexers, and a barrel shifter can be combined to create a working cipher. The goal is to provide a lightweight, hardware-efficient encryption mechanism that can fit inside the strict area limits of TinyTapeout while still showing the principles of cryptography.

The project is useful in two main ways. First, it acts as an educational tool: it demonstrates how digital logic can be used to build real-world applications such as ciphers and highlights the concepts of confusion (via XOR with a key) and diffusion (via bit rotation). Second, it serves as a TinyTapeout demo project, helping students and beginners understand how to design, test, and tape out their own ASIC circuits in a structured flow.

The working principle is based on a stream cipher model. The input data is 4 bits wide, and the key is 2 bits. The key is expanded internally into four bits {k1, k0, k1, k0} and is also used as the shift amount for the barrel shifter. During encryption, the data is first XORed with the expanded key, and the result is then rotated left by the shift value using the barrel shifter. During decryption, the order is reversed: the cipher text first passes through the barrel shifter, which rotates the word right by the shift value, and the result is then XORed with the expanded key to recover the original plaintext. This works because XOR is symmetric and rotation is reversible, so the same hardware can perform both encryption and decryption depending on the mode selected. Multiplexers are used internally to direct the data path appropriately between encryption and decryption flows.

The input and output ports are assigned in a way that matches TinyTapeout’s standard ui_in and uo_out structure. The 8-bit input bus is divided as follows: ui_in[3:0] carries the 4-bit data, ui_in[5:4] carries the 2-bit key, ui_in[6] is the encrypt/decrypt select (0 for encryption, 1 for decryption), and ui_in[7] is the enable signal. The 8-bit output bus is divided as follows: uo_out[3:0] carries the 4-bit result (ciphertext or plaintext depending on mode), uo_out[4] is the done flag (mirroring enable), and uo_out[7:5] are debug signals that show the mode and key bits for easier testing.

A simple example of the truth table illustrates the functionality. If the data input is 1011 and the key is 10, the key expands to 1010 and sets the shift amount to 2. In encryption mode, the XOR stage produces 1011 XOR 1010 = 0001, and the barrel shifter rotates this left by 2 bits, resulting in 0100. This is the ciphertext. In decryption mode, starting from 0100, the barrel shifter rotates the value right by 2 bits, giving 0001. XORing this with the expanded key again produces 1011, which restores the original plaintext. This symmetry ensures that encryption and decryption are exact inverses.

Testing is straightforward in both simulation (Wokwi) and hardware (TinyTapeout board). DIP switches or virtual switches are connected to the ui_in bus to set the data, key, mode, and enable signals. LEDs are connected to the uo_out bus to display the result, status, and debug values. To test encryption, set the data and key, select encryption mode, and enable the circuit. The LEDs for uo_out[3:0] will show the cipher text. To verify decryption, take this output as the new input data, keep the key the same, switch to decryption mode, and enable again. The LEDs will show the original data, confirming correct operation.

In summary, this project shows how a miniature encryption/decryption engine can be built using just XOR gates, a barrel shifter, and multiplexers. It highlights basic cryptographic principles in a hardware-friendly way and provides a working design that can be simulated in Wokwi or fabricated on a TinyTapeout chip.

##  What is this Project?

A 4-bit encryption/decryption circuit built with XOR logic and a 4-bit barrel shifter.

Designed for the TinyTapeout platform (fits within one tile).

Demonstrates basic cryptography in hardware using simple gates and multiplexers.

##  Why is it Used?

As an educational demo of how encryption can be implemented in digital logic.

To show the principles of confusion (XOR with key) and diffusion (bit rotation).

As a TinyTapeout tapeout candidate for learning ASIC design flows.

##  How it Works

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
