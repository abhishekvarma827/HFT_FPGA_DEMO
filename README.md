# Low-latency FPGA tick-to-trade: live demo

**Live demo:** https://YOUR-GITHUB-USERNAME.github.io/hft-fpga-demo/
**60-second video:** [demo.mp4](demo.mp4)

A complete tick-to-trade pipeline in SystemVerilog for the Xilinx Kria KR260 (UltraScale+):
10G XGMII MAC → UDP/IPv4 → MoldUDP64 A/B feed handling with hardware gap re-request →
ITCH 5.0 parser → order book (196,608 orders in UltraRAM) → strategies → pre-trade risk →
OUCH 4.2 → SoupBinTCP over a hardware TCP client.

The demo replays the first 30 seconds of a real NASDAQ session (AAPL, 2019-10-30,
TotalView-ITCH 5.0 public sample) through the pipeline, with each stage's measured latency.

| Metric | Value |
|---|---|
| Core tick-to-trade (first ITCH byte to decision) | 70 ns |
| Wire to wire (KR260 path, 64-bit order datapath) | 678 ns |
| Order book capacity | 196,608 orders |
| Clock domains | 156.25 / 250 / 100 MHz |

Timings are from cycle-accurate RTL simulation replaying real NASDAQ data. Vivado synthesis
and implementation run on a stand-in UltraScale+ part; KR260 board bring-up is in progress.

## Publishing on GitHub Pages
1. Create a new public repository named `hft-fpga-demo` on GitHub.
2. Upload every file in this folder (`index.html`, `flow_data.json`, `demo.mp4`, `README.md`, `.nojekyll`).
3. Repository **Settings → Pages**: Source = "Deploy from a branch", Branch = `main`, folder `/ (root)`, Save.
4. After about a minute the site is live at `https://YOUR-GITHUB-USERNAME.github.io/hft-fpga-demo/`.
5. Put that link on the resume and replace YOUR-GITHUB-USERNAME above.

## Previewing locally
`python3 -m http.server 8777` in this folder, then open http://localhost:8777/
(opening index.html directly as a file will not load the replay data).
