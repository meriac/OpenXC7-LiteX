# Containerized LiteX/OpenXC7 Verilog Development Environment for Xilinx FPGAs

## Command Line Reference for Container
After [installation of the development container](../README.md#getting-started) locally or [launching it in the GitHub cloud](https://youtu.be/082bH_Vu2bo), you can use the following environment-specific commands to maintain your environment.
- `litex_update`: Update the current version of [LiteX](https://github.com/enjoy-digital/litex) to the latest version. Restarting your container might discard this update depending on your container configuration.
- `chipdb`: List all supported FPGA chips by this distribution
- `chipdb-full`: List all supported FPGA chips including their speed grades