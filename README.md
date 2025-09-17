# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

Block diagram :


<img width="466" height="500" alt="image" src="https://github.com/user-attachments/assets/8c4f7ab2-9a50-4df7-ba02-60408149321f" />

<img width="419" height="254" alt="image" src="https://github.com/user-attachments/assets/6d64ada6-5783-4358-a768-dd3797effbd4" />

<img width="358" height="136" alt="image" src="https://github.com/user-attachments/assets/51fc99ae-1b89-4463-a0b2-9dab668c253b" />


<img width="300" height="162" alt="image" src="https://github.com/user-attachments/assets/2538d90e-14a5-4e4d-a586-ac6004b3546a" />



## VERILOG CODE

### SR Flip-Flop (Blocking)
```verilog
module srff1(q, s, r, clk, rst);
    input s, r, clk, rst;
    output reg q;

    always @(posedge clk) begin
        if (rst)
            q <= 1'b0;
        else begin
            case ({s, r})
                2'b00: q <= q;
                2'b01: q <= 1'b0;
                2'b10: q <= 1'b1;
                2'b11: q <= 1'bx; // invalid state
                default: q <= 1'bx; // safety default
            endcase
        end
    end
endmodule
```
### SR Flip-Flop Test bench 
```
module srff1_tb;

  reg clk_t, rst_t, s_t, r_t;
  wire q_t;

  
  srff1 dut (
    .clk(clk_t),
    .rst(rst_t),
    .s(s_t),
    .r(r_t),
    .q(q_t)
  );

  
  initial clk_t = 0;
  always #10 clk_t = ~clk_t;

  
  initial begin
    rst_t = 1;
    s_t = 0;
    r_t = 0;

    #20 rst_t = 0;  // Deactivate reset

    #20 s_t = 1; r_t = 1;  
    #20 s_t = 0; r_t = 1;  
    #20 s_t = 1; r_t = 0;  
    #20 s_t = 0; r_t = 0;  

    #40 $finish;
  end

endmodule


```
#### SIMULATION OUTPUT
<img width="1205" height="774" alt="image" src="https://github.com/user-attachments/assets/5769582e-dc50-44df-8ba4-5d840ef438b9" />


### JK Flip-Flop (Blocking)
```verilog
module jkf(j, k, q, clk, rst);
  input j, k, clk, rst;
  output reg q;

  always @(posedge clk) begin
    if (rst)
      q = 1'b0;
    else begin
      case ({j, k})
        2'b00: q = q;
        2'b01: q = 1'b0;
        2'b10: q = 1'b1;
        2'b11: q = ~q;
        default: q = 1'bx;
      endcase
    end
  end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module jkf_tb;
  reg j_t, k_t, clk_t, rst_t;
  wire q_t;

  jkf dut (
    .j(j_t),
    .k(k_t),
    .clk(clk_t),
    .rst(rst_t),
    .q(q_t)
  );

  initial clk_t = 0;
  always #5 clk_t = ~clk_t;

  initial begin
    rst_t = 1;
    j_t = 0;
    k_t = 0;
    #12 rst_t = 0;

    #10 j_t = 0; k_t = 0;
    #10 j_t = 0; k_t = 1;
    #10 j_t = 1; k_t = 0;
    #10 j_t = 1; k_t = 1;
    #10 j_t = 1; k_t = 1;
    #10 j_t = 0; k_t = 0;
    #10 rst_t = 1;
    #10 rst_t = 0;

    #20 $finish;
  end
endmodule


```
#### SIMULATION OUTPUT
<img width="1542" height="674" alt="image" src="https://github.com/user-attachments/assets/ce97981c-bede-433f-8632-68067ea17947" />

### D Flip-Flop (Blocking)
```verilog
module dff(clk, rst, din, dout);
    input clk, rst, din;
    output reg dout;

    always @(posedge clk)
    begin
        if (rst)
            dout = 1'b0;
        else
            dout = din;
    end     
endmodule
```
### D Flip-Flop Test bench 
```verilog
module dff_tb;

  reg clk, rst, din;
  wire dout;

  dff dut (
    .clk(clk),
    .rst(rst),
    .din(din),
    .dout(dout)
  );

  always #5 clk = ~clk;

  initial begin
    clk = 0;
    rst = 1;
    din = 0;

    #10 rst = 0;
    #10 din = 1;
    #10 din = 0;
    #10 din = 1;
    #10 rst = 1;
    #10 rst = 0;
    #10 din = 0;
    #20 $finish;
  end

endmodule


```

#### SIMULATION OUTPUT
<img width="1411" height="763" alt="image" src="https://github.com/user-attachments/assets/68463a88-c45a-4047-903c-f54f30c82897" />

### T Flip-Flop (Blocking)
```verilog
module tff(clk, rst, tin, tout);
  input clk, rst, tin;
  output reg tout;

  always @(posedge clk) begin
    if (rst)
      tout = 1'b0;
    else if (tin)
      tout = ~tout;
    else
      tout = tout;
  end
endmodule
```
### T Flip-Flop Test bench 
```verilog

module tff_tb;
  reg clk_t, rst_t, tin_t;
  wire tout_t;

  tff dut (.clk(clk_t), .rst(rst_t), .tin(tin_t), .tout(tout_t));

  initial begin
    clk_t = 1'b0;
    rst_t = 1'b1;
    tin_t = 1'b0;

    #20 rst_t = 1'b1;

    #20 rst_t = 1'b0;
    tin_t = 1'b1;

    #20 tin_t = 1'b0;

    #40 $finish;  // End simulation
  end

  always #10 clk_t = ~clk_t;
endmodule

```

#### SIMULATION OUTPUT
<img width="1536" height="737" alt="image" src="https://github.com/user-attachments/assets/c9434732-cb47-4ed9-98a0-95fc46de4de7" />

### RESULT
All four flip-flops—SR, D, JK, and T—were successfully simulated in Verilog using blocking assignments. The simulation results aligned with their respective truth tables, confirming that each flip-flop exhibited the expected sequential behavior.
