# dockerfile

Docker environments

In this repo, I provide several docker environments in daily usage. \
The prebuilt images also available on DockerHub.

## [dev_basic](https://hub.docker.com/r/jerryzj/dev_basic)

Based on latest ubuntu LTS image with a bunch of development tools installed.

## [dev_basic_gpu](https://hub.docker.com/r/jerryzj/dev_basic_gpu)

Based on latest cuda image released by nvidia, have the same tools installed as **dev_basic**.

## [riscv](https://hub.docker.com/r/jerryzj/riscv)

Based on dev_basic image, with latest SiFive RISCV toolchain release and latest Spike ISA simulator installed. This image is mainly for NTHU EE3450 course usage.

## [systemc_dev](https://hub.docker.com/r/jerryzj/systemc_dev)

Based on dev_basic image, with latest SystemC, Synopsys SCML library, Andestech RISCV 32-bit toolchain and ArchC simulator installed. This image is mainly for NTHU EE6470 course usage.
