<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

The serial_alu_ctrl module works as a serial-input controller for a 7-bit ALU. It receives two 7-bit operands and a 3-bit opcode serially through the Bit_in signal, synchronized with the clock CLK, with data arriving LSB first. A bit counter tracks the number of received bits while internal shift registers store operand A, operand B, and the opcode. After all 17 bits are received, the finite state machine (FSM) moves from the receive state (S_RECV) to the calculation state (S_CALC), where the combinational alu_7b module performs the selected arithmetic or logical operation such as addition, AND, OR, XOR, or subtraction. The result is stored in an 8-bit result register and provided on Data_out, while the Done signal goes high for one clock cycle to indicate that the operation is complete.
## How to test

To test the serial_alu_ctrl module, apply a clock signal and first reset the circuit by making RST_n = 0 for one clock cycle, then set it back to 1. After reset, send the input data serially through Bit_in in LSB-first order. First transmit the 7 bits of operand A, then the 7 bits of operand B, and finally the 3 bits of the opcode. One bit must be applied on every positive edge of CLK. The internal counter and shift registers will capture these bits automatically. After all 17 bits are received, the FSM moves to the calculation state, the ALU performs the selected operation, and the result appears on Data_out. At the same time, the Done signal becomes HIGH for one clock cycle indicating that the output is valid. For example, to test addition, send A = 5 (0000101), B = 3 (0000011), and opcode 000; after the final clock cycles, Data_out should become 00001000 (8).

## External hardware

The project mainly uses the FPGA development board hardware itself and does not require complex external peripherals. The external hardware used includes a clock source for generating the CLK signal, push buttons or switches for providing RST_n and serial Bit_in inputs, and LEDs or a seven-segment display to observe the Data_out result and Done signal. Optionally, a PMOD interface or UART module can be connected for easier serial data transmission and debugging, but the core design works without additional external hardware.
