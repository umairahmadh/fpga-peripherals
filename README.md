# 03 · Peripherals & Interfaces

**Part of [FPGA Journey](https://github.com/YOUR_USERNAME/fpga-journey)**

> The real world of FPGA design: SPI, I2C, FIFOs, AXI-Stream handshaking, VGA output, and a playable Pong game as the capstone demo.

This is where designs stop being exercises and start being things you'd want to show people. Every module here ships with a self-checking testbench. The Pong game is the visual demo that makes your portfolio pop.

---

## 🎯 Learning Objectives

- SPI protocol: CPOL/CPHA modes, shift registers, chip select
- I2C protocol: start/stop conditions, ACK/NACK, clock stretching
- Synchronous and asynchronous (CDC) FIFOs — one of the most-tested interview topics
- AXI4-Stream: the VALID/READY handshake used everywhere in real SoCs
- VGA timing: horizontal and vertical sync, front/back porch, active region
- Clock domain crossing (CDC): the silent killer of FPGA designs
- Integrating multiple modules into a working system

---

## 📁 Structure

```
fpga-03-peripherals/
├── 01_spi/
│   ├── rtl/
│   │   ├── spi_master.sv       # Parametric: CPOL, CPHA, CLK_DIV, WIDTH
│   │   └── spi_slave_model.sv  # Simulation-only slave model
│   ├── tb/
│   │   └── spi_master_tb.sv    # Full-byte transfer, all 4 SPI modes
│   ├── sim/waveforms/
│   ├── Makefile
│   └── README.md               # Protocol explanation + timing diagram
├── 02_i2c/
│   ├── rtl/
│   │   ├── i2c_master.sv       # Start, address, R/W, data, stop, ACK
│   │   └── i2c_slave_model.sv
│   ├── tb/
│   │   └── i2c_master_tb.sv
│   ├── Makefile
│   └── README.md
├── 03_fifo/
│   ├── rtl/
│   │   ├── sync_fifo.sv        # Single clock domain, parametric depth/width
│   │   └── async_fifo.sv       # Gray-code pointer CDC — the classic design
│   ├── tb/
│   │   ├── sync_fifo_tb.sv     # Full/empty/overflow/underflow corner cases
│   │   └── async_fifo_tb.sv    # Multi-clock simulation
│   ├── formal/
│   │   └── sync_fifo_formal.sv # SymbiYosys proof: never overflow/underflow
│   ├── Makefile
│   └── README.md               # Gray code explanation, CDC theory
├── 04_axi_stream/
│   ├── rtl/
│   │   ├── axis_skid_buffer.sv # Proper VALID/READY pipeline register
│   │   └── axis_fifo.sv        # AXI-Stream FIFO
│   ├── tb/
│   │   └── axis_skid_buffer_tb.sv
│   ├── Makefile
│   └── README.md               # VALID/READY handshake patterns
├── 05_vga/
│   ├── rtl/
│   │   ├── vga_timing.sv       # Sync generator (parametric resolution)
│   │   ├── vga_pattern.sv      # Test pattern: color bars, grid, gradient
│   │   └── vga_top.sv
│   ├── tb/
│   │   └── vga_timing_tb.sv    # Checks H/V sync pulse widths
│   ├── sim/waveforms/
│   ├── Makefile
│   └── README.md               # VGA timing explained, resolution table
└── 06_pong/                    ← Capstone demo
    ├── rtl/
    │   ├── pong_top.sv
    │   ├── ball.sv             # Ball position + velocity FSM
    │   ├── paddle.sv           # Paddle movement + boundary
    │   ├── collision.sv        # Ball-wall and ball-paddle detection
    │   ├── score.sv            # BCD score counter
    │   ├── renderer.sv         # Pixel-by-pixel VGA rendering
    │   └── ps2_kbd.sv          # PS/2 keyboard for paddle control
    ├── tb/
    │   ├── ball_tb.sv
    │   └── collision_tb.sv
    ├── docs/
    │   ├── block_diagram.png
    │   └── demo.gif            # Screen recording of the game running
    ├── Makefile
    └── README.md               # Architecture, how to build, how to play
```

---

## ✅ Project Checklist

- [ ] SPI master — all 4 modes (CPOL/CPHA), verified
- [ ] SPI master — loopback test with slave model
- [ ] I2C master — write + read sequence, ACK handling, verified
- [ ] Synchronous FIFO — full/empty flags, corner cases, no false flags
- [ ] **Async FIFO (CDC)** — gray-code pointers, dual-clock simulation ⭐
- [ ] Async FIFO — formal proof (never overflow, never underflow)
- [ ] AXI4-Stream skid buffer — backpressure tested
- [ ] VGA timing generator — 640×480@60Hz, sync widths verified
- [ ] VGA pattern generator — color bars displayed
- [ ] **Pong** — complete game, playable, visually demonstrated

---

## 💡 The Async FIFO is the Interview Star

The gray-code async FIFO is one of the most commonly asked-about designs in FPGA/ASIC interviews. Understanding it deeply (why gray code? what's metastability? how do you prove no pointer aliasing?) is worth more than 10 simpler projects.

Add a `README.md` section explaining the gray code pointer synchronization in your own words. That writeup alone signals real engineering depth.

---

## 🛠️ Tools

| Tool | Use |
|------|-----|
| Verilator + cocotb | Primary simulation + testbenches |
| GTKWave | Waveform viewing |
| SymbiYosys | Formal proof on the FIFO |
| Yosys | Synthesis check (even without hardware) |
| Board (optional) | Any board with VGA/HDMI output for live Pong demo |

---

## 📖 Resources

- [ZipCPU: Building a FIFO](https://zipcpu.com/blog/2017/05/21/formal-to-verify.html)
- [Cummings 2002 — the definitive async FIFO paper](http://www.sunburst-design.com/papers/CummingsSNUG2002SJ_FIFO1.pdf)
- [Project F: VGA timing](https://projectf.io/posts/video-timings-vga-720p-1080p/)
- [AXI4-Stream reference](https://developer.arm.com/documentation/ihi0051)

---

**← Previous:** [02 · HDL / SystemVerilog](https://github.com/YOUR_USERNAME/fpga-02-hdl-sv) &nbsp;|&nbsp; **Next →** [04 · RISC-V CPU](https://github.com/YOUR_USERNAME/fpga-04-riscv-cpu)
