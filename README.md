---

# Digital Logic with TL-Verilog and Makerchip

[Makerchip](http://makerchip.com/) is an integrated development environment (IDE) that offers advanced Verilog design capabilities. It simplifies circuit design and provides free and instant access to the latest tools for coding, compiling, simulating, and debugging Verilog designs directly from a web browser.

Makerchip supports Transaction-Level Verilog (TL-Verilog), which simplifies coding by eliminating the need to declare variables separately and assign inputs explicitly. TL-Verilog introduces constructs for pipelines and transactions, which can lead to faster development, fewer bugs, easier maintenance, and higher-quality silicon.

## Combinational Logic in TL-Verilog using Makerchip IDE

To understand Makerchip, we began with simple digital logic gates, starting with an inverter. In TL-Verilog, the inverter logic can be expressed as `$out = !$in1`. Notably, variable highlighting in the output is possible.
![1](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/1.PNG?raw=true)
Apart from inverters, we explored other logic gates, multiplexers, and vectors. In TL-Verilog, there's no need to explicitly assign inputs, making the code more concise.
![2](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/2.PNG?raw=true)
## Combinational Calculator

We implemented a combinational calculator capable of performing addition, subtraction, multiplication, and division on two input values. The code, waveform, and diagram for this calculator were provided.
![3](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/3.PNG?raw=true)

## Sequential Calculator

We discussed the Fibonacci series and implemented it using TL-Verilog. We introduced the "ahead" operator (`>>1` and `>>2`) to produce output one or two cycles before the current cycle. We also designed a free-running counter using the ahead operator concept. The sequential calculator takes the output of the previous stage as its input.
![4](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/4.PNG?raw=true)
![5](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/5.PNG?raw=true)
## Pipelining

Pipelining is a crucial feature in TL-Verilog that can be implemented easily with minimal code, reducing the likelihood of bugs. We demonstrated pipelining using the Pythagoras theorem example in both TL-Verilog and SystemVerilog. In TL-Verilog, pipelining is defined using the "|" symbol and stages are indicated with `@1`, `@2`, and so on.
![6](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/6.PNG?raw=true)
## Validity

TL-Verilog introduces the concept of "when" scope, denoted as `?$valid`, for asserting the validity of transactions in a pipeline. This feature simplifies design, enhances debugging, improves error checking, and enables automated clock gating.
![7](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/7.PNG?raw=true)
## Calculator with Memory

We added memory and a recall feature to the two-cycle calculator, enhancing its functionality.
![8](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/8.PNG?raw=true)
![9](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/9.PNG?raw=true)
# Basic RISC-V CPU Microarchitecture

We presented the block diagram of a basic RISC-V microarchitecture. Using Makerchip, we implemented the RISC-V core based on a starter code.
![10](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/10.PNG?raw=true)
## Fetch

During the fetch stage, the processor retrieves instructions from memory at the address pointed to by the program counter (PC).
![11](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/11.PNG?raw=true)
## Decode

Decoding is a critical stage where the processor identifies the instruction type and format. We discussed the six instruction types in RISC-V and performed instruction immediate decoding.
![12](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/12.PNG?raw=true)
![13](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/13.PNG?raw=true)

## Execute and Register File Read/Write

We read from and write into registers, allowing two read and write operations simultaneously. We executed ADDI and ADD instructions, with results stored in the register file.
![14](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/14.PNG?raw=true)
![15](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/15.PNG?raw=true)
![16](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/16.PNG?raw=true)

## Branches

We added branch instructions and updated the PC accordingly. Branch instructions direct the PC to a branch target address based on certain conditions.
![17](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/17.PNG?raw=true)
## Pipelining the RISC-V Microarchitecture

Pipelining the CPU core allows for easy retiming and faster computation.
![18](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/18.PNG?raw=true)

## Load, Store, and Data Memory

We added load and store instructions and incorporated a one-read/one-write data memory. A test bench was used to validate load and store functionality.
![19](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/19.PNG?raw=true)
## Completing the RISC-V CPU

We completed the RISC-V CPU core, including jump instructions, instruction decoding, and ALU for the entire RV32I base integer set.
![20](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/20.PNG?raw=true)

## Combinational logic Examples

### Incrementer
![21](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/21.PNG?raw=true)
```
\m4_TLV_version 1d: tl-x.org
\SV

   // ===========
   // Incrementer
   // ===========

   // An incrementer implemented bit-level using hierarchy for the bit slice.
   // Testbench compares with result of + operator.

   m4_makerchip_module
\TLV
   m4_define(M4_WIDTH, 8)
   
   // The incrementer
   /slice[M4_WIDTH-1:0]
      // Get carry in from previous slice (or 1 into slice 0).
      $carry_in = (#slice == 0) ? 1'b1
                                : /slice[(#slice - 1) % M4_WIDTH]$carry_out;
!     $Value <= *reset ? 1'b0    // reset to zero
                       : $Value ^ $carry_in;
      $carry_out = $Value && $carry_in;
   
   // Combine output bits into a vector.
   $value[M4_WIDTH-1:0] = /slice[*]$Value;
   
   
   // Testbench
   /tb
!     $Value[M4_WIDTH-1:0] <= *reset ? M4_WIDTH'b0 : $Value + M4_WIDTH'b1;
      $error = /top$value != $Value;
   
      // End simulation
!     *failed = ! *reset && $error;
!     *passed = $Value == M4_WIDTH'd30 && *cyc_cnt >= 32'd30;
\SV
   endmodule
```
The module, named m4_makerchip_module, implements this incrementer using a hierarchical approach with bit slices. Each slice calculates a carry-in value, computes the next value, and determines a carry-out value for the next slice. The output bits from all the slices are then combined into a final value, denoted as $value. The associated testbench, defined under /tb, is responsible for comparing the module's output with the result of a simple addition operation and checking for any discrepancies. It calculates an error flag and sets pass/fail conditions based on the correctness of the output. This code is likely a part of a larger hardware design project, and its purpose is to verify the functionality of the incrementer module in a simulated environment.

### Fibonacci Sequence
```
\m4_TLV_version 1d: tl-x.org
\SV
   // A Fibonacci Sequence example.
   // Each cycle we generate a new number in the sequence,
   // where each new value is the sum of the previous two.
   // (1, 1, 2, 3, 5, 8, ...)

   m4_makerchip_module
\TLV
!  $reset = *reset;

   // =========================================
   // Fibonacci.
   $val[15:0] = $reset ? 1 : >>1$val + >>2$val;
   // =========================================

\SV
   endmodule
```
![22](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/22.PNG?raw=true)
The provided code defines a hardware module that generates a Fibonacci sequence. In each cycle, it produces a new number in the sequence, where each new value is the sum of the previous two values. The module has a 16-bit output named `$val`, which represents the current number in the Fibonacci sequence. It initializes to 1 when a reset signal (`$reset`) is active, and after that, it continuously updates by adding the two previous values. This module captures the essence of the Fibonacci sequence in hardware, making it a valuable component for various applications in digital design and data processing.
##Ripple Carry adder

```
\m4_TLV_version 1d: tl-x.org
\SV
   // Based on the design from: http://surf-vhdl.com/how-to-implement-pipeline-multiplier-vhdl/?utm_source=mult-pipe&utm_medium=LK2&utm_campaign=ACLEAD
   // Converted to TL-Verilog with validity added, as well as inline stimulus and checking.
   
   // The logic diagram for this example, from Surf VHDL is (originally) here:
   //   http://surf-vhdl.com/wp/wp-content/uploads/2016/12/Mult-35x35_break_PIPE2.jpg

   m4_makerchip_module
 
\TLV
   |mul
      ?$valid
         
         // Random stimulus
         @0
            m4_rand($aa, 34, 0)
            m4_rand($bb, 34, 0)
         
         
         // 1. A.lower * B.lower (green rectangle in surf-vhdl diagram)
         @1
            $pp1[33:0] = $aa[16:0] * $bb[16:0];
         @2
            $mm1[16:0] = $pp1[16:0];
         
         // 2. A.lower * B.upper (lower purple rectangle in surf-vhdl diagram)
         @2
            $pp2[51:17] = $aa[16:0] * $bb[34:17];
         @3
            $mm2[52:17] = $pp2[51:17] + $pp1[33:17];
         
         // 3. A.upper * B.lower (upper purple rectangle in surf-vhdl diagram)
         @3
            $pp3[51:17] = $aa[34:17] * $bb[16:0];
         @4
            $mm3[52:17] = $pp3[51:17] + $mm2[52:17];
         
         // 4. A.upper * B.upper (orange rectangle in surf-vhdl diagram)
         @4
            $pp4[69:34] = $aa[34:17] * $bb[34:17];
         @5
            $mm4[69:34] = $pp4[69:34] + $mm3[52:34];
         
         // Output
         @6
            $mm[69:0] = {$mm4[69:34], $mm3[33:17], $mm1[16:0]};
         
         
         //---------------------
         // Testbench (inlined)
         
         // Perform full width multiplication and compare with DUT result.
         @7
            $mm_full[69:0] = $aa * $bb;
            // Sticky error flag.
            $Error <= *reset ? 0 : $Error || ($mm_full != $mm);
            
            
            // Assert these to end simulation (before Makerchip cycle limit).
            *passed = *cyc_cnt > 40;
            *failed = $Error;
 
\SV
   endmodule
```
This code defines a hardware module in TL-Verilog for a pipelined multiplier, adapted from an original VHDL design. The module computes the product of two 35-bit numbers and includes validity checks, stimulus generation, and result verification. It follows a pipeline architecture, performing multiple stages of multiplication to achieve high throughput. The individual stages are implemented sequentially, with intermediate values like `$pp1`, `$pp2`, and `$pp3` used to store partial products. The final result, `$mm`, is obtained by combining the partial products. A testbench is also inlined within the module to verify the correctness of the multiplier by comparing the result with a full-width multiplication (`$mm_full`) and tracking errors. The code specifies pass and fail conditions for simulation termination based on cycle count and error flags, making it a self-contained, pipelined multiplier module with validation in TL-Verilog.
![23](https://github.com/Akshay1000101/Akshaya_riscv/blob/main/screenshots/23.PNG?raw=true)
