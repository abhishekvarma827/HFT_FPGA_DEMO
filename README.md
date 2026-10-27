# Low-latency FPGA tick-to-trade: live demo

**Live demo:** https://YOUR-GITHUB-USERNAME.github.io/hft-fpga-demo/
**60-second video:** [demo.mp4](demo.mp4)

A complete tick-to-trade pipeline in SystemVerilog for the AMD Kria KR260 (K26 UltraScale+):
10G XGMII MAC → UDP/IPv4 → MoldUDP64 A/B feed handling with hardware gap re-request →
ITCH 5.0 parser → order book (196,608 orders in UltraRAM) → strategies → pre-trade risk →
OUCH 4.2 → SoupBinTCP over a hardware TCP client.

The demo replays the first 30 seconds of a real NASDAQ session (AAPL, 2019-10-30,
TotalView-ITCH 5.0 public sample) through the pipeline, with each stage's measured latency.

| Metric | Value |
|---|---|
| Wire to wire (XGMII in → order out, idle, median / max) | 294.4 ns / 371.2 ns |
| Wire to wire at full 10G line rate (median) | 344 ns |
| First ITCH byte → risk-approved decision | 75 ns |
| Order book capacity | 196,608 orders |
| Clock domains | 156.25 / 200 / 100 MHz |
| Timing on the real K26 (full board build) | met at 200 MHz, 0 failing endpoints |

Latency comes from cycle-accurate RTL simulation with the board's settings, replaying real NASDAQ
data. The full KR260 board build (10G PCS, ARM, block design) meets timing at 200 MHz on the real
xck26 part. The order book alone has run on the KR260: 4,084,599 real messages, 0 mismatches.
Running the full system on the board is next.
