# 6DoF-arm-IK-accelerator

The intention with this project (which is a WIP) is to create a useful IP block in Verilog which will perform inverse kinematics calculations with extremely high throughput for 6 DoF robotics arms.

I aim for the module to have a signature resembling this:
```verilog
module cordic_ik_accelerator #(
    parameter FIXED_WIDTH = 32,         // Fixed-point bit-width
    parameter FRACTIONAL_BITS = 16,     // Fractional bits for fixed-point
    parameter CORDIC_ITERATIONS = 16    // Number of CORDIC iterations
) (
    // Clock and reset
    input  wire clk,                    // Clock signal
    input  wire rst_n,                  // Active-low reset

    // Control signals
    input  wire start,                  // Start computation
    output wire done,                   // Computation complete

    // DH parameters (fixed-point)
    input  wire [FIXED_WIDTH-1:0] alpha0, alpha1, alpha2, alpha3, alpha4, alpha5, // Link twists
    input  wire [FIXED_WIDTH-1:0] a0, a1, a2, a3, a4, a5,                        // Link lengths
    input  wire [FIXED_WIDTH-1:0] d0, d1, d2, d3, d4, d5,                        // Link offsets

    // Target position and rotation (fixed-point)
    input  wire [FIXED_WIDTH-1:0] target_x, target_y, target_z,                  // Target position
    input  wire [FIXED_WIDTH-1:0] target_roll, target_pitch, target_yaw,         // Target rotation

    // Output joint angles (fixed-point)
    output wire [FIXED_WIDTH-1:0] theta0, theta1, theta2, theta3, theta4, theta5 // Joint angles
);
    // Internal logic and pipeline stages go here
endmodule
```

The module will be fully pipelined and use CORDIC algorithms in order to run fast on FPGA.
