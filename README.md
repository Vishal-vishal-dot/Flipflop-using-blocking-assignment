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

## Block Diagram:

<img width="413" height="309" alt="image" src="https://github.com/user-attachments/assets/059399a7-0cd5-4e22-a2c4-ca29184813c5" />


## VERILOG CODE

### SR Flip-Flop (Blocking)

```
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

<img width="1205" height="774" alt="Screenshot 2025-09-17 110933" src="https://github.com/user-attachments/assets/ced15ca9-cdb0-4f13-83ea-e0c834a8007c" />



### JK Flip-Flop (Blocking)

```
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

```
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

<img width="1542" height="674" alt="Screenshot 2025-09-17 113132" src="https://github.com/user-attachments/assets/b4521c40-8db2-460f-b295-37c116f5a6ce" />


### D Flip-Flop (Blocking)

```
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

```
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

<img width="1411" height="763" alt="Screenshot 2025-09-17 110534" src="https://github.com/user-attachments/assets/36724df6-5219-44c4-a9ae-d3426d5323f6" />


### T Flip-Flop (Blocking)

```
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

```
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

<img width="1536" height="737" alt="Screenshot 2025-09-17 112130" src="https://github.com/user-attachments/assets/af353f2f-5e81-48a2-9419-ce530c70d90f" />


### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
